---
tags:
  - Kafka
  - ksqlDB
  - ksql
alias:
  - ksql
---
# Why
- ksqlDB allows you to build stream processing applications on top of Apache Kafka with the ease of building traditional applications on a relational database. Using SQL to describe _what_ you want to do rather than _how_, it makes it easy to build Kafka-native applications for processing streams of real-time data. Some key ksqlDB use cases include:
	- Materialized caches
	- Streaming ETL pipelines
	- Event-driven [[microservices]]
- ksqlDB is designed from the principle of simplicity. While many streaming architectures require a grab-bag of components pieced together from many projects, ksqlDB provides a single platform on which you can build streaming ETL and streaming applications, and all with just a single dependency - [[Apache Kafka]].

![[../../../../../_images/Kafka/KSQLDB.png]]

# Topology
- ksqlDB separates its distributed compute layer from its distributed storage layer, for which it uses Apache Kafka.

![[../../../../../_images/Kafka/ksqldb-kafka.png]]

- ksqlDB allows us to read, filter, transform, or otherwise process streams and tables of events, which are backed by Kafka topics. We can also join streams and/or tables to meet the needs of our application. And we can do all of this using familiar SQL syntax.

![[../../../../../_images/Kafka/ksqldb-computer.png]]

- ksqlDB can also build stateful aggregations on event streams. How many orders have been placed in the last hour? Errors in the last five minutes? Current balance on an account? These aggregates are held in a state store within ksqlDB, and external applications can query the store directly.

# Glossaries

- Stream -> tương ứng với khái niệm table trong [[Relational Database|RDBMS]]. Dữ liệu của stream được lấy từ [[Apache Kafka|Kafka]] topic. Stream sẽ xử lý dữ liệu theo dạng event
- Table -> Cũng giống như stream, lấy data từ Kafka topic. Tuy nhiên dữ liệu của table sẽ được **reduce by key**.
# Interact with ksqlSB
- ksqlDB có thể tương tác thông qua 
	- [[CLI]]
	- [[../../../../../Software Engineering/Web Development/API |API]]
	- Web UI
	- Client Library

## Creating, Exporting, and Importing Data
- Tron ksqlDB, 1 stream tương đương với table trong [[Relational Database]]. Là 1 đon vị logical, nhóm các records có chung ý nghĩa.
- Stream tương đương với Kafka topic -> dữ liệu ghi vào [[Apache Kafka|Kafka]] cũng sẽ được lưu vào Kafka topic và ngược lại
- Để tạo stream trong ksqlDB có thể dùng các cách sau:
  
	- You can insert directly into ksqlDB streams or tables using the CLI. Hoặc cũng có thể thông qua client, Web UI, 
	  	  
	``` sql
INSERT INTO orders(product, quantity, price) VALUES ('widget', 3, 19.99);
	```

	- #### Streams/Tables Backed by an Existing Kafka Topic
		- If you have data in an existing Apache Kafka topic, you can create a stream or a table backed by that topic and begin streaming the data into ksqlDB:

		``` sql
	CREATE STREAM people WITH(KAFKA_TOPIC='topic1',VALUE_FORMAT='AVRO');
		```

		- Any subsequent data produced to the topic will be streamed into ksqlDB, and any data inserted into the new stream will be written to the Kafka topic automagically.

	- #### Streams/Tables with a New Kafka Topic

		- If you don't have an existing topic, you can create a new stream or table, and ksqlDB will create the backing topic for you.

		``` sql 
	CREATE TABLE departments(id INT PRIMARY KEY,name VARCHAR) WITH 
	(KAFKA_TOPIC='dept_topic', PARTITIONS=3,VALUE_FORMAT='AVRO')
		```

		- the two-way data flow is automatic.

	- #### Importing and Exporting Data from Other Systems Using ksqlDB

		- Using [[Kafka Connect]] để sync dữ liệu từ source về [[Apache Kafka|Kafka]] rồi từ đó đưa về ksqlSB
		
		``` sql
	CREATE SOURCE CONNECTOR SOURCE_MONGODB_UNIFI_01 WITH (
		'connector.class' = 'io.debezium.connector.mongodb.MongoDbConnector',  
		'mongodb.hosts' = 'rs0/mongodb:27017',
		'mongodb.name' = 'unifi',
		'collection.whitelist' = 'ace.device, ace.user'
	);
		```
		
		- Việc sử dụng [[Kafka Connect]] cũng giúp ksqlDB đẩy dữ liệu ra các destination.

		``` sql
	CREATE SINK CONNECTOR SINK_ELASTIC_ORDERS_01 WITH (
		'connector.class' = 'io.confluent.connect.elasticsearch.ElasticsearchSinkConnector',
	    'topics' = 'ORDERS_ENRICHED',
	    'Connection.url' = 'http://elasticsearch:9200',
	    'type.name' = '_doc'
	);
		```

