---
author: "Daniel Sherwood"
layout: post
title: "Designing Data Intensive Applications - Chapter 12"
date: 2025-06-09 08:54:00 -0500
lead: "The Future of Data Systems"
---

*This chapter is* about what the future holds for data systems.

# Data Integration

For a given problem, there are several solutions. 
The most appropriate software tool is dependent on the circumstances.

## Combining Specialized Tools by Deriving Data

Often systems require multiple tools to work together. 
In this sense, consider the "data flowing" through a series of software tools. 

### Reasoning about dataflows

Important to track the inputs and outputs of data as it flows through your software. 
Allowing an application to write to both the search index and the database could affect concurrency.
Useful to funnel all user input through a single system that decides on an ordering for all writes. 

### Derived data versus distributed transactions

Distributed transactions are used to keep different data systems consistent. 
Distributed systems provide linearizability while derived data systems update asynchronously.

### The limits of total ordering

A totally ordered event log is feasible with systems that are small. 
Formally, "deciding on a total order of events is known as *total order broadcast* which is equivalent to consensus."

### Ordering events to capture causality

How to get a correct order of events, needed by causally connected systems? 
Log an event to record the state before the change with a unique identifier, then later events will refer to this previous event to capture causal dependency.

## Batch and Stream Processing

Batching and stream have a lot in common. 
"Spark performs stream processing on top of a batch processing engine by breaking the stream into *microbatches*, whereas Apache Flink performs batch processing on top of a stream processing engine".

### Maintaining derived state

Batch processing encourages deterministic, prue functions whose output depends only on the input and which have no side effects other than the explicit outputs.
Stream processing has all the above but also extends operators to allow managed, fault-tolerant state.

### Reprocessing data for application evolution 

Stream processing allows changes in the input to be reflected in derived views with low delay and batch processing allows large amounts of accumulated historical data to be reprocessed to derive new views onto an existing dataset.

### The lambda architecture

The *lambda architecture* runs two different systems in parallel:

1. batch processing like Hadoop MapReduce
2. stream processing like Storm

with the intention of recording incoming data by appending immutable events to an always growing dataset.
From these events, read-optimized views are derived. 

### Unifying batch and stream processing

Replaying historical events, ensuring that bad outputs from faults are eliminated from the stream, and tools for windowing time allow for the unification of streaming and batch processing.

# Unbundling Databases

The hadoop ecosystem is somewhat like a distributed version of Unix.
Unix presents programmers with low-level hardware abstraction, while relational databases give programmers a high-level abstraction that hides complexities of data structures on disk, concurrency, cash recovery, etc.

## Composing Data Storage Technologies

Databases have: 

1. Secondary indexes
2. Materialized views
3. Replication logs
4. Full-text search indexes

There are parallels between the features that are built into databases and derived data systems.

### Creating an index

1. Database scans over a consistent snapshot of a table
2. pick out field values being indexed
3. Sort them
4. Write out to index
5. Process the backlog of writes (since the consistent snapshot was taken)
6. Continue to keep the index up to date

This is similar to setting up a follower replica.

### The meta-database of everything

The dataflow over an entire organization starts looking like one huge database.
Batch and streaming processes are like elaborate triggers, stored procedures, and materialized views

Perhaps there will be two main avenues of storage and processing: 

1. Federated databases: unifying reads
2. Unbundled databases: unifying writes

### Making unbundling work

The *"traditional approach to synchronizing writes requires distributed transactions across heterogeneous storage systems*", which the author believes is incorrect.
And asynchronous event log with idempotent writes is preferable.

The big advantage of log-based integration is *loose coupling* between the various components. 

### Unbundled versus integrated systems

"Databases are still required for maintaining state in stream processors, ad in order to server queries for the output of batch and stream processors."

### What's missing?

"We don't yet have the unbundled database equivalent of the Unix shell." (a high level language for composing storage and processing systems in a simple and declarative way).
Ex, ```mysql | elastic search```

## Designing Applications Around Dataflow

Composing specialized storage and processing systems with application code is known as "database inside-out."
Spreadsheets have more dataflow programming capabilities compared to most programming languages.

### Application code as a derivation function 

A derived dataset results from a transformation applied to an initial dataset.

1. Secondary index
2. Full text index
3. ML Model
4. Cache containing aggregated data

Databases struggle with executing custom code.

### Separation of application code and state

Theoretically, "databases could be deployment environments for arbitrary application code, like an operating system."
Practically, this doesn't work. 
Other applications like Kubernetes, work well at this.

The trend has been to keep stateless application logic separate from state management (database).
Databases notifying users via subscription of changes is slowly becoming a feature.

### Dataflow: Interplay between state changes and application code

Thinking about applications in terms of dataflow, means reconsidering relationship between database and state management.
Keep in mind that maintaining derived data is not the same as asynchronous job execution. 

* derived data: order of state changes is important, but message brokers do not have this capability
* derived data: fault tolerance is key

Modern stream processing can provide these ordering and reliability guarantees at scale?

### Stream processors and services

Currently, application development involves breaking down functions into a set of *services* that communicate via synchronous network requests such as REST APIs.
Loose coupling provides scalability.
Dataflow systems are different, as they use asynchronous, 1 way communication instead of two way REST services.

( wait, how is this working?)
In the database approach, the code that processes purchases would subscribe to a stream of exchange rate updates, record the current rate in a local database, then just query the database when needed.

## Observing Derived State

The process for creating and keeping derived datasets is called the *write path*.
If you want to query the derived dataset, then you will follow the *read path*.

### Materialized views and caching

