## Title: 
ADR-6: Use separate warehouse storage  with huge size.

## Status: 
Proposed

## Context: 
The warehouse could be a huge size. Reports should run less then 30seconds(**QA-4**)

## Decision: 

Only Haddop and S3 can work with huge sizes(PeatBytes).
The choice is obvious as we use AWS

## Consequences: 
Synchronization with DynamoDb [ADR-5](ADR/ADR-5-use-dynamodb.md)
