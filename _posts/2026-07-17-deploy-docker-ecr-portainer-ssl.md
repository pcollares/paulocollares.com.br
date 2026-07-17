---
title: 'Deploy de containers Docker na AWS: ECR, Portainer e SSL'
date: 2026-07-17
permalink: /textos/deploy-docker-ecr-portainer-ssl/
categories:
  - textos
excerpt: Como estruturei um fluxo de deploy de containers Docker usando ECR, Portainer e Let's Encrypt em uma EC2.
---

Recentemente precisei estruturar um fluxo de deploy para um serviço containerizado rodando em uma instância EC2 na AWS, já com Docker e Portainer instalados e um domínio apontado via Route 53. Neste texto documento o processo completo: da criação do repositório de imagens até o HTTPS funcionando.

## O cenário

- EC2 Ubuntu com Docker e Portainer já rodando
- Domínio já configurado no Route 53
- Objetivo: subir uma imagem Docker para o Amazon ECR (Elastic Container Registry), fazer o Portainer consumir essa imagem e expor o serviço via HTTPS

## 1. Autenticação AWS

O primeiro passo é garantir que o AWS CLI local tenha credenciais válidas:

```bash
aws sts get-caller-identity
```

Se o comando falhar (erros como `InvalidClientTokenId` ou pendências de configuração SSO), o caminho mais simples para uso individual é criar um usuário IAM dedicado, sem acesso ao console, com uma access key gerada especificamente para linha de comando:

```bash
aws configure
```

Informando `Access Key ID`, `Secret Access Key`, região e formato de saída.

## 2. Criando o repositório no ECR

Com as credenciais configuradas:

```bash
aws ecr create-repository --repository-name meu-servico --region sa-east-1
```

A URL do repositório segue o padrão:

```
<account-id>.dkr.ecr.<regiao>.amazonaws.com/meu-servico
```

## 3. Build e push da imagem

Na pasta do projeto, com o `Dockerfile` já pronto:

```bash
# autenticação no registry
aws ecr get-login-password --region sa-east-1 | \
  docker login --username AWS --password-stdin <account-id>.dkr.ecr.sa-east-1.amazonaws.com

# build
docker build -t meu-servico .

# tag apontando para o ECR
docker tag meu-servico:latest <account-id>.dkr.ecr.sa-east-1.amazonaws.com/meu-servico:latest

# push
docker push <account-id>.dkr.ecr.sa-east-1.amazonaws.com/meu-servico:latest
```

Um detalhe que vale mencionar: depender só da tag `latest` dificulta rastrear qual versão está em produção e complica rollback. Tags mais específicas (SHA do commit, número de versão) resolvem isso sem custo adicional.

## 4. Um usuário IAM só de leitura para o Portainer

Para o Portainer puxar imagens do ECR, não é necessário dar permissão de escrita. A AWS já tem uma policy gerenciada pronta para isso:

```
AmazonEC2ContainerRegistryReadOnly
```

Ela cobre exatamente o necessário — `GetAuthorizationToken`, `BatchGetImage`, `DescribeRepositories`, entre outras — sem permitir push, delete ou alteração de repositórios. Vale criar um usuário IAM exclusivo com essa policy só para as credenciais que o Portainer vai usar, seguindo o princípio de menor privilégio.

## 5. Registrando o ECR no Portainer

No Portainer:

1. **Registries → Add registry**, tipo **AWS ECR**
2. Informar as credenciais do usuário read-only e a região
3. Salvar

O Portainer se encarrega de renovar o token de autenticação do ECR automaticamente (ele expira a cada 12h).

### Subindo o container pela primeira vez

Em **Containers → Add container**, seleciona-se o registry cadastrado, o repositório e a tag desejada. Um ponto de atenção aqui: o campo de imagem espera `repositorio:tag` — a URL do registry já é preenchida automaticamente pelo Portainer a partir do registry selecionado. Tentar digitar a tag isoladamente (ex: só `latest`) gera erro de referência inválida, porque o Portainer concatena o valor ao path do repositório.

Configurações do container:
- **Port mapping:** porta interna do host (ex: `3000`) → porta exposta pela aplicação (ex: `8080`)
- **Restart policy:** `unless-stopped`

Essa política reinicia o container automaticamente após falhas ou reboot da instância, mas **não** o reinicia se ele for parado manualmente — diferente de `always`, que reinicia mesmo após parada intencional.

### Atualizando o container em deploys futuros

Após um novo `docker push`, no Portainer: seleciona-se o container → **Recreate** → marca-se **"Pull latest image"**. Isso refaz o container com a imagem mais recente, mantendo porta, nome e demais configurações.

> Container webhooks (que permitiriam disparar esse recreate via uma chamada HTTP simples, útil em pipelines de CI/CD) fazem parte da Business Edition do Portainer — que, vale registrar, é gratuita para até 3 nós, sem limite de tempo. Uma alternativa sem depender disso é rodar o pull/stop/rm/run diretamente via SSH a partir do pipeline de CI.

## 6. HTTPS com Let's Encrypt

Como o container passa a escutar numa porta interna (não mais diretamente na 80), um reverse proxy na própria instância cuida do SSL.

Instalação:

```bash
sudo apt update
sudo apt install nginx certbot python3-certbot-nginx -y
```

Configuração do site (`/etc/nginx/sites-available/meu-servico`):

```nginx
server {
    listen 80;
    server_name meuservico.exemplo.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/meu-servico /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

Emissão do certificado:

```bash
sudo certbot --nginx -d meuservico.exemplo.com
```

O Certbot detecta a configuração do Nginx, emite o certificado e já configura o redirecionamento HTTP → HTTPS. A renovação automática é instalada junto (via cron ou systemd timer), e pode ser validada com:

```bash
sudo certbot renew --dry-run
```

## Considerações finais

O fluxo inteiro — ECR para armazenar imagens, Portainer como camada de gerenciamento visual dos containers, Nginx + Certbot para TLS — é uma combinação simples de montar e barata de manter, especialmente para quem já tem uma única instância EC2 e não quer (ainda) a complexidade de um orquestrador completo como ECS ou Kubernetes. Para cenários com múltiplos serviços ou necessidade de deploy automatizado via webhook, vale considerar migrar para a Business Edition do Portainer ou integrar o pull/redeploy diretamente no pipeline de CI/CD via SSH.