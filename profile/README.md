# Investment Order Platform

Projeto de estudo colaborativo para desenvolvimento de uma plataforma distribuída de processamento de ordens de investimento.

O objetivo não é apenas construir uma aplicação funcional. O objetivo é criar um ambiente onde todos os integrantes desenvolvam experiência prática em **desenvolvimento full stack, arquitetura, cloud, infraestrutura, mensageria, observabilidade, segurança, testes e CI/CD**.

A principal regra do projeto é:

> **Todos devem ser capazes de implementar uma feature de ponta a ponta.**

E a principal regra de aprendizado é:

> **A pessoa que implementa é responsável por investigar e construir. O time existe para ser consultado, não para fazer a implementação por ela.**

---

# 1. Objetivos

O projeto deve proporcionar experiência prática com:

* C# e .NET moderno
* ASP.NET Core
* Angular
* TypeScript
* PostgreSQL
* Dapper
* Docker
* AWS
* Terraform
* GitHub Actions
* Git Flow
* Mensageria
* Event-Driven Architecture
* DDD
* Vertical Slice Architecture
* CQRS
* Outbox Pattern
* Idempotência
* Retry
* Dead Letter Queue
* Saga
* Observabilidade
* OpenTelemetry
* Testes automatizados
* Segurança
* Performance
* CI/CD
* Arquitetura distribuída
* Resiliência
* Engenharia de software

O projeto deve simular problemas encontrados em sistemas reais.

---

# 2. Princípio do projeto

O projeto não será dividido permanentemente da seguinte forma:

```text
Pessoa A -> Backend
Pessoa B -> Angular
Pessoa C -> AWS
Pessoa D -> Terraform
Pessoa E -> Mensageria
```

Esse modelo cria especialistas isolados e faz com que o conhecimento fique concentrado.

Em vez disso, trabalharemos com **features verticais**.

```text
                    FEATURE
                       │
        ┌──────────────┼──────────────┐
        │              │              │
    Angular          .NET           Database
        │              │              │
        └──────────────┼──────────────┘
                       │
                  Mensageria
                       │
                    Worker
                       │
                Observabilidade
                       │
                 Infraestrutura
                       │
                    CI/CD
```

Cada pessoa deve passar pelo fluxo completo.

---

# 3. Full E2E Ownership

Uma feature pertence a uma pessoa.

Essa pessoa é responsável por levar a feature desde a interface até a infraestrutura necessária para que ela funcione.

Por exemplo:

```text
Create Order
│
├── Angular
├── API
├── Domain
├── Database
├── Outbox
├── Event
├── Consumer
├── Validation
├── Tests
├── Observability
├── Docker
├── Terraform
└── CI/CD
```

A implementação pode envolver outras partes do sistema, mas a pessoa responsável deve entender o fluxo completo.

## Exemplo

### FEATURE-001 — Create Order

Responsabilidades:

* criar tela Angular;
* criar formulário;
* implementar validações;
* criar endpoint;
* implementar regras de domínio;
* persistir ordem;
* criar evento de domínio;
* implementar Outbox;
* publicar evento;
* consumir evento;
* processar validação;
* implementar idempotência;
* criar testes;
* adicionar logs;
* adicionar tracing;
* configurar infraestrutura necessária;
* atualizar pipeline;
* documentar decisões arquiteturais.

A feature só está concluída quando o fluxo completo estiver funcionando.

---

# 4. O desenvolvedor como dono da feature

A pessoa responsável pela feature deve:

1. entender o problema;
2. pesquisar as tecnologias necessárias;
3. desenhar uma solução;
4. implementar;
5. testar;
6. observar;
7. investigar problemas;
8. corrigir;
9. documentar;
10. abrir Pull Request.

O objetivo não é que a pessoa saiba tudo antes de começar.

O objetivo é que ela **aprenda aquilo que for necessário para resolver o problema**.

---

# 5. Como pedir ajuda

Não utilizaremos pair programming como modelo padrão.

