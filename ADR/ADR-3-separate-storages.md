## Title: 
ADR-3: Analytics storage

## Status: 
**Proposed**

## Context: 
Since the size of receipt storage will grow we need we improve performance and availability of operational services.

## Decision: 
Report generation is usually a heavy process and affect the operational system significantly, so it is reasonable to store reporting related data in a separate database.

## Consequences: 
We need to keep in sync reporting data.
