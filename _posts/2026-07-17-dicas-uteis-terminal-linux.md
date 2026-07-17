---
title: 'Dicas úteis de terminal Linux'
date: 2026-07-17
permalink: /textos/dicas-uteis-terminal-linux/
categories:
  - textos
excerpt: Um apanhado de comandos e atalhos de terminal que economizam tempo no dia a dia.
---

Reuni aqui um conjunto de comandos e atalhos de terminal que uso com frequência. Nada revolucionário, mas são daquelas coisas que, uma vez aprendidas, você não larga mais.

## Limpar swap

Quando o swap está "sujo" e você quer forçar o kernel a liberar e realocar:

```bash
sudo swapoff -a && sudo swapon -a
```

Isso desativa todas as áreas de swap e as reativa em seguida, efetivamente limpando o que estava armazenado ali.

## Alterar grupo de um diretório

```bash
chgrp suporte [diretorio]
```

Troca o grupo dono do diretório informado.

## Listar tamanho de diretórios

```bash
du -hd 1
```

Mostra o tamanho de cada subdiretório (1 nível de profundidade) em formato legível (`-h`).

## top / htop

`top -c` mostra a linha de comando completa do processo, em vez de só o nome do programa.

Atalhos úteis dentro do `top`:

- `e` — altera as unidades na lista de processos
- `E` — altera as unidades no resumo (topo da tela)
- `P` — ordena por uso de processador
- `M` — ordena por uso de memória
- `T` — ordena por tempo de execução
- `espaço` — força atualização imediata

O `htop`, por padrão, mostra cada thread de cada processo separadamente. Para desabilitar isso, adicione a linha abaixo em `~/.config/htop/htoprc`:

```
hide_userland_threads=1
```

## Testar portas

```bash
nc -zv 192.168.0.1 80
nc -zv 192.168.0.1 80-81
```

- `-z` — faz o `nc` apenas escanear por serviços escutando, sem enviar dados de fato
- `-v` — modo verboso

## Listar processo com porta aberta

```bash
lsof -i :portNumber
```

## Detalhes do executável de um PID

```bash
ls -l /proc/1138/exe
```

O link simbólico aponta para o binário em execução daquele processo.

## wget ignorando verificação de certificado SSL

```bash
wget --no-check-certificate [url]
```

Útil em ambientes de teste com certificados autoassinados — evite em produção.

## Nano

Algumas flags que deixam o `nano` mais confortável:

- `-m` — habilita suporte a mouse
- `-q` — esconde a barra de rolagem
- `-l` — mostra números de linha
- `-t` — autosave
- `-B` — mantém uma versão de backup do arquivo original

## Reexecutar o último comando com sudo

```bash
sudo !!
```

Repete o último comando digitado, agora com privilégios de superusuário. Clássico para quando você esquece o `sudo`.

## Busca reversa no histórico

`Ctrl + R` inicia a busca reversa; `Esc` edita o comando encontrado; `Ctrl + R` novamente rotaciona entre as ocorrências anteriores.

Para tornar essa busca realmente útil, vale aumentar o tamanho do histórico. Adicione ao `.bashrc` ou `.zshrc`:

```bash
HISTSIZE=10000
HISTFILESIZE=20000
HISTCONTROL=ignoredups:erasedups
```

Assim o histórico guarda até 10.000 comandos e não fica poluído com duplicatas.

## Corrigir erros de digitação instantaneamente

Digitou um comando com typo, tipo:

```bash
git chekcout main
```

Em vez de subir com a seta e editar manualmente, use a substituição `^antigo^novo`:

```bash
^chekcout^checkout
```

Isso troca `chekcout` por `checkout` no último comando e já executa.

## Diversos úteis

```bash
cd -
```

Volta para o último diretório em que você estava.

## Rodar processos em background

Está rodando algo demorado — um build, uma suíte de testes, um servidor — e não quer abrir outra aba? Adicione `&` ao final do comando:

```bash
npm run build &
```

Para trazer de volta ao primeiro plano:

```bash
fg
```

Para ver todos os jobs em background:

```bash
jobs
```

E se você já iniciou um processo longo sem o `&`, dá para suspendê-lo com `Ctrl + Z` e depois retomá-lo em background:

```bash
Ctrl + Z   # suspende
bg         # retoma em background
```

## Atalhos de linha de comando

| Atalho     | Ação                                      |
|------------|--------------------------------------------|
| `Ctrl + U` | Limpa tudo antes do cursor                  |
| `Ctrl + K` | Limpa tudo depois do cursor                 |
| `Ctrl + L` | Limpa a tela (equivalente ao `clear`)       |