A pessoa deve tentar resolver o problema primeiro.

Quando encontrar uma dificuldade, deve consultar o time.

Exemplo:

> "Preciso publicar esse evento utilizando SQS. Estou pensando em fazer X porque precisamos garantir idempotência. Alguém vê algum problema nessa abordagem?"

Ou:

> "Estou criando o módulo de autenticação e fiquei em dúvida entre colocar essa regra no Application Layer ou no Domain. Qual seria o trade-off?"

Ou:

> "Preciso criar esse recurso no Terraform e não conheço esse serviço da AWS. Alguém pode me explicar como vocês estruturariam isso?"

A pessoa que está ajudando deve priorizar:

* explicação;
* contexto;
* documentação;
* alternativas;
* trade-offs;
* experiências anteriores.

Não deve simplesmente pegar a tarefa e implementá-la.

---

# 6. Regra de ouro para ajuda

A pergunta não deve ser:

> "Você pode fazer isso para mim?"

Mas sim:

> "Estou tentando fazer isso dessa maneira. Minha dúvida é X. O que você acha?"

O objetivo é transformar dúvidas em aprendizado.

Quem recebeu ajuda continua sendo responsável pela implementação.

---

# 7. Compartilhamento de conhecimento

Depois de resolver um problema relevante, a pessoa deve compartilhar o aprendizado com o time.

Exemplos:

* nova tecnologia utilizada;
* decisão arquitetural;
* problema encontrado;
* solução escolhida;
* erro cometido;
* comportamento inesperado da AWS;
* problema de concorrência;
* comportamento da mensageria;
* configuração de Terraform;
* problema de performance;
* decisão de segurança.

O compartilhamento pode acontecer em:

* reunião;
* documentação;
* ADR;
* Pull Request;
* apresentação curta;
* README;
* issue.

O objetivo é evitar que o conhecimento fique preso em uma única pessoa.

---

# 8. Rotação de conhecimento

As features serão distribuídas de forma que todos tenham contato com diferentes partes do sistema.

Exemplo:

| Sprint | Pessoa A     | Pessoa B     | Pessoa C     | Pessoa D     |
| ------ | ------------ | ------------ | ------------ | ------------ |
| 1      | Create Order | Account      | Position     | Settlement   |
| 2      | Position     | Settlement   | Create Order | Account      |
| 3      | Settlement   | Create Order | Account      | Position     |
| 4      | Account      | Position     | Settlement   | Create Order |

A cada sprint, as pessoas devem trabalhar em uma nova feature ou em uma evolução significativa de uma feature existente.

O objetivo é que ninguém fique conhecido como:

> "a pessoa do Terraform"

ou:

> "a pessoa do Angular".

Todos devem eventualmente ter trabalhado com:

* Angular;
* .NET;
* banco;
* mensageria;
* AWS;
* Terraform;
* Docker;
* CI/CD;
* observabilidade;
* testes;
* segurança;
* arquitetura.

---

# 9. Arquitetura

A aplicação terá inicialmente uma arquitetura distribuída evolutiva.

```text
                    ┌───────────────┐
                    │    Angular    │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   API Gateway │
                    └───────┬───────┘
                            │
                            ▼
                    ┌───────────────┐
                    │   Order API   │
                    │    .NET       │
                    └───────┬───────┘
                            │
                  ┌─────────┴─────────┐
                  │                   │
                  ▼                   ▼
             PostgreSQL          EventBridge
                                      │
                         ┌────────────┼────────────┐
                         ▼            ▼            ▼
                       SQS          SQS          SQS
                         │            │            │
                         ▼            ▼            ▼
                    Validation   Settlement   Position
                      Worker       Worker       Worker
```

A arquitetura deve evoluir conforme os problemas surgirem.

Não devemos criar complexidade apenas para dizer que utilizamos determinada tecnologia.

---

# 10. Vertical Slice Architecture

As features devem ser organizadas verticalmente.

Exemplo:

