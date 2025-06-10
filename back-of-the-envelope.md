# Latency Estimates

## Back-of-the-Envelope Latency Cheat Sheet (2025)

| Operation                                             | Latency Estimate        | Notes                                           |
|-------------------------------------------------------|-------------------------|-------------------------------------------------|
| L1 cache reference                                    | ~0.5 ns                 | Fastest memory access                           |
| L2 cache reference                                    | ~4 ns                   |                                                 |
| L3 cache reference                                    | ~12 ns                  |                                                 |
| Main memory (RAM) access                              | ~100 ns                 | DRAM latency                                    |                                                                     
| SSD I/O (local NVMe)                                  | ~10–100 µs              | Varies by device and queue depth                |                                                  
| Read 1MB from SSD                                     | ~250 µs                 | Assuming high throughput SSD (~4 GB/s)          |                                                  
| Spin on CPU (no context switch)                       | ~1 µs                   | Active wait                                     |                                               
| Lock/unlock mutex (uncontended)                       | ~25 ns – 1 µs           | Depends on architecture and contention          |                  
| Send packet on 1 Gbps Ethernet                        | ~1 ms per 1500-byte pkt | Includes transmission & propagation time        |                   
| Round-trip within same datacenter (microservice call) | ~0.5 – 1 ms             | Fast path for internal services (no congestion) |
| Round-trip between regions (cloud interzone)          | ~50 – 100 ms            | Depends on cloud provider / routing             |                     
| Context switch (OS)                                   | ~1 – 5 µs               | Depends on kernel and CPU                       |                                                           
| Read 1MB over network (1 Gbps)                        | ~10 ms                  | Network + disk latency combined                 
| Disk seek (HDD)                                       | ~8 – 10 ms              | Rare today for latency-critical paths           |                                                  
| Database query (simple, indexed)                      | ~1 – 5 ms               | Depending on cache hit and backend              |                                     
| Garbage collection pause (minor)                      | ~10 – 50 ms             | Highly dependent on language and heap size      |                           
| GC pause (major/full)                                 | ~100 ms – 1 sec+        | JVM, Go, etc.                                   |                        
| Cold start (Lambda/Cloud Function)                    | ~100 ms                 | – few sec Cloud provider + language dependent   |                          

**Reference**: Jeff Dean, “Numbers Every Programmer Should Know”,

Tips:

- Use these values to estimate cost trade-offs (e.g., “Should I call another service or duplicate data?”).
- Chain numbers to get pipeline latency: 2 microservices = ~1–2 ms latency minimum.
- If you’re targeting <10 ms SLAs, avoid chaining more than 5+ network calls.
- Use asynchronous processing if chaining can't be avoided.

### Microservice call

- Base latency per call: ~0.5 ms.
- Additional overhead: Add time for serialization/deserialization, request processing, load balancer hops, etc. — commonly 1–3 ms extra.

Realistic rough estimate: ~2–5 ms per internal network call, depending on workload.

## The cost of locks

Interesting paper which expands on the cost of locks:https://lmax-exchange.github.io/disruptor/disruptor.html#_the_cost_of_locks