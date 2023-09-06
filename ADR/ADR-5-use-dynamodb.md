## ADR-5: Use DynamoDb as Receipt Storage

## Status
Proposed

## Context
Need to use a database with good performance even with a big volume of data

## Decision
The challenge is that each day size of database can grow by 173GB per day (**CONC-2**). So each month will get around 520Gb of data.
We could use RDS or Aurora but it is RDMS and they don't support many connections at the same time (**CONC-1**). Also need additional management of this database (Maester, Read replicas) 

We should use DynamoDb based on **QA-2** and **QA-3**
DynamoDb is key-value serverless storage with HA/HP.

## Consequences: 
High cost. Expensive than RDS and Aurora 