## Filtering with ksqlDB
- Lọc ra các records theo yêu cầu (phép chọn trong [[../../../../../Math/Relational Algebra]])
- syntax giống với [[SQL]]

![[../../../../../_images/Kafka/filter_ksqldb.png]]

## Lookups and Joins

- When processing data, a common requirement is to enrich it with other data. These lookups can be done in ksqlDB using the [[SQL]] `JOIN` syntax. Joins can be between streams of events, between streams and tables, or between tables and tables.

![[../../../../../_images/Kafka/ksqldb-join.png]]

- Ví dụ:

``` sql
CREATE STREAM ORDERS_ENRICHED AS
SELECT O.*,
       I.*,
       O.ORDERUNITS * I.UNIT_COST AS TOTAL_ORDER_VALUE,
  FROM ORDERS O
       LEFT OUTER JOIN ITEMS I
       ON O.ITEMID = I.ID ;
```

## Transforming Data with ksqlDB

- ksqlDB allows you to transform events in one stream and then send them to a new stream.
- ksqlDB can be used to perform many common transformations required when handling data, including:
	- Changing data types (`CAST`)
	- Reformatting date/time fields (`TIMESTAMPTOSTRING`)
	- Changing field names (`AS`)
	- Dropping fields
	- Concatenating fields (`CONCAT`)
- When creating a new stream from an existing one, you can pick and choose the fields you’d like to preserve in the new stream. You can also use functions to modify data or to create derived fields.
- Ví dụ: Chuyển từ linux timestamp sang timestamp 

![[../../../../../_images/Kafka/ksql-transform-timestamp.png]]

## Flatten Nested Records with ksqlDB

- ksqlDB can work with complex types, including `STRUCT` and `ARRAY`, as well as primitive types.
- Since nested JSON can be difficult to work with, ksqlDB makes it easy to transform the schema of an event stream. You can convert nested fields into top-level fields and leave the nested field out of the SELECT for a new stream. The original stream is not changed, so downstream operations that are expecting the original data structure are not affected.
- To access a nested field in ksqlDB, use `->`

``` sql
	ksql> SELECT ADDRESS->STREET FROM ORDERS 
	       EMIT CHANGES LIMIT 1;
	+-------------------+
	|STREET             |
	+-------------------+
	|52994 Debra Plaza  |
```

- Using this approach, you can create a flattened version of a source stream that contains nested fields:

``` sql
CREATE STREAM ORDERS_FLAT AS
SELECT TIMESTAMPTOSTRING(ORDERTIME, 'yyyy-MM-dd HH:mm:ss') AS ORDER_TIMESTAMP,
	    ORDERID,
		ITEMID,
		ORDERUNITS,
		ADDRESS->STREET AS ADDRESS_STREET,
		ADDRESS->CITY   AS ADDRESS_CITY,
		ADDRESS->STATE  AS ADDRESS_STATE
	FROM ORDERS;
```

## Converting Data Formats with ksqlDB