```text
Features/
└── Orders/
    └── Create/
        ├── CreateOrderCommand.cs
        ├── CreateOrderHandler.cs
        ├── CreateOrderValidator.cs
        ├── CreateOrderEndpoint.cs
        ├── CreateOrderRepository.cs
        └── CreateOrderTests.cs
```

O objetivo é aproximar tudo que pertence à mesma funcionalidade.

---

# 11. Domínio

Entidades principais:

* Account
* Asset
* Order
* Position
* Settlement
* Transaction

Exemplo de estados de uma ordem:

```text
CREATED
   │
   ▼
VALIDATING
   │
   ├──────────────► REJECTED
   │
   ▼
VALIDATED
   │
   ▼
EXECUTING
   │
   ├──────────────► CANCELLED
   │
   ▼
EXECUTED
   │
   ▼
SETTLED
```

---

# 12. Stack

## Frontend

* Angular
* TypeScript
* RxJS
* Angular Signals
* Angular Router
* Angular Material

## Backend

* C#
* .NET 10
* ASP.NET Core
* Dapper
* PostgreSQL
* FluentValidation
* xUnit

## Cloud

* AWS ECS
* AWS ECR
* AWS RDS
* AWS S3
* AWS SQS
* AWS SNS
* AWS EventBridge
* AWS CloudWatch
* AWS Secrets Manager
* AWS IAM
* AWS KMS

## Infraestrutura

* Terraform
* Docker
* Docker Compose

## CI/CD

* GitHub Actions

## Observabilidade

* OpenTelemetry
* Structured Logging
* Metrics
* Distributed Tracing
* CloudWatch

---

# 13. Git Flow

O projeto utilizará obrigatoriamente Git Flow.

```text
main
 │
 └── production

develop
 │
 └── integration
```

Branches:

```text
feature/*
release/*
hotfix/*
```

Exemplo:

```text
feature/create-order
feature/cancel-order
feature/account-management
feature/settlement
```

Fluxo:

```text
develop
   │
   ▼
feature/create-order
   │
   ▼
Pull Request
   │
   ▼
develop
   │
   ▼
release/*
   │
   ▼
main
```

Não é permitido push direto em:

```text
main
develop
```

Todas as alterações devem passar por Pull Request.

---

# 14. Pull Requests

O Pull Request não serve apenas para encontrar bugs.

Ele também é uma ferramenta de aprendizado.

O autor deve conseguir explicar:

* qual problema foi resolvido;
* por que escolheu aquela abordagem;
* quais alternativas foram consideradas;
* quais trade-offs existem;
* como funciona o fluxo E2E;
* como o sistema reage a falhas;
* como foi testado;
* como foi observado;
* como escalar essa solução.

Os revisores devem questionar a solução.

Exemplos:

> Por que essa regra está nesse layer?

> O que acontece se a mensagem for processada duas vezes?

> E se o banco cair?

> E se o consumer morrer depois de processar a mensagem?

> Como saberemos que essa operação falhou?

> Essa solução escala?

> Existe alguma condição de corrida?

> Como essa alteração será provisionada?

O objetivo é desenvolver capacidade de engenharia, não apenas revisar sintaxe.

---

# 15. Outbox Pattern

Eventos importantes não devem depender exclusivamente de uma publicação direta no broker.

Exemplo:

```text
API
 │
 ├── INSERT Order
 │
 └── INSERT OutboxEvent
          │
          ▼
       Database
          │
          ▼
   Outbox Publisher
          │
          ▼
     EventBridge
```

Isso permitirá estudar problemas como:

* atomicidade;
* falha entre banco e broker;
* retry;
* duplicação;
* processamento assíncrono.

---

# 16. Idempotência

O sistema deve assumir que mensagens podem ser processadas mais de uma vez.

Exemplo:

```text
Message A
   │
   ▼
Consumer
   │
   ├── Processa
   │
   └── Falha antes do ACK
          │
          ▼
       Retry
          │
          ▼
     Message A novamente
```

O consumer deve conseguir identificar que a mensagem já foi processada.

