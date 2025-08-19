---
title: 'Algumas boas práticas de desenvolvimento com Spring Boot'
date: 2025-08-15
permalink: /textos/algumas-boas-praticas-de-desenvolvimento-com-spring/
categories:
  - textos
excerpt: Algumas boas práticas de desenvolvimento com Spring Boot
---

**1. Separar propriedades por ambiente** Evite colocar configurações de produção diretamente no application.properties padrão. Utilize perfis específicos como application-dev.yml, application-prod.yml e application-test.yml para organizar propriedades de acordo com cada ambiente.

**2. Agrupar configurações em classes específicas com @Configuration** Centralize configurações relacionadas em classes dedicadas anotadas com @Configuration, como para banco de dados, segurança, mensageria ou cache. Facilita testes e mantém separação limpa entre serviços.

**3. Manter separação clara entre Controller, Service e Repository**

    **Controller:** lida com chamadas HTTP, validação e delega o trabalho.
    **Service:** encapsula a lógica de negócio e orquestra interações entre repositórios e outros serviços.
    **Repository:** gerencia a persistência no banco de dados.

**4. Manter controllers enxutos e serviços robustos** Controllers devem apenas delegar a lógica; toda lógica de negócio deve ficar nos serviços, garantindo clareza, testabilidade e separação de responsabilidades.

**5. Usar DTOs para requisição, entidade e resposta** Não exponha entidades JPA diretamente nos controllers. Crie DTOs específicos (CreateUserRequest, UserEntity, UserResponse) para proteger camadas, evitar over-fetching, LazyInitializationException e proteger dados sensíveis.

**6. Criar handlers globais de exceção com @ControllerAdvice** Centralize o tratamento de erros usando @ControllerAdvice, retornando respostas consistentes e mapeando exceções para códigos HTTP apropriados.

**7. Usar códigos HTTP significativos** Retorne códigos adequados para cada operação: 201 Created para novos recursos, 204 No Content para exclusões, 400 Bad Request para erros de validação etc. Isso torna a API mais intuitiva.

**8. Prefira injeção via construtor em vez de via campo** Promove imutabilidade, facilita testes, identifica dependências ausentes em tempo de compilação e torna dependências explícitas.

**9. Reduzir uso de classes utilitárias estáticas** Encapsule a lógica em serviços ou beans que podem ser testados isoladamente. Reserve métodos estáticos apenas para funções puramente utilitárias.

**10. Adicionar resiliência com Retry, Circuit Breaker e Rate Limiting** Use @Retryable (Spring Retry), @CircuitBreaker (Resilience4j) e rate limiting (Redis ou Bucket4j) ao chamar APIs externas ou bancos de dados.

**Referências:**

https://medium.com/@sanchitvarshney/ten-architecture-tips-every-spring-boot-developer-should-learn-early-26ce8327f3c4

https://medium.com/@ujjawalr/5-spring-boot-patterns-that-separate-senior-developers-from-juniors-3fdb110d7d30

https://medium.com/javaguides/12-best-practices-every-spring-boot-developer-should-follow-4d678d0d5ed6

https://medium.com/@pudarimadhavi99/advanced-spring-boot-concepts-every-java-developer-should-know-575fe023d4e5
