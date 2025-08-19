---
title: 'Quatro mitos da engenharia de software'
date: 2025-08-13
permalink: /textos/quatro-mitos-da-engenharia-de-software/
categories:
  - textos
excerpt: Alguns mitos da engenharia de software
---

 Texto baseado no episódio de mesmo nome do podcast **Tech Leadership Rocks** do [Eduardo Matos](/in/eduardodematos/): [https://escolaforja.com.br/podcast/tech-leadership-rocks/4-mitos-em-engenharia-de-software-com-edu-matos](https://escolaforja.com.br/podcast/tech-leadership-rocks/4-mitos-em-engenharia-de-software-com-edu-matos)

1.  **Linguagem ‘X’ é lenta** – O desempenho real depende mais do contexto (CPU-bound vs. I/O-bound) do que da velocidade intrínseca da linguagem. Aplicações web são majoritariamente I/O-bound, e otimizar operações de I/O traz ganhos muito maiores do que trocar de linguagem.
2.  **Microsserviços são sempre melhores (ou piores) que monolitos** – Nenhuma abordagem é universal. Monolitos simplificam o início do projeto e aceleram entregas; microsserviços fazem sentido quando há dores reais. Mais importante que adotar uma determinada arquitetura, é escrever um código bem estruturado, com componentes isolados e contratos claros.
3.  **Bancos de Dados SQL não escalam** – Sim, porém com otimizações adequadas. Leitura pode ser otimizada com índices, réplicas, cache e desnormalização; escrita com batching, transações otimizadas e filas assíncronas para diluir carga.
4.  **Trunk-Based Development é a solução definitiva** – Entregas pequenas reduzem riscos, mas o uso excessivo de feature flags pode gerar código complexo e funcionalidades fragmentadas. O foco deve ser entregar valor em incrementos pequenos, não apenas código.

### Bônus:

Alguns mitos que considero importante discutir:

*   **Quanto mais código, melhor o software** \- Mensurar produtividade com linhas de código pode ter sido uma métrica do passado, hoje, entendemos que um código limpo, e fácil de manter torna o software mais seguro.
*   **Adicionar mais programadores acelera qualquer projeto** \- A clássica lei de Brooks, adicionar mais programadores em um projeto atrasado aumenta a interação entre os participantes, diminuindo a eficácia a curto prazo.
*   **Automatizar testes elimina bugs** \- Automatizar testes previne que os mesmos bugs reapareçam, não é uma ferramenta mágica que descobre os bugs.
*   **Software bom não precisa de manutenção** \- Software degrada sim, otimizações são necessárias para suprir novas demandas de acesso, novas vulnerabilidades de segurança são descobertas e exploradas, etc.
*   **Entregar rápido é mais importante que clareza** \- um código limpo, fácil de entender e manter é possível realizar melhorias futuras, um código entregue rápido sem responsabilidades definidas, com alto acoplamento e sem testes, torna quase impossível de manter.
*   **O trabalho do programador é só escrever código** \- Parte importante do nosso trabalho é conversar com os pares e o cliente, para extrair do primeiro grupo melhores práticas para cada implementação e com o segundo a real necessidade dele e saber interpretar e transpor isso para um bom produto, que não necessariamente é o que ele quer, mas o que ele precisa para resolver seu problema.

**Quais mitos você ainda ouve por aí?**