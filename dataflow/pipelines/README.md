# cloud-dataflow-samples-java

## dataflow

Example streaming dataflow pipelines for processing Wikipedia Edits, and correlating Stock and News feeds.


## Prerequisites

Download the dataflow-streaming SDK, and copy the pipeline examples to
cloud-dataflow/src/main/java/com/google/cloud/dataflow/examples/ directory in the SDK.

Compile the SDK:

```
$ mvn clean compile bundle:bundle
```

### To run the Wikipedia pipeline:

Create a Pubsub topic for input and output:
https://cloud.google.com/pubsub/v1beta1/topics/create#try-it

Setup a BigQuery dataset using the UI or the bq command-line tool.

Run the pipeline with the following command, specifying the correct project, staging location, topics and dataset/tables:

```
$  java -cp target/examples-1.jar com.google.cloud.dataflow.examples.StreamingWikipediaExtract --runner=DataflowPipelineRunner --project=$PROJECT --stagingLocation=gs://$PROJJECT/staging-$USER --inputTopic=/topics/$PROJECT/$INPUT_TOPIC --outputTopic=/topics/$PROJECT/$OUTPUT_TOPIC --dataset=$DATASET --table=$TABLE

```

Once the pipeline is deployed, run the Wikipedia injector:

```
$ java -cp target/examples-1.jar com.google.cloud.dataflow.examples.WebSocketInjectorStub /topics/$PROJECT/$INPUT_TOPIC

```


### To run the Stock-News pipeline:

Create a Pubsub topic for News-Input and one for Stocks-Input.
For a given prefix '$TOPIC_PREFIX', the topics should have the following names:

news: $TOPIC_PREFIX_news

stock: $TOPIC_PREFIX_stocks

They can be created here:
https://cloud.google.com/pubsub/v1beta1/topics/create#try-it

Setup a BigQuery dataset using the UI or the bq command-line tool.

Run the pipeline with the following command, specifying the correct project, staging location, topic-prefix and dataset/tables:

```
$  java -cp target/examples-1.jar com.google.cloud.dataflow.examples.StreamingStocksNewsCorrelate --runner=DataflowPipelineRunner --project=google.com:clouddfe --stagingLocation=gs://$PROJJECT/staging-$USER --inputTopic=/topics/$PROJECT/$TOPIC_PREFIX --dataset=$DATASET --table=$TABLE

```

Once the pipeline is deployed, run the stock injector:

```
$ java -cp target/examples-1.jar com.google.cloud.dataflow.examples.StockInjector /topics/$PROJECT/$TOPIC_PREFIX_stocks

```

Also separately run the news injector:

```
$ java -cp target/examples-1.jar com.google.cloud.dataflow.examples.NewsInjector /topics/$PROJECT/$TOPIC_PREFIX_news

```