Ex. the *write path* updates a full-text search index and the *read path* searches the index for keywords.
Without an index, search would be expensive.
But precomputing all possible queries would be extremely difficult on the *write-path*, too.
An additional option is to create a *cache* of common search results.
Basically, perform work that exists on the boundary between the *write-path* and the *read-path*.

### Stateful, offline-capable clients

Renewed interest in *offline-first* applications (they use a local database first, then connect online second).
Think of the on-device state as a *cache of state on the server*.
"The pixels on the screen are a materialized view onto model objects in the client app; the model objects are a local replica of state in a remote datacenter."

### Pushing state changes to clients

"...actively pushing state changes all the way to client devices means extending the write path all the way to the end user."
Each device would be a subscriber to a small stream of events.

### End-to-end event streams

React + Flux + Redux manage internal client-side state by subscribing to a stream of events representing user input/responses from a server. 
Natural to extend this to allow a sever to push state-change events into the client-side event pipeline.
State changes flow through *write-path*.
Challenge is that the assumption of stateless clients and request/response interactions is ery deeply ingrained. 
Need to move away from request/response to publish/subscribe.

### Reads are events too

Recall that stream processors also need to maintain state to perform aggregations and joins.
When both writes and reads are represented as events, and outed to the same stream, a stream-table join is performed.
Serving requests = performing joins.
Writing read events to durable storage enables better tracking of causal dependencies.

### Multi-partition data processing

Send queries through a stream and collect responses. 
Treating queries as streams provides an option for implementing large-scale solutions.

# Aiming for Correctness

The old reliable ( but slower transactions ) vs. newer scalability ( but only eventual consistency ). 

## The End-to-End Argument for Databases

If an application writes incorrect data or delete data from a database, serializable transactions aren't going to save you.

### Exactly-once execution of an operation 

Processing twice is a from of data corruption. _Exactly once_ means arranging the computation such that the final effect is the same as if no faults ahd occurred. 

### Duplicate suppression 

A nasty problem that is hard to completely overcome, as it can occur at several points. 
They can occur between the database client and server, but they can also occur between the network and the end device. 
From the web server's point of view, a retry is a separate request, but from the database's point of view it is a separate transaction. 

### Operation identifiers

So to truly make something idempotent one has to consider the _end-to-end_ flow. 
Could generate an id for every operation, then write to a table that has a uniqueness constraint.

### The end-to-end argument

The function in question can only be implemented by the applications standing at the endpoints. 
Providing the function as a feature of the communication system itself is not possible.

### Applying end-to-end thinking in data systems

Transactions are useful but not quite good enough.

## Enforcing Constraints

Uniqueness constraints can help assuage duplication issues, and can be useful in unbunbled databases. 

### Uniqueness constraints require consensus

Enforcing a uniqueness constraint requires consensus.

### Uniqueness in log-based messaging 

"The log ensures that all consumers see messages in the same order -- a guarantee that is formally known as _total order broadcast_ which is equivalent to consensus."
In the case of several users attempting to claim the same user-name:

1. Each request is encoded as a message & appended to a partition 
2. Stream processor reads the requests in the log. For every request for a username that is available, it records the name as taken it emits a success message ( or if already taken, a rejection message) to an output stream. 
3. Client watches the output stream, waiting for a success or a rejection.

### Multi-partition request processing

Ensuring an operation satisfies constraints when involving multiple partitions can be difficult. 
Correctness can be achieved with partitioned logs: 

1. The request is given a unique ID an appended to the log partition
2. The stream processor reads the lof or requests and emits *2* messages:
    * a debit instruction to the payer account
    * a credit instruction to the payee account
3. Subsequent processors consume the streams of credit and debit instructions, de-duplicate by request ID, and apply changes to account balances.

## Timeliness and Integrity

Transactions are typically _linearizable_ .
When unbundling occurs, this is no longer the case.
Definition of _Timeliness_ and _Integrity_ (preferable to the term _consistency_).

### Correctness of dataflow systems

ACID transactions provide by timeliness and integrity guarantees. 
But an event based dataflow decouples timeliness and integrity.
Stream processing can preserve integrity without requiring distributed transactions and an atomic commit. 

1. Represent content of write message as a single message
2. Derive all other state updates from that single message
3. Pass a client generated request ID through all these levels of processing, ensuring _idempotence_ and _end-to-end duplicate suppression_. 
4. Make the messages immutable and allow derived data to be reprocessed from time to time

### Loosely interpreted constraints

Enforcing a uniqueness constraint requires consensus, implemented by funneling all events in a partition to a single node.
Integrity is required, but timeliness on the _enforcement_ of the constraint is **not**. 

### Coordination-avoiding data systems

Observations: 
1. "Dataflow systems can maintain integrity guarantees on derived data without atomic commit, linearizability, or synchronous cross-partition coordination."
2. "Although strict uniqueness constraints require timeliness and coordination, many applications are actually fine with loose constraints that maybe temporarily violated and fixed up later, as long as integrity is preserved throughout."

This means that dataflow systems cna work without requiring coordination, wile still giving strong integrity guarantees.

## Trust, but Verify 

### Maintaining integrity in the face of software bugs

### Don't just blindly trust what they promise

### A culture of verification 

### Designing for auditability 

### The end-to-end argument again

### Tools for auditable data systems

# Doing the Right Thing

## Predictive Analytics

### Bias and discrimination 

### Responsibility and accountability 

### Feedback loops

## Privacy and Tracking

### Surveillance

### Consent and freedom of choice

### Privacy and use of data

### Data as assets and power 

### Remembering the Industrial Revolution 

### Legislation and self-regulation 

# Summary 
