# Study2Gether

> **Never study alone. Find your focus, build your streak.**

Study2Gether is an app made for uni students who are tired of studying alone. We took that familiar left/right swiping mechanic you already know and applied it to finding the perfect study buddy.

Before you start swiping through profiles, you can set up exactly what kind of study vibe you're going for:
1. **Subject-Specific Mode:** Type in exactly what you're working on (like "Cloud Computing" or "Calculus") to link up with someone studying the exact same thing so you can figure it out together.
2. **Flex Mode:** Just pick "Nothing Specific" to match with someone doing totally different work. You just hang out (virtually or IRL) and keep each other accountable so nobody gets distracted by their phone.

Once you both swipe right on each other, it's a match! Using a time and place proposal form you can figure out when and where to study. To keep you motivated and actually hitting the books, we've got a built-in streak system. The more days a week you study, the higher your rank climbs (like hitting Silver, Gold, or Diamond).


## Core Features
* **Web-First Experience:** A fully responsive website accessible from any browser.
* **Account Creation:** Secure user sign-up and authentication via email.
* **Individual Profiles:** Users can set up their profiles with multiple pictures, a personal bio, and selectable subjects they are interested in.
* **Dual Study Modes:** The ability to toggle between "Subject-Specific Mode" (finding someone studying the exact same topic) and "Flex Mode" (matching with anyone).
* **Discovery & Swiping:** An intuitive, card-based feed where users can swipe left to pass or right to connect with potential study partners.
* **Meetup Proposal System:** Matches coordinate using a streamlined scheduling tool. One user proposes a time and a campus location, and the other userer can accepts, declines, or proposes an alternative. Once accepted, the session is officially scheduled.
* * **QR Code Meetup Verification:** To ensure sessions actually happen and streaks remain legit, the site generates a unique QR code when you meet up. Your buddy scans it to verify the session, track the time, and securely update your ranks.
* **Gamified Streaks & Rankings:** A dynamic tracking system that rewards consistent study habits. Meeting up regularly builds your streak and promotes you through visual ranks (e.g., Silver, Gold, Diamond).
* **Admin Dashboard:** A separate, restricted interface for platform administrators to manage users, review flagged profiles, and oversee the platform's general activity.

## System Architecture

<p align="center">
  <img src="images/architecture.jpg" width="900" alt="System Architecture Diagram">
  <br>
  <em>Figure 1: System Architecture Diagram</em>
</p>

### Logical Flow

Our application leverages a fully serverless architecture on AWS to ensure high availability, scalability, and real-time responsiveness:

1. **Frontend Delivery & Authentication:** The client-side application (React) is hosted in an **Amazon S3 Bucket** and distributed globally via **Amazon CloudFront** (CDN) for fast loading. User authentication is managed securely by **AWS Cognito**, which issues JWT tokens to authorize subsequent API requests.
2. **REST API (Swiping & Profiles):** Standard client requests (like updating a profile or swiping left/right) are routed through **Amazon API Gateway (REST API)**. These endpoints trigger specific **AWS Lambda** functions (e.g., `UpdateProfileLambda`, `ProcessSwipeLambda`) in the compute layer. A scheduled event via EventBridge also routinely triggers `GenerateMatchesLambda` to finalize matches based on swipe data.
3. **Real-time WebSockets (Chat & Timers):** For real-time interactions, such as private post-match chats and study timers, the client establishes a persistent connection through **Amazon API Gateway (WebSocket API)**. Connection states and messages are handled by dedicated serverless functions (`WebSocketConnectLambda`, `SendMessageLambda`, etc.).
4. **Data Storage & Single Table Design:** All application data (User Profiles, Swipe/Match records, Chat logs, and Streak data) is stored in a single **Amazon DynamoDB** table. The database uses a highly optimized Single Table Design with specific primary access patterns (e.g., `PROFILE#{UserID}`, `ROOM#{RoomID}`) for rapid querying.
5. **Real-time Synchronization:** When a match occurs or a chat message is sent, the system uses the **AWS API Gateway Management API (PostToConnection)** to instantly push updates and notifications back to the connected clients.

## Project Structure & Modules

This repository is organized into three main sub-projects. Please refer to their specific READMEs for detailed technical documentation.

| Module | Technology | Description | Documentation |
| :--- | :--- | :--- | :--- |
| **study2gether-core** | AWS Lambda, API Gateway, DynamoDB, Cognito, EventBridge | The serverless backend infrastructure. It handles REST APIs, WebSocket connections, swiping/matching logic, and database operations. | [View README](https://github.com/Study2Gether/study2gether-core/blob/master/README.md) |
| **study2gether-web-client** | React, Amazon S3, Amazon CloudFront | The user-facing web application where students can set their study modes, swipe on profiles, and chat in real-time. | [View README](https://github.com/Study2Gether/study2gether-web-client/blob/master/README.md) |
| **study2gether-web-admin** | React, AWS | The administrative dashboard used for platform management, monitoring user data, and system moderation. | [View README](https://github.com/Study2Gether/study2gether-web-admin/blob/master/README.md) |

### Credits
**University:** Hochschule Heilbronn (HHN)<br>
**Course:** Cloud Computing SS26<br>
**Team:**
* [NDXIII](https://github.com/NDXIII)
* [Kartoffelbauer](https://github.com/Kartoffelbauer)
* [Gnas20](https://github.com/orgs/Study2Gether/people/gnas20)
* [Philipp-stack-code](https://github.com/orgs/Study2Gether/people/gnas20)