---

# 17. Dead Letter Queue

Mensagens que não conseguem ser processadas após determinada quantidade de tentativas devem ser direcionadas para DLQ.

Devemos estudar:

* retry;
* exponential backoff;
* poison messages;
* DLQ;
* reprocessamento;
* observabilidade;
* recuperação.

---

# 18. Saga

Processos que envolvam múltiplos componentes devem permitir estudar transações distribuídas.

Exemplo:

```text
Create Order
     │
     ▼
Validate
     │
     ▼
Execute
     │
     ▼
Settlement
     │
     ▼
Update Position
```

Devemos simular falhas intermediárias e implementar mecanismos de compensação quando fizer sentido.

---

# 19. Observabilidade

Toda feature relevante deve possuir observabilidade.

Devemos conseguir responder:

* O que aconteceu?
* Quando aconteceu?
* Onde aconteceu?
* Qual serviço falhou?
* Qual foi a requisição?
* Qual foi o evento?
* Quanto tempo demorou?
* Quantas vezes houve retry?

Devemos utilizar:

* logs estruturados;
* métricas;
* traces;
* correlation ID;
* distributed tracing.

---

# 20. Testes

Cada feature deve possuir testes adequados ao problema.

### Unitários

Regras de negócio.

### Integração

Banco, mensageria e componentes externos.

### Contract Tests

Contratos entre componentes.

### E2E

Fluxo completo da funcionalidade.

Exemplo:

```text
Angular
   ↓
API
   ↓
Database
   ↓
Outbox
   ↓
Event Bus
   ↓
Consumer
   ↓
Database
```

---

# 21. Terraform

Toda infraestrutura necessária para o projeto deve ser reproduzível.

Exemplo:

```text
Terraform
│
├── VPC
├── ECS
├── RDS
├── SQS
├── SNS
├── EventBridge
├── IAM
├── Secrets Manager
└── CloudWatch
```

A infraestrutura não deve depender de configuração manual.

---

# 22. CI/CD

Pipeline inicial:

```text
Pull Request
     │
     ▼
Build
     │
     ▼
Unit Tests
     │
     ▼
Integration Tests
     │
     ▼
Static Analysis
     │
     ▼
Security Checks
     │
     ▼
Terraform Validate
     │
     ▼
Merge
     │
     ▼
Docker Build
     │
     ▼
Push ECR
     │
     ▼
Deploy Dev
     │
     ▼
Integration/E2E
     │
     ▼
Staging
     │
     ▼
Approval
     │
     ▼
Production
```

---

# 23. Segurança

Devemos estudar e implementar:

* OAuth2/OIDC;
* JWT;
* IAM;
* least privilege;
* Secrets Manager;
* KMS;
* HTTPS;
* segurança de containers;
* segurança de APIs;
* gerenciamento de secrets;
* isolamento de ambientes.

---

# 24. Docker

O ambiente local deve ser reproduzível.

Exemplo:

```text
docker-compose
│
├── API
├── PostgreSQL
├── Message Broker
├── Worker
└── Observability
```

O objetivo é que um novo integrante consiga executar o projeto localmente sem depender de configuração manual complexa.

---

# 25. Performance

Depois que o fluxo principal estiver funcionando, devemos investigar performance.

Ferramentas:

* k6;
* BenchmarkDotNet.

Devemos medir:

* latência;
* throughput;
* concorrência;
* consumo de CPU;
* consumo de memória;
* banco;
* filas;
* processamento de mensagens.

Não devemos otimizar por suposição.

> **Measure first. Optimize second.**

---

# 26. Chaos Engineering

O sistema deve ser quebrado propositalmente.

Exemplos:

* desligar consumer;
* indisponibilizar banco;
* atrasar mensagens;
* duplicar mensagens;
* provocar timeout;
* gerar mensagens inválidas;
* interromper processamento;
* aumentar carga.

Depois devemos observar:

