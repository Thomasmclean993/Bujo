https://github.com/Hatch1fy/messaging-service

Project Objectives:
- [ ] Build a Unified Messaging Service
- [ ] Manage Conversations Automatically 
- [ ] Persist All Data in a Relational Database
- [ ] Handle Provider Reliability & Network Issues Gracefully
- [ ] Conform to the Given Project Scaffold
- [ ] Demonstrate Engineering Excellence
---
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