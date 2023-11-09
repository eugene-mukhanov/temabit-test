## Title: 
ADR-7: Use AWS Athena for analytics

## Status: 
**Proposed**

## Context: 
Need to get analytics for the last 20 years(**QA-4**) from S3 data warehouse[ADR-6](ADR/ADR-6-use-s3-as-warehouse.md).

## Decision: 
The best way to get data from S3 is AWS Athena. We could use Redshift Sceptum but it is more complex and more costly
  
## Consequences: 

* For best performance we need to use files in Parquet format and at least 128MB size. Data should store in different folders s3:/{data}/..
