---
title: 'Codecon Summit 2025'
date: 2025-08-19
permalink: /textos/codecon-summit-2025/
categories:
  - textos
excerpt: Mês passado participei da Codecon Summit 2025, conto aqui um pouco da minha experiência
---

Nos dias 18 e 19 de julho de 2025 participei da Codecon Summit 2025, um evento voltado para desenvolvedores, com foco em engenharia de software, programação e segurança. Realizado em Curitiba, Paraná, esta foi a terceira edição do evento. Com mais de 1500 participantes nos dois dias do evento, e contou com mais de 30 palestras e painéis.

Deixo aqui, um resumo dos principais temas e discutidos:

## IA no Mercado de Trabalho

A Inteligência Artificial foi abordada de forma prática: deve ser usada como auxílio, não como substituto. A recomendação é explorar a IA com curiosidade e ceticismo, focando em seu uso para otimizar processos, sem cair em "hypes" sem justificativa real.

## Monorepo vs. Microsserviços 

Foi apresentada a ideia de Monorepo como uma alternativa aos microsserviços, usando modularização para separar responsabilidades e permissões em um único repositório. As vantagens incluem menor custo e maior facilidade de deploy e manutenção. Enquanto microsserviços são úteis para escalar times, não necessariamente aplicações, o monorepo pode oferecer escalabilidade de módulos e manter fronteiras bem definidas. A mensagem principal foi a importância de escalar a complexidade conforme a necessidade, evitando adicionar complexidade acidental sem justificativa clara. A regra é: "Do the Simplest Thing That Could Possibly Work".

> Microsserviços não servem para escalar aplicações, mas para escalar times

## Refatoração

A refatoração deve ser uma prática constante da equipe, e não uma tarefa fixa. Sugere-se que entre 10% e 20% do tempo do desenvolvedor seja dedicado a isso, de forma natural, enquanto se trabalha em novas funcionalidades. Refatorar é essencial para manter o sistema coeso, atualizado e resistente a mudanças.

## Testes

O teste unitário é o seu primeiro usuário e não existe uma boa arquitetura sem testes. A pirâmide de testes (unitário na base, integração no meio, e2e no topo) orienta onde concentrar os esforços, com testes unitários sendo mais "baratos" e importantes para diminuir a dependência do QA. Testes devem ser simples, objetivos, pequenos e fáceis de ler. Um ponto crucial é que "Code Coverage não é tudo, quantidade nem sempre significa qualidade", e testes que falham intermitentemente devem ser corrigidos ou removidos, pois não são confiáveis.

> O teste unitário é o seu primeiro usuário!

## REST APIs

É melhor falhar para alguns usuários do que para todos, enfatizando a importância do conceito "Fail Fast". APIs REST devem ser idempotentes, ou seja, chamar o mesmo endpoint várias vezes deve produzir o mesmo comportamento. Endpoints de escrita não devem ter efeitos colaterais de inserir o mesmo dado duas vezes, e as operações devem ser "tudo ou nada" para evitar inconsistências.
Aprendizagem e Carreira

A aprendizagem se divide em duas formas: difusa (conhecimentos adquiridos em podcasts, eventos, conversas, palestras) e focada (conhecimento formal, cursos, estudo pessoal direcionado). "Aprender dói, tem que doer", pois o crescimento ocorre quando saímos da zona de conforto. A mensagem final para a carreira é comece agora, não esperar o momento certo, pois "Feito é melhor do que bem feito!". E lembre-se: "Nem tudo dá certo, mas o que importa é tentar. Falhe rápido, mas não deixe de tentar".

> Nem tudo dá certo, mas o que importa é tentar. Falhe rápido, mas não deixe de tentar.

Foi um evento incrível, quero poder voltar nas próximas edições.