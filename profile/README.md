# 💘 CloudCrushHHN - Cloud Computing Competition (CCC’26)

**CloudCrushHHN** is the exclusive dating and networking app for students of Heilbronn University (HHN). Our goal: Bringing students together across different study programs, whether for a date in the campus cafeteria or a shared late-night study session in the LIV library.

This project was developed as part of the **Cloud Computing Competition (CCC’26)** and demonstrates a highly scalable, 100% serverless microservices architecture.

## 🏗 Architecture & Tech Stack

Our architecture follows the "Serverless First" principle to guarantee massive scalability and reduce idle costs to almost €0.00.

* **Frontend & UX:** React (Progressive Web App / Mobile-First)
* **Hosting:** AWS S3 + Amazon CloudFront (CDN)
* **Identity & Access Management (IAM):** Amazon Cognito (RBAC: User & Admin Roles)
* **API Routing:** Amazon API Gateway
* **Compute Services:**
  * User Service: AWS Lambda (Serverless)
  * Matching Service: AWS Lambda (Serverless for scalable swipe logic)
  * Notification/Metrics Service: AWS Lambda
* **Databases:** Amazon DynamoDB (NoSQL for millisecond latency)
* **Asynchronous Communication:** Amazon SQS (Decoupling of services)
* **Infrastructure as Code (IaC):** Terraform
* **Monitoring & Observability:** AWS CloudWatch & AWS X-Ray (Distributed Tracing)

### Architecture Diagram
```mermaid
graph TD
    %% User Interfaces
    User[Web Client App] -->|HTTPS| CF[AWS CloudFront]
    Admin[Web Admin Dashboard] -->|HTTPS| CF

    %% Hosting
    CF -->|Static Assets| S3[Amazon S3 Frontend Hosting]

    %% Identity Provider
    User -->|Login/JWT| Cognito[Amazon Cognito IAM]
    Admin -->|Login/JWT| Cognito

    %% API Layer
    User -->|API Requests| API[Amazon API Gateway]
    Admin -->|API Requests| API

    %% Backend Microservices
    subgraph "AWS Cloud - Serverless Backend"
        API --> ProfileService[User Service <br/> AWS Lambda]
        API --> MatchService[Swipe & Match Service <br/> AWS Lambda]
        API --> AdminService[Admin Metrics Service <br/> AWS Lambda]

        %% Databases
        ProfileService --> DB_Profile[(DynamoDB: Profiles)]
        MatchService --> DB_Match[(DynamoDB: Swipes)]

        %% Asynchronous Event Handling
        MatchService -->|Match Event| SQS[Amazon SQS Queue]
        SQS --> AdminService
        AdminService --> DB_Stats[(DynamoDB: Statistics)]
    end

    %% Monitoring & DevOps
    subgraph "DevOps & Observability"
        TF[Terraform IaC] -.->|Deploys| Cognito
        TF -.->|Deploys| API
        TF -.->|Deploys| S3

        CW[AWS CloudWatch & X-Ray] -.->|Monitors| ProfileService
        CW -.->|Monitors| MatchService
    end
```

# 📂 Repository Structure

To adhere to the Separation of Concerns principle and keep our deployments clean, the CloudCrushHHN ecosystem is divided into three distinct repositories:

* `cloudcrush-core`: The engine of the platform. This monorepo contains our complete Infrastructure as Code (Terraform) alongside the source code for all our AWS Lambda backend microservices.

* `cloudcrush-web-client`: The Mobile-First React Progressive Web App (PWA) that acts as the primary dating interface for the students.

* `cloudcrush-web-admin`: The internal React dashboard for administrators to monitor platform metrics, secured via Cognito admin roles.