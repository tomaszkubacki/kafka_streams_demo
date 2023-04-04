### Kafka Streams explained

Kafka streams is a Java library to process data from Kafka topics and save data to kafka topics

```mermaid
flowchart TB
    subgraph Java application
        KStream{{KStream}}:::someclass
    end
        IN --> KStream
        KStream --> OUT
    subgraph Kafka
        direction LR
        IN(input topic) 
        OUT(output topic)
    end
        
    click IN callback "Kafka Topic: input-topic"
    click OUT callback "Kafka Topic: output-topic"
    click KStream callback "Kafka streams in app memory"
    classDef someclass fill: #0000ff, color: white
```

## Kafka Streams artifacts

KStreams is a stream of data in created in the app

### KStreams

> KStream definition
: is an abstraction of partitioned record stream, in which data is represented using insert semantics 
> i.e. each record is independent of other events 

As a developer you can consider KStream as never ending (unbound) event log, 
which you can modify using kafka stream operations.

important properties:
- always insert only
- similar to Log
- unbounded data stream

what does the unbound mean ?
: having start but not end - being possibly infinite at the end


| Topic(key, value) | KStream             |
|-------------------|---------------------|
| (a,1)             | (a,1)               |
| (b,1)             | (a,1), (b,1)        | 
| (a,2)             | (a,1), (b,1), (a,2) |


### KTable


> KTable
: is a table of key value records, which are upserted 

- upserts on non null values
- deletes on null values
- similar to table
- parallel with log compacted topics


| Topic(key, value) | KStream             |
|-------------------|---------------------|
| (a,1)             | (a,1)               |
| (b,1)             | (a,1), (b,1)        | 
| (a,2)             | (a,2), (b,1)        |
| (b,2)             | (a,2), (b,2)        |
| (c,3)             | (a,2), (b,2), (c,3) |
| (a,null)          | (b,2), (c,3)        |


### KStream/KTable most popular operations

### Map values and Map

Mapping values in record stream

*mapValue* and *map*

eg. 

```jshelllanguage
// using lambda expression on KStream<byte[],String>
stream.mapValues(value -> value.toUpperCase());
```

repartitioning operations
: mapValues is cheaper than map, since map is causing repartitioning, 
  thus use mapValues whenever possible

### Filter and filterNot

Filtering records in stream

e.g. remove all records with value lower than 3

```jshelllanguage
// using lambda expression on KStream<byte[],String>
stream.filter((key, value) -> value >= 3);
```

### Flatmap

Takes one record and produces zero one or more records

e.g. remove all messages with value lower than 3

```jshelllanguage
// using lambda expression on KStream<byte[],String>
stream.flatMapValues(value -> value.split("\\s+"));
```

### Split

Split operation is a successor of branch operation

TODO: add split example
