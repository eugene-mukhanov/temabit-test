## Title: 
ADR-2: Use message queues with guaranteed delivery for ticket workflow

## Status: 
Proposed

## Context: 
Since we use AWS Lambda ([ADR-1](ADR-1-serverless.md) we need to make some communication between lambda functions in different steps.
This makes an implication for decoupling these areas from one another.

## Decision: 
Best way to make communication is AWS SQS.
Also it guarantees that all messages will be processed.

## Consequences: 
Let's consider the trade-offs:
 - added complexity;
 - need to process dead queue
