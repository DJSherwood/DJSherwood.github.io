---
author: "Daniel Sherwood"
layout: post
title: "Designing Data Intesive Applications - Chapter 11"
date: 2025-06-02 15:44:00 -0500
categories: python
lead: "Stream Processing"
---
**I have had** the incredible opportunity to participate in the *Coder's Study Group* located in my city. 
The other participants are incredibly intelligent and experienced. 
Their insights have helped bring the abstract down into a more practical and relatable realm. 
We are now on Chapter 11 - Streaming Data. 
It is nearly the final chapter. Possibly, the author intends to tie many concepts introduced previously into this chatper.
Although I usually take notes privately, here I will put them out into the public space. 

# Streaming Data

## Transmitting Event Streams

### Messaging Systems

#### Direct Messaging from producers to consumers

#### Message Brokers

#### Message Brokers Compared to Databases

#### Multiple Consumers

#### Acknowledgements and redelivery 

### Partitioned Logs

#### Using Logs For Message Storage

#### Logs Compared to Traditional Messaging

#### Consumer Offsets

#### Disk Space Usage

#### When Consumers Cannot Keep Up with Producers

#### Replaying Old Messages

## Databases and Streams

### Keeping Systems in Sync

### Change Data Capture

#### Implementing Change Data Capture

#### Initial Snapshot

#### Log Compaction 

#### API Support for Change Streams

### Event sourcing

#### Deriving Current State From the Event Log

#### Commands and Events

### State, Streams, and Immutability

(ooh a formula )

#### Advantages of Immutable Events

#### Deriving Several Views from the Same Event Log

#### Concurrency Control 

#### Limitations of Immutablity 

## Processing Streams

### Uses of Streams Processing

#### Complex Event Processing

#### Stream Analytics

#### Maintaining Materialized Views

#### Search on Streams

#### Message passing and RPC

### Reasoning about Time

#### Event Time Versus Processing Time

#### Knowing When You Are Ready 

#### Whose Clock Are You Using, Anyway?

#### Types of Windows

### Stream Joins

#### Stream-Stream Join (Window Join)

#### Stream-Table (Stream Enrichment)

#### Table-Table Join (Materialized View Maintenance)

#### Time-Dependence of Joins 

### Fault Tolerance

#### Microbatching and Checkpointing

#### Atomic Commit Revisited 

#### Idempotence

#### Rebuilding State After a Failure

## Summary
