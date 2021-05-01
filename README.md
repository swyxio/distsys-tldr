# distsys-tldr

TLDRs of Distsys Papers.

- [Spanner: Google’s Globally-Distributed Database (2012)](https://static.googleusercontent.com/media/research.google.com/en//archive/spanner-osdi2012.pdf) - [1hr 20min read](https://youtu.be/OdbOCpwWZLs)
  -  Design goals
    - Multi-version
    - Globally Distributed
    - Synchronously Replicated
    - Externally-consistent distributed transactions 
      - Strong consistency in the presence of wide-area replication
      - Non-blocking reads in the past
      - Lock-free read-only transactions
    - Automatic reshard across machines to balance load/respond to failures
    - Scale: millions of machines, hundreds of datacenters
    - Atomic schema changes (aka Schema-Change Transactions)
    - SQL-base query language
    - Semi-relational interface (key + timestamp primary key)
  - Solution
    - TrueTime API exposing clock uncertainty
    - Enables globally meaningful commit timestamps
    - 2 types of clocks - GPS and Atomic (Armageddon masters) - poll every 30s
    - Account for worst-case clock drift, 200 microseconds/second
    - Keep e (uncertainty) small, <10ms
    - Types of Transactions
      - Read-Write (slowest, writes never commit in the same phase as the write)
      - Read-only
      - Snapshot read with timestamp
      - Snapshot read with bound (Spanner picks timestamp)
    - Disjoint invariant - intervals for each Paxos leader
      - lots of proofs that these guarantees are reliable
      - a leader must only assign timestamps within the interval of its leader lease
  - Discussion of internal architecture - universemaster, placement driver, zones, location proxies, spanservers, directories, fragments 
  - Discussion of usage in Google F1 with benchmark results on latency, throughput, and availability
    - resharding mysql at google took 2 years of intense effort
  - Comparison of Spanner vs other work incl DynamoDB, Clavin, VoltDB etc

> We have shown that reifying clock uncertainty in the time API makes it possible to build distributed systems with much stronger time semantics. In addition, as the underlying system enforces tighter bounds on clock uncertainty, the overhead of the stronger semantics decreases. As a community, we should no longer depend on loosely synchronized clocks and weak time APIs in designing distributed algorithms.

## Reading Lists

- https://dancres.github.io/Pages/
- http://muratbuffalo.blogspot.com/2021/02/foundational-distributed-systems-papers.html
