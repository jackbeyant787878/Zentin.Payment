# Zenith Pay Platform \| Enterprise\-Level Payment Platform Architecture Based on \.NET 10 \+ DDD \+ Microservice \+ Cloud Native

## 1\. System Architecture Overview

**Full interactive architecture diagram \(with service flow \& component description\)**: [View Complete Architecture Diagram](https://jackbeyant787878.github.io/Zentin.Payment/index.html)

**Quick Preview**

\!\[Architecture Screenshot\]\(https://your\-image\-cdn\-url/architecture\-preview\.png\)

## 2\. Project Background

Zenith Pay Platform is a brand\-new, enterprise\-grade cloud\-native aggregated payment microservice platform\. The project only references the complete business scenarios and design concepts of the open\-source project AgPayPlus, covering core full\-link payment capabilities including merchant management, multi\-channel payment transactions, refund processing, split accounting and settlement, fund transfer, asynchronous transaction notification, and transaction reconciliation\.

This project is **not a code migration, version upgrade, or reconstruction of old projects**\. It abandons the traditional three\-tier architecture, business coupling, simplistic data\-driven modeling, and fragmented architecture of legacy payment systems\. Driven by real financial payment business scenarios, the platform is completely built from scratch based on the **\.NET 10 technology stack**, implementing standardized DDD domain modeling, microservice boundary division, event\-driven architecture, and full cloud\-native construction\. It delivers a standardized, commercial\-grade, highly scalable, and technical\-debt\-free modern enterprise payment middle platform\.

## 3\. Core Project Objectives

### 2\.1 Business Objective: Standard Domain Modeling Based on Real Payment Scenarios

The payment system features complex business rules, long order lifecycles, frequent state transitions, multi\-channel access, rigorous fund workflows, and strong asynchronous dependencies\. Traditional database\-table\-driven development cannot meet the iteration requirements and fund consistency constraints of complex payment businesses\.

This project strictly follows **Domain\-Driven Design \(DDD\)**, focusing on business domains rather than database structures\. It completes professional business modeling through entities, value objects, aggregate roots, domain services, and domain events to fit the essence of payment business\. The core domain models are designed as follows:

- **Payment\.Domain \(Core Payment Domain\)**: Contains payment orders, transaction records, payment channels, payment aggregate roots, order state machines, and core payment domain events\.

- Strictly distinguishes **Entity, Value Object, Aggregate Root, Domain Service, and Domain Event** to achieve cohesive business logic, clear domain boundaries, and controllable state transitions\.

### 2\.2 Architecture Objective: Build Cloud\-Native Microservice System

The project abandons the monolithic architecture and adopts a **Cloud\-Native Microservice Architecture**\. It achieves service decoupling, unified governance, and event\-driven workflow based on enterprise\-grade infrastructure, featuring layered clarity, single responsibility, independent scaling, and seamless iteration\.

**Overall Architecture Topology**

Client Request → YARP API Gateway \(Public Key Signature Verification, Permission Authentication, Traffic Governance\) → Business Microservice Cluster → RabbitMQ Event Bus \(Asynchronous Business Processing\)

Core business microservices: **Merchant Service, Agent Profit\-sharing Service, Platform Management Service, Payment Core Service**\. All business services are completely unaware of authentication logic, with full traffic control and security verification implemented at the gateway layer\.

## 4\. Enterprise\-Grade Technical Infrastructure \(Fully Implemented\)

This project adopts a **production\-grade gateway\-centric security architecture**\. Different from inefficient traditional link modes, the Identity Service Center only acts as a static token issuer\. All request authentication, signature verification, and permission validation are implemented statelessly at the YARP gateway\. Business services completely eliminate authentication logic, ensuring high performance, security, and decoupling of the request link\.

### 3\.1 Identity Service Center

- **Tech Stack**: OpenIddict

- **Core Positioning**: Independent identity token issuance center, **only responsible for JWT token signing and issuance**, not involved in any request link verification\.

- **Capabilities**: OAuth2, OpenID Connect, JWT private\-key signature issuance, client management, role permission configuration, unified identity key management\.

### 3\.2 API Gateway \(YARP\)

- **Tech Stack**: YARP High\-Performance \.NET Reverse Proxy Gateway

- **Core Positioning**: Unified system traffic entrance, **undertakes full\-link security verification and traffic governance**\.

- **Production\-Grade Capabilities**: Request routing, dynamic service forwarding, **JWT public\-key local signature verification, stateless authentication, interface signature verification, decoupled key compatibility**, traffic rate limiting, service aggregation, full\-link request interception\.

### 3\.3 Consul Service Discovery \& Configuration Center

- **Core Capabilities**: Microservice automatic registration, dynamic service discovery, health check, distributed KV unified configuration management\.

- **Value**: Supports dynamic scaling, automatic fault elimination, and global unified configuration management, ensuring the stability of distributed services\.

### 3\.4 SchedulerJob Distributed Task Service

- **Tech Stack**: Quartz\.NET

- **Core Positioning**: Unified background task scheduling center, undertaking all asynchronous and scheduled tasks in the payment domain\.

- **Business Scenarios**: Close overdue unpaid orders, asynchronous payment status synchronization, automatic reconciliation and settlement, business exception retry and compensation, periodic fund clearing\.

### 3\.5 Self\-Developed General NuGet Components \(Published on NuGet\.org\)

Provides unified standardized underlying capabilities for all microservices with consistent global specifications and out\-of\-the\-box usage:

- **Zenith\.Extensions\.Consul**: Encapsulates service registration, discovery, and configuration center capabilities\.

- **Zenith\.Extensions\.Rabbitmq**: Standardizes event bus, message publish/subscribe, dead\-letter queue, and message idempotency encapsulation\.

- **Zenith\.Extensions\.Redis**: Encapsulates distributed cache, distributed lock, current limiter, and hot data cache\.

- **Zenith\.Extensions\.Elasticsearch**: Implements structured log collection, retrieval, and full\-link tracing\.

## 5\. Business Domain Boundary Planning \(DDD Reconstruction Based on Legacy Business Modules\)

The legacy AgPayPlus backend contains four core business APIs: `agpay-manager-api`, `agpay-agent-api`, `agpay-merchant-api`, `agpay-payment-api`\. This project completely inherits the mature original business domain boundaries, **reconstructs and models from scratch based on \.NET 10 DDD specifications**, eliminates the fragmented architecture and coupled code of the old system, and implements standardized architecture upgrading on the basis of original complete business capabilities\.

- **ManagerService \(Platform Management Domain, aligned with agpay\-manager\-api\)**: Platform backend management, system configuration, payment channel management, payment parameter configuration, global order management, and risk control configuration\.

- **AgentService \(Agent Profit\-Sharing Domain, aligned with agpay\-agent\-api\)**: Agent hierarchy management, profit\-sharing rule configuration, agent income statistics, automatic split settlement, and agent fund accounting\.

- **MerchantService \(Merchant Domain, aligned with agpay\-merchant\-api\)**: Merchant settlement, merchant profile management, application access management, merchant rate configuration, merchant permission and data statistics\.

- **PaymentService \(Core Payment Domain, aligned with agpay\-payment\-api\)**: Payment order creation, cashier interaction, channel callback processing, order state machine management, refund processing, transaction record management, and payment notification callback\.

## 6\. Core Architecture Design Concepts

### 5\.1 DDD Layered Clean Architecture

All microservices adopt a unified four\-layer clean architecture\. **Business services are completely unaware of authentication and security logic**, and the domain layer has zero external dependencies, realizing complete decoupling between business and technical implementation:

- **Domain Layer**: Core business models, business rules, domain events, and specifications \(pure business logic without technical dependencies\)\.

- **Application Layer**: CQRS command/query scheduling, business process orchestration, and domain event consumption\.

- **Infrastructure Layer**: Technical implementations including database persistence, cache, message queue, third\-party channel integration, and task scheduling\.

- **Presentation Layer**: Minimalist API controllers, only responsible for request receiving and response returning\.

### 5\.2 Event\-Driven Microservice Architecture

Implements cross\-service event bus based on RabbitMQ, achieving thorough service decoupling through domain events without hard\-coded service dependencies\.

**Typical Scenario: Payment Success Workflow**

PaymentService publishes `PaymentSucceededEvent` → RabbitMQ Event Bus distribution → Downstream services consume events asynchronously to complete business iteration, realizing decoupled, parallel, and high\-availability business workflow\.

## 7\. Cloud\-Native \& DevOps System Construction

### 6\.1 Containerized Deployment

All microservices support independent Docker packaging and stateless deployment, realizing service isolation, version controllability, and consistent deployment environment\.

### 6\.2 Fully Automated CI/CD \(GitHub Actions\)

Implements full automated delivery pipeline: Code Push → Auto Build → Unit Test → Docker Image Packaging → Cluster Automatic Deployment, realizing fully unmanned enterprise\-level DevOps workflow\.

### 6\.3 Kubernetes\-Evolvable Architecture

Currently stably deployed on lightweight K3s clusters, fully compatible with standard Kubernetes\. It can seamlessly evolve capabilities such as automatic scaling, rolling upgrade, gray release, cluster monitoring, and configuration hot update\.

## 8\. Project Core Value

### 7\.1 Technical Value

Builds a **\.NET 10 enterprise\-grade full\-architecture benchmark project**, fully implementing enterprise technical systems including DDD, Clean Architecture, CQRS, Event\-Driven Architecture, Microservice, Cloud\-Native, and DevOps\. All implementations follow production standards without simplified demo logic\.

### 7\.2 Business Value

Forms a scalable and commercial\-grade standardized payment platform base, supporting multi\-merchant, multi\-payment\-channel, multi\-level profit\-sharing, and automatic reconciliation settlement\. It supports SaaS multi\-tenant extension and rapid iteration of various payment operation scenarios\.

### 7\.3 Learning \& Interview Value

Different from ordinary CRUD and legacy reconstruction projects, this project is a **complete enterprise\-grade architecture practice built from scratch**\. It fully accumulates core capabilities including advanced \.NET 10 development, DDD domain modeling, microservice governance, distributed high\-concurrency processing, cloud\-native delivery, and event\-driven architecture, serving as a high\-gold\-content practical project for senior \.NET backend and architecture positions\.

## 9\. Complete DDD Engineering Architecture of Payment Business

### 8\.1 Architecture Design Description

The core payment business system is built from scratch based on \.NET 10, adopting **standard DDD Clean Architecture \+ CQRS Read\-Write Separation**\. It strictly follows the principle of "domain layer zero external dependencies and business\-technology decoupling"\. The entire business system covers full\-link capabilities of payment, merchant, refund, reconciliation, accounting, and notification, unified in one solution with standardized DDD engineering specifications, adapting to high\-concurrency payment transactions, complex fund workflows, state machine driving, and event\-driven scenarios\.

### 8\.2 Overall Solution Structure

The solution completely covers the four major business domains of the legacy project with standardized DDD reconstruction:

```Plain Text
Zenith.Payment.Platform.sln
├── ManagerService         # Align with agpay-manager-api (Platform Management Domain)
├── AgentService           # Align with agpay-agent-api (Agent Profit-Sharing Domain)
├── MerchantService        # Align with agpay-merchant-api (Merchant Domain)
└── PaymentService         # Align with agpay-payment-api (Core Payment Domain: Cashier, Refund, Callback, Notification)

```

### 8\.3 Standard DDD Four\-Layer Specification for Single Service

All business services in the solution follow a unified DDD engineering structure\. Taking the core **PaymentService** as an example:

```Plain Text
PaymentService/
├── PaymentService.Presentation          # Presentation Layer
│   └── Controllers                      # Minimalist API Controllers (Request Forwarding & Response Uniformization)
│
├── PaymentService.Application           # Application Layer (Business Orchestration & CQRS Scheduling)
│   ├── Commands                         # Write Commands: Order creation, payment, refund, status change, fund operation
│   ├── Queries                          # Read Queries: Order query, transaction statistics, reconciliation data, fund flow query
│   ├── EventHandlers                    # Domain event consumer, driving cross-domain business workflow
│   ├── Dtos                             # Business input/output DTO & model mapping
│   └── ApplicationServices              # Application business assembly & transaction orchestration
│
├── PaymentService.Domain                # Domain Layer (Core, Pure Business, Zero Dependencies)
│   ├── Aggregates                       # Aggregate Roots (Domain Boundary Core)
│   │   ├── PaymentOrder                 # Payment Order Aggregate
│   │   ├── PaymentTransaction           # Transaction Flow Aggregate
│   │   └── RefundRecord                 # Refund Record Aggregate
│   ├── Entities                         # Domain sub-entities
│   ├── ValueObjects                     # Immutable business objects: Amount, Channel, Status, Rate
│   ├── DomainEvents                     # Domain event definition: PaymentSucceeded, PaymentFailed, RefundCompleted, OrderTimeout
│   ├── Specifications                   # Domain business rule validation
│   └── DomainServices                   # Pure domain business logic & core algorithm
│
├── PaymentService.Infrastructure        # Infrastructure Layer (Technical Implementation)
│   ├── Data                             # EF Core, DbContext, Unit of Work, Repository
│   ├── Caching                          # Redis cache, distributed lock, hot data optimization
│   ├── MessageBus                       # RabbitMQ event bus reliable delivery
│   ├── ExternalChannel                  # Third-party payment channel integration (WeChat/Alipay)
│   ├── Configuration                    # Consul configuration reading & service registration
│   ├── Logging                          # Structured logging & ELK collection
│   └── JobIntegration                   # Quartz task docking for order timeout & exception compensation
│
├── PaymentService.Shared                # Service Shared Layer
│   ├── Constants                        # Business constants
│   ├── Enums                            # Business state enumerations
│   ├── Exceptions                       # Custom domain & business exceptions
│   └── Mappers                          # Entity & DTO object mapping
│
├── PaymentService.Tests                 # Unit Test (Domain rule & state machine verification)
├── Dockerfile                           # Independent container deployment
└── appsettings.json                     # Service configuration (Consul hot update supported)

```

### 8\.4 Core DDD Design Advantages

- **Pure Domain\-Driven Modeling**: Business\-centric design instead of data\-driven, ensuring fund transaction consistency and strict domain boundary control via aggregate roots\.

- **Strict CQRS Read\-Write Separation**: Separates high\-concurrency transaction writing and high\-frequency order query, resolving performance conflicts in payment scenarios\.

- **Event\-Driven Decoupling**: Core business actions trigger domain events to drive downstream services, realizing complete decoupling between microservices\.

- **Pure Business Domain Layer**: Core fund rules, state machines and business logic are completely decoupled from middleware and databases, achieving high cohesion and testability\.

- **Single Responsibility Layered Architecture**: Presentation for forwarding, Application for orchestration, Domain for business rules, Infrastructure for technical implementation, fully compliant with enterprise clean architecture standards\.

### 8\.5 Core Responsibilities of Four Domain Services

- **ManagerService \(Platform Management Domain\)**: Platform\-level configuration, payment channel management, risk control parameters, global order management, and system operation \& maintenance capabilities\.

- **AgentService \(Agent Profit\-Sharing Domain\)**: Agent hierarchy system, profit\-sharing rules, automatic split accounting, agent settlement, and revenue statistics\.

- **MerchantService \(Merchant Domain\)**: Merchant settlement, application access, rate configuration, merchant permission management, and business data statistics\.

- **PaymentService \(Core Payment Domain\)**: Order creation, channel payment processing, callback parsing, order state machine iteration, refund business, and payment notification scheduling\.

## 10\. Project Positioning \(One\-Sentence Summary\)

**Zenith Pay Platform is a completely redesigned and implemented enterprise payment architecture practice based on \.NET 10, DDD, microservices and cloud\-native technology system, referencing the business model of AgPayPlus aggregated payment\. Adopting production\-grade gateway security architecture with YARP as unified traffic and security entrance and OpenIddict for exclusive token issuance, the project combines Consul service governance, Quartz\.NET task scheduling and self\-developed Zenith\.Extensions general components as standardized technical foundations, exploring modern \.NET enterprise distributed system security design, domain modeling and production\-level landing specifications\.**


