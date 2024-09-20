# Setting up the stack

## The Workflow

* Data Ingestion: We use sockets to stream data from the dataset to simulate live capture data capture and push it to socket consumer.
* Data Processing: Spark Streaming consumes the data from the socket using the socket readstream API, processes it, sends chunks of text to OpenAI then eventually streaming it to our Kafka cluster.
* Sentiment Analysis: We leverage OpenAI analyzes the sentiment of each chunk and returns the sentiment analysis of each of the review.
* Data Storage: The sentiment analysis, along with the original data, is stored in Kafka and streamed into Elasticsearch with Kafka Elasticsearch Sink connector for indexing.

### Kafka

1- Create a basic kafka cluster on confluent.io.
2- Configure the cluster(Name, Cloud Provider, Region, Availability).
3- Launch the cluster.
4- Access the cluster details:<br>
* After your cluster is created, click on its name from the dashboard to access its details. Here, you can find important information like the broker endpoints, cluster API keys, and other configurations.
* Navigate to the API Keys section and click on Create Key. Follow the prompts to generate a new API key and secret. 
* Set Up Topics (Optional): If you know the topics you’ll be using, navigate to the Topics section and click on Create Topic. Provide the necessary details and configurations for your topic.

You may be asked to set a schema, we’ll be creating the value schema in the schema registry to ensure data consistency and compatibility as schemas evolve.
```json
        {
    "type": "record",
    "name": "customers_reviews_schema",
    "namespace": "com.airscholar",
    "doc": "Schema for customer reviews",
    "fields": [
        { "name": "review_id",  "type": "string" },
        { "name": "user_id",  "type": "string" },
        { "name": "business_id", "type": "string" },
        { "name": "stars", "type": "float" },
        { "name": "date", "type": "string" },
        { "name": "text",  "type": "string" },
        { "name": "feedback",  "type": "string" }
    ]
    }
```
With your cluster up and the API keys generated, you can now connect your applications, producers, and consumers to your Kafka cluster using the provided broker endpoints ( cluster settings >> endpoints) and credentials (from the API Keys section)

### Spark

### OpenAI

### Elasticsearch