- There are different ways to serialize data written to Apache Kafka topics. Common options include Avro, Protobuf, and JSON.
- You can use ksqlDB to create a new stream of data identical to the source but serialized differently. This can be useful in several cases:
	- If the source stream is JSON or delimited (CSV), then it does not have an explicit schema, which can make it more difficult for consumers to work with. Using ksqlDB, you can apply a schema to the data and write it in a format more suitable for all consumers.
	- You may be using a format, such as Avro, but a consuming application may insist on another format, such as delimited (CSV).
- To write a stream of data from its CSV source to a stream using Protobuf, you would first declare the schema of the CSV data:

``` sql
CREATE STREAM source_csv_stream 
	(ITEM_ID INT, 
	 DESCRIPTION VARCHAR, 
	 UNIT_COST DOUBLE, 
	 COLOUR VARCHAR, 
	 HEIGHT_CM INT, 
	 WIDTH_CM INT, 
	 DEPTH_CM INT
	) WITH (KAFKA_TOPIC ='source_topic', 
		VALUE_FORMAT='DELIMITED');
```

- Then you would use a continuous query to write all of these events to a new ksqlDB stream serialized as Protobuf:

``` sql
	CREATE STREAM target_proto_stream 
	WITH (VALUE_FORMAT='PROTOBUF') 
	AS SELECT * FROM source_csv_stream
```

## Merging Two Streams

- ksqlDB can write data from multiple streams into a single target. This can be useful when you have events for the same logical entity (for example, orders) written to separate topics (perhaps originating in different Apache Kafka clusters or instances of the producing application). At the same time, you can add in or modify the data to ensure that attributes such as identifiers remain unique.

![[../../../../../_images/Kafka/merge_stream.png]]

## Splitting Streams
- Ngược lại với merge stream
- Tạo Stream bằng cách select data to stream mới.

![[../../../../../_images/Kafka/split_stream.png]]

## Streams and Tables
- While relational databases center around the concept of a table, ksqlDB has two first-class object types: streams and tables.
- Streams are unbounded series of events, while tables are the current state of a given key.

![[../../../../../_images/Kafka/stream-vs-table.png]]

- Both streams and tables are built from Apache Kafka topics; the difference is only the semantic interpretation of the data. Which you choose is determined by how you want to use the data.
- Taking the chess board example in the graphic above, a single Kafka topic would hold the history of all the moves.
	- A ksqlDB **stream** on that topic would apply a schema to it, and from that you could replay some or all of the game, or identify any events involving a specific piece or square.
	- A ksqlDB **table** on the same topic would apply a schema to it and tell you the **current location of each piece**. Each time a piece moves (and a new event is written to the Kafka topic), the table updates its state
