[[Datadog integration]]
[[Pa Status]]
[[Workflow.canvas|Workflow]]
Brief 

Soft coding challenge -> in Elixir
- Address some ambiguously topics
- logic
- Ask questions
- Business requirements
Q and A -> 20 mins 


## Rest 
Look up:
- Learn graphql 
- AWS
- What like about Elixir
- Best practice around Elixir 
- Data persistance 
- Data retrieval
- Caching 
- Testing 
- Performance 
- Migration work
- Scalability 
- Maintenance 
- Observability
- Junior/Mentees

### What I like about Elixir
For Full stack: Liveview-> A Snappy Real time app that is just as rich as Javascript while being extremely simple.
OTP -> I don't hear this enough, the level of fault Tolerance is hard to compare. Process that fail can simple restart using the supervision tree. Lightweight process that make concurrency a breeze
ML -> It's kinda surprising how Elixir is ignored for machine learning. It libraries make it a serious contender
Nx AXON.

### What I dont like about it
The community can be kinda ruby like. A little too much dogma sometimes.

### Data persistence
AWS for token
ETS for caching of tokens
TokenWarmer project:
	Original implementation using a genserver failed failed due to complexity. It was difficult to create of token fetchers. So I replaced the implementation use Cachex library and ETS.

### Performance 
We had an integration with an universal engine that make our api request to the external partner take 6 secs. I replace the universal engine with my own cutting the response down to sub 2 sec responses. Also limited the ongoing timeout problem, where responses randomly take 15 secs. 
Used Task.async to retrieve supplementary data while waiting for the response
<details>
<summary> **Performance Optimization of External API Integration**</summary>
**Context:**  
Our application relied on an integration with a universal engine to handle API requests to an external partner. This approach resulted in **high latency (average 6 seconds per request)** and occasional **timeout spikes up to 15 seconds**, negatively impacting user experience and system reliability.

**Action Taken:**

- **Replaced the universal engine** with a custom-built solution tailored to our API requirements, eliminating unnecessary overhead and improving efficiency.
- Implemented **concurrent data retrieval** using Elixir’s `Task.async` to fetch supplementary data in parallel while waiting for the primary API response.
- Optimized error handling and timeout logic to reduce variability in response times.

**Results:**

- Reduced average API response time from **6 seconds to under 2 seconds** (≈67% improvement).
- Eliminated random timeout spikes (previously up to 15 seconds), improving system stability.
- Increased throughput and improved user experience for time-sensitive operations.

**Technical Highlights:**

- Leveraged Elixir’s **concurrency model** for parallel execution without blocking the main process.
- Designed a **lightweight, fault-tolerant integration layer** that aligns with OTP principles.
- Improved monitoring and logging for better visibility into API performance.
- 
	
</details>

### Migration Work
New endpoint work for Antem Pastatus
Medimpact 
file storage vs Azure key vault for certs and monitoring

## Mentorship
I have mentor interns, mentees and junior devs.
## Questions:
- AI is 

## Call Zane after interview

