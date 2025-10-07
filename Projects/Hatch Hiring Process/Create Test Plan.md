# Test Cases

## Schema & Validation Test
#### Message Schema Validation 
- [ ] Valid Type
	- Input: Proper Payload with `type: "sms"`, valid fields.
	- Expected: Changeset to be valid
- [ ] Invalid Type
	- Input: Invalid `type:fax`
	- expected: Changeset invalid, appropriate
#### Missing Required Fields 
- [ ] Omit `from` or `to` 
	- Expected: Validation failures, appriorate error message
#### Attachment field normalization
- [ ] Input: `attachments: null`
- Expected: Normalized to empty list `[]`
#### Conversation schema Validation 
- [ ] Input: participants list valid, stored correctly as json/array
	- Expected: Valid changeset

## Conversation Logic
1. **Find or create conversation – new pair**
    
    - Given no conversation between `A` and `B`.
    - Expected: New conversation record created.
2. **Find or create conversation – existing pair**
    
    - Given existing conversation between `A` and `B` (any direction).
    - Expected: Returns same conversation record (order-insensitive).
3. **Add message to conversation**
    
    - Adds message to existing conversation.
    - Expected: Message count increments; foreign key set.
4. **Conversation last‑updated timestamp**
    
    - Adding message updates conversation’s `updated_at`.

### **Provider Adapters**

1. **SMS provider: successful send**
    
    - Mock provider returns `200`.
    - Expected: `{:ok, response}` tuple.
2. **Email provider: successful send**
    
    - Simulated success returns expected response fields.
3. **Provider error handling – HTTP 500**
    
    - Provider mock returns 500 error.
    - Expected: Function surfaces a recoverable error atom (e.g., `{:error, :provider_failure}`).
4. **Provider error handling – HTTP 429 (rate limit)**
    
    - Should be retried or logged properly depending on strategy.
    - Assert correct retry or delay handling invoked.
5. **Provider interface contract**
    
    - All provider modules comply with behaviour definition (`@callback send_message/1`).


### **Messaging Context Logic**

1. **Routes message to correct provider**
    
    - `type: "sms"` → SMS provider; `type: "email"` → Email provider.
2. **Handles provider errors gracefully**
    
    - Provider failure returns error but does not crash app.
3. **Persists message after send success**
    
    - After sending a message, DB record created with correct conversation ID.
4. **Inbound message normalization**
    
    - Transforms inbound payload (SMS/MMS/Email) into unified internal representation.
5. **Inbound message triggers conversation lookup or creation**
    
    - Ensures inbound content from unknown participants creates a new conversation.
6. **Timestamp normalization**
    
    - Converts inbound timestamps to UTC; consistent across message types.

### **Utility and Edge Tests**

1. **Participant normalization logic**
    
    - Different formats (e.g., capitalization, phone vs. E.164) are normalized consistently.
2. **Duplicate inbound message prevention**
    
    - When receiving provider message with known message ID (`messaging_provider_id` or `xillio_id`), ignore duplicates.
3. **Graceful failure for malformed inbound payload**
    
    - Missing key fields result in 400 error or rejection flag, not crash.
## 🌐 INTEGRATION TEST CASES

### **Outbound Message Flow**

1. **POST /api/messages/send – SMS success**
    
    - Input: Valid SMS payload.
    - Expected: 200 OK; message saved; provider mock called once.
2. **POST /api/messages/send – Email success**
    
    - Valid email payload.
    - Expected: 200 OK; email provider mock called; DB record created.
3. **Outbound send – auto conversation creation**
    
    - First message between two participants creates conversation.
4. **Outbound send – existing conversation appended**
    
    - Subsequent message between same pair adds to existing conversation.
5. **Outbound send – provider error (HTTP 500)**
    
    - Provider mock returns error; API still responds gracefully (e.g., 502 Bad Gateway or 500 Internal Error).
6. **Outbound send – provider rate‑limit (HTTP 429)**
    
    - Simulated retry logic tested: eventual success or descriptive error.
7. **Outbound invalid payload**
    
    - Missing `to` or `body`.
    - Expected: 422 Unprocessable Entity.
### B. **Inbound Webhook Flow (Simulated Providers)**

1. **POST /api/webhooks/sms – valid inbound SMS**
    
    - Expected: 200 OK; new message stored; correct conversation linked or created.
2. **POST /api/webhooks/mms – inbound with attachments**
    
    - Attachments saved properly; stored as list.
3. **POST /api/webhooks/email – valid inbound email**
    
    - Creates email message; preserves HTML body.
4. **POST /api/webhooks/sms – missing fields**
    
    - Invalid payload returns 400 Bad Request; no message stored.
5. **POST /api/webhooks/email – duplicate message ID**
    
    - Duplicate ignored; existing record preserved.

### C. **Conversation Retrieval**

13. **GET /api/conversations/:id – valid**
    
    - Returns JSON list of messages in correct chronological order.
14. **GET /api/conversations/:id – nonexistent**
    
    - Returns 404 Not Found.
15. **Conversations include cross‑provider messages**
    
    - Send and receive SMS + Email within same conversation; ensure all messages returned under same thread.

---

### D. **Database & Integrity Behavior**

16. **Transactional consistency**
    
    - Failed provider send rolls back message insertion.
    - Database remains clean.
17. **Referential integrity**
    
    - Deleting a conversation (if allowed) cascades or restricts messages correctly (depending on schema design).
18. **Index efficacy check (query test)**
    
    - Retrieve conversation messages sorted by timestamp — verifies index usage (implicitly tested for speed and order).

---

### E. **Resilience & Edge Scenarios**

19. **Simulated transient provider errors with retry**
    
    - Mock causes first two attempts to fail, third to succeed; message eventually sent and persisted once.
20. **Concurrent inbound and outbound activities**
    
    - Two simultaneous sends from/to same participants still end up in same conversation without duplicates.
21. **Database unavailable (simulate failure)**
    
    - Appropriate error handling/logging without unhandled crash.