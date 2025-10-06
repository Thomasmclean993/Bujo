https://github.com/Hatch1fy/messaging-service

Project Objectives:
- [ ] Build a Unified Messaging Service
- [ ] Manage Conversations Automatically 
- [ ] Persist All Data in a Relational Database
- [ ] Handle Provider Reliability & Network Issues Gracefully
- [ ] Conform to the Given Project Scaffold
- [ ] Demonstrate Engineering Excellence
---
## Requirements
Unified Messaging API: HTTP endpoints to send and receive messages from both SMS/MMS and Email Providers.
	- Support sending messages through the appropriate provider based on message type
	- Handle incoming webhook messages from both providers
Conversation Management: Messages should be automatically grouped into conversations based on participants (from/to addresses)
Data Persistence: All conversations and messages must be stored in a relational database with proper relationships and indexing



## 🧩 In Practical Terms, You’ll Deliver

1. **HTTP Endpoints**
    
    - `/messages/send` → Send outbound messages via SMS/MMS or Email.
    - `/webhooks/<provider>` → Handle inbound webhooks from providers.
    - Possibly `/conversations` → View or query message threads.
2. **Database**
    
    - Tables for `conversations`, `messages`, and maybe `participants`.
    - Relationship: one conversation → many messages.
3. **Core Logic**
    
    - Detect or create conversation by participants.
    - Normalize inbound/outbound payloads to a unified schema.
    - Retry or handle provider errors gracefully.
4. **Operational Scripts**
    
    - Makefile + Docker Compose for consistent environment.
    - Test scripts validating correctness.