## Title: 
ADR-1: Use Serverless architectural style as the basic style

## Status: 
Proposed

## Context: 
Software system does not process receipts at night also processing of receipts is durable.
The system should be scalable (QA-1)

## Decision: 
We discovered four main domain areas here each having a separate group of architectural characteristics:

 - We can use AWS Lambda function because they run only we call it.
 - We can increase cost of infrastructure.
 - AWS Lambda could help us to implemet durable process
