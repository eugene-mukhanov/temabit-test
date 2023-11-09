## Title: 
ADR-9: Request Limiter

## Status: 
Proposed

## Context: 
RP can not send more receipts per day than is in its Subscription Plans (UC-4)

## Decision: 
For implement functinality of Request Limitter we could use AWS ElasticCache for Redis. When system received new receipt we need to increment item in Redis by command
[INCR](https://redis.io/commands/incr/).
Key of item should be in such formt: {providerId}-yyyy-mm-dd.

For each request we need to check did we exceede the limit in ElasticSearch.

Also each key-pair value should have TTL=24hours