- More at: [Streams and Tables in Apache Kafka: A Primer (confluent.io)](https://www.confluent.io/blog/kafka-streams-tables-part-1-event-streaming/)

## Stateful Aggregations (Materialized Views)

- ksqlDB can filter, join, and enrich events as they happen. But you may wish to look beyond individual events to a composite view. This is where aggregation comes in.
- You can use ksqlDB to build a materialized view of state, driven by the events in an Apache Kafka topic. This is done using SQL aggregation functions, such as `COUNT` and `SUM`.
- In the example of order data, you can look at how many orders were placed in a given timeframe, their average value, or the total value. These materialized views are stored within ksqlDB in an embedded RocksDB instance, and are also backed by Kafka topics. This means that you can query them directly using ksqlDB, and you can also use ksqlDB to precalculate aggregates that you want to consume from the topic and use in another system, such as for analytics.
- Aggregates can be calculated over the entire stream of data, or more usefully, over a windowed period. ksqlDB supports a number of different window types, including hopping, tumbling, and session. The data returned from an aggregation is always a table, with the key being the field or fields in the `GROUP BY` clause. You can wrap your query in a `CREATE TABLE AS` statement to make it persistent.
- Ví dụ

``` sql
CREATE TABLE ORDERS_PER_HOUR_BY_MAKE AS
  SELECT MAKE,
         COUNT(*) AS ORDER_COUNT,
         CAST(SUM(TOTAL_ORDER_VALUE) AS DECIMAL(9,2)) AS TOTAL_ORDER_VALUE
    FROM ORDERS_ENRICHED
          WINDOW TUMBLING (SIZE 1 HOUR)
  GROUP BY MAKE
  EMIT CHANGES;
```

## Push Queries and Pull Queries

- There are two ways of querying data in ksqlDB: _push queries_ and _pull queries_.
	- **Push queries** are identified by the `EMIT CHANGES` clause. By running a push query, the client will receive a message for every change that occurs on the stream (that is, every new message).
	- **Pull queries** return the current state to the client, and then terminate. In that sense, they are much more akin to a `SELECT` statement executed on a regular RDBMS. They can only be used against ksqlDB tables with materialized state, that is, a table in which there is an aggregate function. Currently, tables declared against an existing Apache Kafka topic cannot be queried with a pull query (as of ksqlDB v0.15 / February 2021)

## Apply Lambda Functions to Arrays and Maps

### Transform Function
- A transform will change the key or value of every element in a `MAP` or `ARRAY`:

```sql
CREATE STREAM transformed 
  AS SELECT id,
  TRANSFORM(map_field,(k,v)=> UCASE(k), (k, v)=> (v * 2))
FROM stream1 EMIT CHANGES;
```

- Note that for a `MAP` field, two lambda expressions are required: one for the key and one for the value. If no change is required for the key or the value, the lambda can just return the existing data.

### Reduce Function
- Reduce will take all of the elements from an `ARRAY` or `MAP` and produce a single result, based on the lambda expression:

```sql
CREATE STREAM reduced 
  AS SELECT name,
  REDUCE(scores,0,(s,x)=> (s+x)) AS total
FROM stream2 EMIT CHANGES;
```

- For an `ARRAY`, the lambda takes two arguments: the `ARRAY` element and an accumulator. For a `MAP`, the lambda takes three arguments: the key, the value, and an accumulator.

### Filter Function
- The filter function takes an `ARRAY` or `MAP` and returns the same type, but filled with only the elements that meet the filter's criteria, which are given in the lambda expression.

```sql
CREATE STREAM filtered 
  AS SELECT id,
  FILTER(numbers,x => (x%2 = 0)) AS even_numbers
FROM stream3 EMIT CHANGES;
```



# Under the Covers

- ksqlDB built on top of [[Kafka Stream]]. và cũng giống như [[Kafka Stream]], ksqlDB sử dụng [[../../../../Database/Others databases/RocksDB]] để lưu các state
- Là distributed system giống [[Apache Kafka|Kafka]]. Sử dụng cơ chế partition để scale out.

# ksql Architecture

- A typical streaming pipeline consists of source databases and apps creating events that feed into Apache Kafka via connectors, a stream processor that filters and aggregates the events, and finally storage facilities that store the events for access by analytics engines.

![[../../../../../_images/Kafka/ksqldb-architecture.png]]

- This is a standard system design, and it works well, but it does consist of a whole host of moving parts that are prone to breakage or failure and that make scaling, securing, monitoring, debugging, and operating all as one difficult.
- An architecture with ksqlDB is much simpler, as many of those external parts have been consolidated into the tool itself: ksqlDB has primitives for connectors, and it performs stream processing. It also features materialized views, so that your data can be queried just like a database table directly in ksqlDB, without needing to be sent to an external source:

![[../../../../../_images/Kafka/ksqldb-database.png]]

- In fact, ksqlDB tucks away complexity in a manner similar to classic [[Relational Database/Relational Database|RDBMS]] such as [[PostgreSQL]], which hides query execution, indexing, concurrency control, and crash recovery behind an intuitive interface.
- Because ksqlDB relies on [[Apache Kafka|Kafka]] to maintain state, you ultimately end up with a simple two-tier architecture: compute and storage—ksqlDB and Kafka. And each can be elastically scaled independently of the other.
- You can interact with a single ksqlDB server or multiple servers using a client-server [[CLI]] setup, like with a relational database, or via a full-featured web UI, a [[REST]] [[API]], or a client for a programming language such as [[Java]].