# Temabit test assignment

## Contents

- [System Requirements](#system-requirements)  
    - [Functional Requirements](#functional-requirements)
    - [Architecture Characteristics Requirements](#architecture-characteristics-requirements)
    - [Constraints](#constraints)
    - [Concerns](#concerns) 
- [Target Architecture](#target-architecture)  
- [Architecture Decision Records](#architecture-decision-records)
- [API Design](#api-design)

**Business Drivers**

 * Business wants to have analytics to make forecasts for the future.
 * Business wants a high-performance system to bring more customers (receipt provider).

**Business Goals**

* Develop a new receipt management system that will satisfy the required qualities.

## System Requirements

### Stakeholders

This section describes key stakeholders of the system and their architectural concerns.

* **SH-1**: **Manager** (availability, performance, scalability, security)
    - managers want the system they're accessing is available anytime they want to use it, and that it responses quickly to their actions;

* **SH-2**: **Analytics** (availability, performance, scalability, security)
    - they want receive analytics data to make forecust or some analysis

* **SH-3**: **Receipt Provider** (availability, security, reability)
    - some retail store or other receipt provider. they want that their receipt could be proccessed at any time

### Functional Requirements

* **UC-1**: **Upload Receipt**:
    - receipt provider (SH-3) uploads receipt to receipt manager system by REST API call, it varified and upload to tax service and logistic service;

* **UC-2**: **Receive analytics**:
    - analytics can receive data for a long term by REST API call (SH-2);

* **UC-3**: **Get receipt status**:
    - manger what to check status of receipt at any time by REST API call (SH-1)

* **UC-4**: **Request Limiter for Receipt Provider**:
    - RP can not send more receipts per day than is in its Subscription Plans 

### Architecture Characteristics Requirements

* **QA-1**: **scalability** (UC-1)
    - number of users  > 10000;

* **QA-2**: **availability** (UC-1, UC-3)
    - system should processed 1000RPS at high peak time;
    - 99.9% seems reasonable here;

* **QA-3**: **performance** (UC-1)
    - response time to get status of receipt < 0.5 sec;

* **QA-4**: **performance** (UC-2)
    - response time of analytics  < 30 sec;

* **QA-5**: **sequrity** (UC-2)
    - only authorized user should have access to service

### Constraints
* **CON-1**: Integration with AWS. Serverless

### Concerns
* **CONC-1**: We need to process 1000RPS. Potentially it could be 1000RPS-2000RPS to Receipt Database because proccess is durable. 
* **CONC-2**: Let's asuume that regular hours will be from 7-12 and 21-24 hours, high peak load from 12-21. 8h*200 + 9h*1000 = 6.4M +36M = 43M Receipts/Day. if size of receipts is 2kB we need to store around 4kB (original data and metadata). So Receipt database will grow by 172GB (43M * 4kB) per day. 
* **CONC-3**: Data warehouse should store for a last 20 years. It is huge vvalue. Could be issue with analytics


## Target Architecture
This section describes the target software architecture.

The architectural style used here as the bases is Serverless architecture (see [ADR-1](ADR/ADR-1-serverless.md) for details).

![Containers](images/Receipts.png "Target Architecture")


### Risk Analysis
These are the possible high risks of the transition architecture.

#### Availability
A single API Gateway may introduce a single point of failure for the whole system.


## Architecture Decision Records

> *Why is more important than how.  
Second Law of Software Architecture*

 - [ADR-1](ADR/ADR-1-serverless.md) Use Serverless architectural style as the basic style.
 - [ADR-2](ADR/ADR-2-event-driven-broker.md) Use message queues with guaranteed delivery for processing steps.
 - [ADR-3](ADR/ADR-3-separate-storages.md) Separate receipts and analytics storages.
 - [ADR-4](ADR/ADR-4-separate-receipt-db.md) Use separate receipt database.
 - [ADR-5](ADR/ADR-5-use-dynamodb.md) Use DynamoDb as Receipt Storage.
 - [ADR-6](ADR/ADR-6-use-s3-as-warehouse.md) Use S3 as data warehouse.
 - [ADR-7](ADR/ADR-7-use-athena-for-reporting.md) Use AWS Athena to get analytics
 - [ADR-8](ADR/ADR-8-use-aws-cognito.md) Use AWS Cognito.
 - [ADR-9](ADR/ADR-9-requests-limiter.md) Request Limiter.

## API Design
Send Receipt:

/receipts

Request Method:

POST

Request Headers:

    - Authorisation: Bearer {access_token}
    - ProviderId: {providerId}

Request Body:

    {
        "DateTime": 11-11-2023,
        "ReceiptBody" : "jsnon or xml"
    }

Request Response:

    {
        "ReceiptId": GUID
    }


Get Receipt's Status:

/receipts/{receipt_id}/status

Request Method:

GET

Request Headers:

    - Authorisation: Bearer {access_token}

Request Response:

    {
        "ReceiptId": GUID,
        "Status": "Created"
    }


/receipts/report?report_type={report_type}&params

Request Method:

GET

Request Headers:

    - Authorisation: Bearer {access_token}

Request Response:

    {        
    }
