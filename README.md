# personal-finance-advisor-docs

## Offering
- Track your finances and understand your spending.
- Get insights about spending to track and plan your finances
- Have a financial goal in mind? We got you.

## Solution Design
- User Service (Identity + User Specific + Application Specific Data) MS SQL
- Transaction Service (Bank Statement Upload, Credit Card Bill Upload, Processing the file and storing them after categorizing) Cosmos DB/MS SQL
- Insights & Budgeting Service (Transaction pattern analysis, Spending insights, generating financial insights, tracking budgets, and offering personalized advice) Python with fast API and MongoDB
- Notification Service (Email, SMS) (Distributed architecture, RabbitMQ, Azure App Function)
- API Gateway
- Web App built with react

## Goal: Become a Solution Architect
To practice to reach my goal, I am building a product, that can be useful for everyone.

## Overall Software Architecture
````mermaid
graph TB
    subgraph "Client Layer"
        Web[React Web App]
    end

    subgraph "API Gateway Layer"
        Gateway[API Gateway]
    end

    subgraph "User Service"
        US[".NET Core API"]
        MSSQL[(MS SQL)]
        US --> MSSQL
    end

    subgraph "Transaction Service"
        TS[".NET Core API"]
        FileP[File Processor]
        Cat[Categorization Engine]
        CDB[(Cosmos DB)]
        TS --> FileP
        FileP --> Cat
        Cat --> CDB
    end

    subgraph "Insights Service"
        IS["Python FastAPI"]
        ML[ML Models]
        MongoDB[(MongoDB)]
        IS --> ML
        ML --> MongoDB
    end

    subgraph "Notification Service"
        MQ[RabbitMQ]
        AF1[Azure Function - Email]
        AF2[Azure Function - SMS]
        MQ --> AF1
        MQ --> AF2
    end

    Web --> Gateway
    Gateway --> US
    Gateway --> TS
    Gateway --> IS
    
    TS --> MQ
    IS --> MQ
````

## GitOps-based deployment strategy
````mermaid
flowchart TD
    subgraph GitHub["GitHub Repositories"]
        Dev["Develop Branch"]
        Main["Main Branch"]
    end
    
    subgraph Azure["Azure Resources"]
        subgraph AKS["Azure Kubernetes Service"]
            DevEnv["Dev Environment"]
            ProdEnv["Prod Environment"]
        end
        ACR["Azure Container Registry"]
    end
    
    subgraph ArgoCD["ArgoCD"]
        ArgoDev["ArgoCD Dev Project"]
        ArgoProd["ArgoCD Prod Project"]
    end
    
    Dev -->|Push Trigger| GHADev["GitHub Actions\nDev Pipeline"]
    Main -->|PR Merge Trigger| GHAProd["GitHub Actions\nProd Pipeline"]
    
    GHADev -->|Build & Push| ACR
    GHAProd -->|Build & Push| ACR
    
    ACR -->|Pull Images| DevEnv
    ACR -->|Pull Images| ProdEnv
    
    GHADev -->|Update Manifests| ArgoDev
    GHAProd -->|Update Manifests| ArgoProd
    
    ArgoDev -->|Deploy| DevEnv
    ArgoProd -->|Deploy| ProdEnv
````

## User Service
- Leveraging my existing knowledge of .NET Core, Identity, and Microsoft SQL Service for this service.
- 