```text
O sistema percebeu?
        ↓
Conseguiu recuperar?
        ↓
Houve perda de dados?
        ↓
Houve duplicação?
        ↓
Conseguimos identificar o problema?
        ↓
Conseguimos recuperar?
```

---

# 27. ADRs

Decisões arquiteturais importantes devem ser documentadas.

Exemplo:

```text
docs/
└── adr/
    ├── 001-event-driven-architecture.md
    ├── 002-outbox-pattern.md
    ├── 003-dapper.md
    └── 004-eventbridge.md
```

Cada ADR deve explicar:

* contexto;
* problema;
* alternativas;
* decisão;
* consequências.

---

# 28. Definition of Done

Uma feature não está pronta simplesmente porque o código funciona localmente.

Uma feature E2E deve, quando aplicável, possuir:

* [ ] Angular implementado
* [ ] API implementada
* [ ] Regras de negócio implementadas
* [ ] Persistência implementada
* [ ] Mensageria implementada
* [ ] Consumer implementado
* [ ] Idempotência
* [ ] Tratamento de erros
* [ ] Testes unitários
* [ ] Testes de integração
* [ ] Teste E2E
* [ ] Logs
* [ ] Métricas
* [ ] Tracing
* [ ] Docker
* [ ] Terraform
* [ ] CI/CD
* [ ] Segurança
* [ ] Documentação
* [ ] ADR, quando necessário
* [ ] Pull Request aprovado

Nem todos os itens serão necessários para absolutamente todas as features, mas a decisão deve ser consciente.

---

# 29. Organização das tarefas

As tarefas devem ser organizadas por **problema de negócio**, e não por tecnologia.

Evitar:

```text
Criar endpoint
Criar tabela
Criar Terraform
Criar componente Angular
Criar consumer
```

Preferir:

```text
FEATURE-001 — Criar Ordem
```

Com os subtópicos:

```text
- Interface
- API
- Domínio
- Persistência
- Evento
- Consumer
- Testes
- Observabilidade
- Infraestrutura
- CI/CD
```

Uma pessoa fica responsável pela feature.

---

# 30. Exemplo de fluxo de desenvolvimento

```text
1. Receber feature
       ↓
2. Entender o problema
       ↓
3. Pesquisar
       ↓
4. Desenhar solução
       ↓
5. Identificar dúvidas
       ↓
6. Consultar o time
       ↓
7. Implementar
       ↓
8. Testar
       ↓
9. Observar
       ↓
10. Corrigir
       ↓
11. Documentar
       ↓
12. Pull Request
       ↓
13. Code Review
       ↓
14. Merge
```

O ponto importante é que **a pessoa continua dona da implementação do início ao fim**.

---

# 31. Como o time deve funcionar

O time não será organizado como uma estrutura de especialistas.

O modelo será:

```text
                 TIME
                  │
      ┌───────────┼───────────┐
      │           │           │
   Feature A   Feature B   Feature C
      │           │           │
   Pessoa A    Pessoa B    Pessoa C
      │           │           │
      └─────── CONSULTAS ─────┘
```

Uma pessoa pode perguntar para outra:

```text
"Como você faria isso?"
```

Mas depois:

```text
Pessoa A
   ↓
entende
   ↓
decide
   ↓
implementa
   ↓
testa
   ↓
explica
```

Isso cria autonomia técnica.

---

# 32. O que não queremos

Não queremos:

> "Essa parte é do fulano."

Não queremos:

> "Só o fulano sabe Terraform."

Não queremos:

> "Só a pessoa X mexe no Angular."

Não queremos:

> "Se der problema na AWS, chama o especialista."

Não queremos:

> "Não sei fazer, então alguém faz para mim."

Queremos:

> "Não sei fazer isso ainda. Vou pesquisar, perguntar ao time e implementar."

---

# 33. Matriz de exposição

Durante o projeto, devemos acompanhar se todos passaram pelas diferentes áreas.

