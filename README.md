
# Analytics

Design for Analytics back-end system (similar to Google Analytics).

Handle large write volume: Billions of write events per day. - The orchestration layers separates writes from reads (CQRS design patterns), it can stream incoming events/data to Apache Kafka. Apache Spark can read ingested data for stream processing, and provide discretized stream(or DStreadm - continuous stream of data) as a high level of abstraction. DStream is represented as a sequence of Resilient Distributed Datasets (RDD). The resulting RDD from Spark can be shared with Apache Ignite (distributed memory centric database and caching platform). Writed from Ignite can be persisted in Apache Cassandra 
 

Handle large read/query volume: Read/Query patterns are time-series related metrics - Orchestration service and separate the read/query logic, and provide the processed result via Apache Spark 

Following is an high level design of the system: 
![Untitled Diagram-2](https://user-images.githubusercontent.com/2815448/114816513-47c1e500-9d86-11eb-82a7-5742ca7c961e.png)

The request for tracking data can be secured using OAuth2.0, and expected to have a JWT token with claims for authentication server (for example - AWS Cognito or Okta). The claims can contain further validation data, such as `tenant_id` indicating organizations (if supporting multitenancy), `api_key` - if this is a subscription based model and need an API key or license number issued to the client. 

The data model for sending the data can contain details about the event and metada, for example;

```javascript
{
 'data': {
  'correlation_id': 1234,
  'user_id': ab11,
  'event_id': 8888,
  'operation_id': 2133,
  'timestamp': 1209342938,
  'further_data': {
    'some': 'additional data',
    'about': 'the',
    'event': 1,
   }
  }
}

```

The orchestration service can be gRPC based service implemented in language - one of python, java/JVM or golan. 

