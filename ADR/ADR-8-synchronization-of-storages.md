## Title: 
ADR-8: Synchronization of Receipt and Analytics Warehouse storages

## Status: 
Proposed

## Context: 
All records from DynamoDb should replace to S3 data warehouse

## Decision: 
In DynamoDb we can use TTL 30 days for each records and they will remove automatically. It gives us big benefit: database will not grow.
We can use DynamoDb Stream to get record removed by TTL. After that, we call Lambda (TTL Processor) and pass data to Kenesis Firehouse
Kenesis Firehouse will format data in Parquet format and put data to S3[ADR-7](ADR/ADR-7-use-athena-for-reporting.md).
Kenesis Firehouse needs to put data to s3 bucket with such prefix: s3bucket/date-prfix/


