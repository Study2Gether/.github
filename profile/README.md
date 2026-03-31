# Study2Gether

...

## Architecture Diagram

graph TD
    %% User Interfaces & Access
    subgraph "Frontend & Identity"
        CDN[AWS CloudFront + S3 SPA] -->|HTTPS| UI[Web Client: Student, Admin, Jury]
        UI -->|Sign Up / Sign In| Auth[Amazon Cognito]
    end

    %% Central API
    subgraph "API Gateway"
        UI -->|API Requests with JWT| API[Amazon API Gateway]
        API -.->|Validates Token & Roles| Auth
    end

    %% Distributed Backend Microservices
    subgraph "Distributed Microservices"
        API -->|Role: Student, Admin| S_Team[Team & Proposal Lambda]
        API -->|Role: Student| S_Sub[Submission Lambda]
        API -->|Role: Admin, Jury| S_Grade[Jury Grading Lambda]
    end

    %% Data Layer
    subgraph "Data & Object Layer"
        S_Team <--> DB_Core[(DynamoDB: Teams, Proposals)]
        S_Sub <--> S3_Files[(S3 Bucket: Diagrams, Presentations)]
        S_Grade <--> DB_Core
    end

    %% Operations
    subgraph "Observability & IaC"
        Observability[AWS CloudWatch & X-Ray <br/> Monitoring, Logging, Tracing]
        IaC[Provisioned via Terraform]
    end

    %% Connect Observability to relevant nodes
    S_Team -.-> Observability
    S_Sub -.-> Observability
    S_Grade -.-> Observability
    DB_Core -.-> Observability
    S3_Files -.-> Observability
```
