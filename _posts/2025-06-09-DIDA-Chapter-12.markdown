---
author: "Daniel Sherwood"
layout: post
title: "Designing Data Intensive Applications - Chapter 12"
date: 2025-06-09 08:54:00 -0500
lead: "The Future of Data Systems"
---

*This chapter is* about what the future holds for data systems.

# Data Integration

## Combining Specialized Tools by Deriving Data

### Reasoning about dataflows

### Derived data versus distributed transactions

### The limits of total ordering

### Ordering events to capture causality

## Batch and Stream Processing

### Maintaining derived state

### Reprocessing data for application evolution 

### The lambda architecture

### Unifying batch and stream processing

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
