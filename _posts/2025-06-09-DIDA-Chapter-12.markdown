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

## Composing Data Storage Technologies

### Creating an index

### The meta-database of everything

### Making unbundling work

### Unbundled versus integrated systems

### What's missing?

## Designing Applications Around Dataflow

### Application code as a derivation function 

### Separation of application code and state

### Dataflow: Interplay between state changes and application code

### Stream processors and services

## Observing Derived State

### Materialized views and caching

### Stateful, offline-capable clients

### Pushing state changes to clients

### End-to-end event streams

### Reads are events too

### Multi-partition data processing

# Aiming for Correctness

## The End-to-End Argument for Databases

### Exactly-once execution of an operation 

### Duplicate supression 

### Operation identifiers

### The end-to-end argument

### Applying end-to-end thinking in data systems

## Enforcing Contraints

### Uniqueness constraints require consensus

### Uniqueness in log-based messaging 

### Multi-partition request processing

## Timeliness and Integrity

### Correctness of dataflow systems

### Loosely interpreted constraints

### Coordination-avoiding data systems

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