| Pessoa   | Angular | .NET | DB | Mensageria | AWS | Terraform | CI/CD | Observabilidade |
| -------- | ------: | ---: | -: | ---------: | --: | --------: | ----: | --------------: |
| Pessoa A |       ✓ |    ✓ |  ✓ |          ✓ |   ✓ |         ✓ |     ✓ |               ✓ |
| Pessoa B |       ✓ |    ✓ |  ✓ |          ✓ |   ✓ |         ✓ |     ✓ |               ✓ |
| Pessoa C |       ✓ |    ✓ |  ✓ |          ✓ |   ✓ |         ✓ |     ✓ |               ✓ |
| Pessoa D |       ✓ |    ✓ |  ✓ |          ✓ |   ✓ |         ✓ |     ✓ |               ✓ |

O objetivo não é simplesmente marcar um checkbox.

A pessoa deve ter **implementado algo significativo** naquela área.

---

# 34. Progressão de complexidade

O projeto deve evoluir gradualmente.

### Fase 1 — Base

* Angular
* .NET
* PostgreSQL
* Docker
* Git Flow
* testes

### Fase 2 — Arquitetura

* Vertical Slice
* DDD
* CQRS
* eventos

### Fase 3 — Distribuição

* SQS
* EventBridge
* workers
* Outbox
* Idempotência
* DLQ

### Fase 4 — Cloud

* AWS
* Terraform
* ECS
* RDS
* IAM
* Secrets Manager

### Fase 5 — Operação

* OpenTelemetry
* métricas
* logs
* tracing
* dashboards

### Fase 6 — Engenharia avançada

* performance
* concorrência
* chaos engineering
* resiliência
* failure recovery
* segurança avançada

---

# 35. Roadmap

```text
[ ] Definir domínio
[ ] Criar repositório
[ ] Configurar Git Flow
[ ] Configurar Angular
[ ] Configurar .NET
[ ] Configurar PostgreSQL
[ ] Configurar Docker
[ ] Implementar primeira feature E2E
[ ] Criar testes
[ ] Implementar mensageria
[ ] Implementar Outbox
[ ] Implementar Idempotência
[ ] Implementar DLQ
[ ] Criar infraestrutura Terraform
[ ] Deploy AWS
[ ] Criar CI/CD
[ ] Implementar observabilidade
[ ] Implementar autenticação
[ ] Testar performance
[ ] Executar Chaos Engineering
[ ] Documentar arquitetura
```

---

# 36. Critério de sucesso

O projeto será considerado bem-sucedido quando todos os integrantes conseguirem:

* entender a arquitetura;
* criar uma feature Angular;
* criar uma API .NET;
* trabalhar com banco;
* implementar mensageria;
* trabalhar com eventos;
* configurar infraestrutura;
* utilizar Terraform;
* criar pipeline;
* escrever testes;
* investigar problemas;
* utilizar observabilidade;
* discutir decisões arquiteturais;
* fazer deploy;
* diagnosticar falhas;
* explicar os trade-offs da solução.

Mais importante:

> **Todos devem conseguir pegar uma feature desconhecida e descobrir como implementá-la.**

---

# 37. Filosofia

O projeto não existe para provar que sabemos utilizar tecnologias.

Ele existe para desenvolver capacidade de engenharia.

Por isso:

> **Não queremos especialistas isolados. Queremos engenheiros capazes de atravessar todo o sistema.**

E:

> **Não queremos que alguém faça por você. Queremos que você aprenda a fazer.**

O time deve funcionar como uma rede de conhecimento:

```text
Você não precisa saber tudo.

Mas precisa saber:
    ↓
como investigar
    ↓
como perguntar
    ↓
como avaliar respostas
    ↓
como tomar decisões
    ↓
como implementar
    ↓
como testar
    ↓
como observar
    ↓
como corrigir
    ↓
como explicar para outra pessoa
```

---

# 38. Princípio final

> **Build it.**
>
> **Break it.**
>
> **Observe it.**
>
> **Fix it.**
>
> **Explain it.**
>
> **Teach someone else.**

E então faça tudo novamente com uma feature diferente.
