# Why ?
- Là 1 [[Java]] library -> dùng trong [[Java]] Application
- Dùng để xử lý dữ liệu streaming từ [[Apache Kafka]]
- Là 1 library native với Kafka

# Topology
- Kafka Stream topology is [[DAG]]
  
  ![[../../../../../_images/Kafka/kafka_stream_topology.png]]
- Mỗi luồng xử lý (DAG) trong Kafka stream bao gồm các thành phần
	- Source node: -> Nơi nhận data từ Kafka, sau đó forward cho các Stream node
	- Stream node: define custom logic -> nhiệm vụ xử lý dữ liệu
	- Sink node: send data back to [[Apache Kafka]]
	- Một node có thể có nhiều node cha và nhiều node con
## Basic Operations

### Streams
- Để khởi tạo 1 stream, dùng <span style="color:pink">StreamBuilder</span>
  
```
StreamBuilder builder = new StreamBuilder();
KStream<String, String> firstStream = 
builder.stream(inputTopic, Consumed.with(Serdes.String(), Serdes.String()));
```

#### Map
- Hỗ trợ 2 hàm:
  
	- mapValues: dùng để biến đổi values trong 1 cặp key-value:
	  
	  ![[../../../../../_images/Kafka/kafka_stream_mapping_operation.png]]

  ```
  mapValues(value -> value.substring(5))
  ```
	
	- map: nhận input là 1 cặp key-values -> tạo ra 1 cặp mới
	  
```
	map((key, value) -> ...)
```

#### Filter

![[../../../../../_images/Kafka/kafka_stream_filter-operation.png]]
Lọc ra nhưng giá trị phù hợp và gửi xuống các node tiếp theo trong [[DAG]]

## KTable
- Dùng trong trường hợp của update stream(Update các giá trị có cùng key) -> Key is required
- Dữ liệu cũ sẽ bị flushed sau một khoảng thời gian(default: 30s)
- Để tạo 1 Ktable, dùng StreamBuilder:
  
``` java
StreamBuilder builder = new StreamBuilder();
KTable<String, String> firstKTable = 
    builder.table(inputTopic, 
    Materialized.with(Serdes.String(), Serdes.String()));
```

### Operations
- KTable cũng có các [[../../../../../Software Engineering/Web Development/API]] giống như KStream bao gồm:
	- Map
	- Filter

### GlobalKTable
- Khởi tạo:
  
``` java
StreamBuilder builder = new StreamBuilder();
GlobalKTable<String, String> globalKTable = 
    builder.globalTable(inputTopic, 
    Materialized.with(Serdes.String(), Serdes.String()));
```

- **KTable** vs **GlobalKTable**
	- The main difference between a KTable and a GlobalKTable is that a KTable shards data between Kafka Streams instances, while a GlobalKTable extends a full copy of the data to each instance

## Joins

### Stream - Stream

- Stream-stream joins kết hợp 2 stream thành 1 stream mới. join operator được thực hiện dựa trên key -> giá trị của key key là bắt buộc
- Sử dụng time window để lấy những message cần join (windowed join).
- Kết quả của join là 1 stream mới -> cặp (key, value) mới. Với key là key của 2 records, value được xác định thông qua `ValueJoiner`, 1 [[../../../../../Programming language/Java/Interface|Java Interface]] nhận 2 values từ 2 records trong join operator -> value mới.
- Hỗ trợ các loại join:
	- Inner joins
	- Outer joins
	- Left-Outer joins

### Stream - Table

- Non-windowed join
- Phép join được thực hiện chủ động ở phía stream. Tức là mỗi khí tream có records mới -> phép join sẽ được thực hiện, còn khi phía table có records mới thì không thực hiện join (this is the case for both KStream-KTable and KStream-GlobalKTable joins).
- Hỗ trợ:
	- Inner joins
	- Left-Outer joins

#### GlobalKTable-Stream Join Properties
- GlobalKTable provide a mechanism: Khi join với KStream, có thể join qua key hoặc value. Việc join operator sử dụng diều kiện nào được định nghĩa bởi `KeyValueMapper`
- Điểm khác giữa KTable join và GlobalKTable join: Global không dùng timestamp để xác định các records được join, tức là sẽ lookup toàn bộ GlobalKTable tại thời điểm thực hiện join operator không quan tâm đến timestamp của Stream side -> phù hợp với static join

### Table - Table
- Giống như join các table thông thường

## Stateful Operations

- Đối với các [[stateful]] operations, các thao tác processing thường sẽ yêu cầu group by key đầu tiên để tìm ra state -> key is required. Nếu message không có key, có thể tạo key cho message, nhưng cần cân nhắc nếu _Repartition_.
- [[Stateful]] oprations trong Kafka Stream dựa trên việc lưu trữ state. Cách thức lưu trữ state mặc định là dùng [[../../../../Database/Others databases/RocksDB]], ngoài ra có thể lưu trữ trực tiếp trên RAM. State stores lại được dựa trên changelog topic -> Kafka Stream có khả năng fault-tolerant. Khi call [[stateful]] operation, 1 KTable(overwrite old values) sẽ được return. [[Stateful]] operations bao gồm các hàm: 

	- _reduce_: takes one value type as a parameter, _reduce_ expects you to return the same type

	``` java
StreamsBuilder builder = new StreamsBuilder();
KStream<String, Long> myStream = builder.stream("topic-A");
Reducer<Long> reducer = (longValueOne, longValueTwo) -> longValueOne + longValueTwo;
myStream.groupByKey().reduce(reducer, Materialized.with(Serdes.String(),
							 Serdes.Long()))
							 .toStream().to("output-topic");
	```
	
	- _count_,
	- _aggregate_: is a form of _reduce_, nhưng có thể return different type.

	``` java
	StreamsBuilder builder = new StreamsBuilder();
	KStream<String, String> myStream = builder.stream("topic-A");
	Aggregator<String, String, Long> characterCountAgg = 
	    (key, value, charCount) -> value.length() + charCount;
	myStream.groupByKey().aggregate(() -> 0L, 
	                                      characterCountAgg, 
	                                      Materialized.with(Serdes.String(), Serdes.Long()))
	                                      .toStream().to("output-topic");
	```


### Considerations
- [[Stateful]] operations không trả về kết quả ngay lập tức. Kafka Stream sẽ cache result. Có 2 yếu tố có thể tác động tới việc lấy result từ cache:
	- Khi cache đầy (defined equally per instance among the number of stores = 10MB).
	- Kafka Stream sẽ commit mỗi 30s(default) -> dữ liệu được update mỗi 30s
	=> Có thể set cache size = 0 để result sẽ được emit ngay lập tức không phải chờ 30s hay cache đầy.
- Even with caching, you will get multiple results, so for a single and final stateful result, you should use suppression overloads with `aggregate/reduce` operations.

## Windowing

- Windowing allows you to bucket stateful operations by time, without which your aggregations would endlessly accumulate. A window gives you a snapshot of an aggregate within a given timeframe, and can be set as hopping, tumbling, session, or sliding.

### 1. Hopping
- A hopping window is bound by time: You define the size of the window, but then it advances in increments smaller than the window size, so you end up with overlapping windows. You might have a window size of 30 seconds with an advance size of five seconds. Data points can belong to more than one window.

### 2. Tumbling
- A tumbling window is a special type of hopping window:
- It's a hopping window with an advance size that's the same as its window size. So basically you just define a window size of 30 seconds. When 30 seconds are up, you get a new window with a time of 30 seconds. So you don't get duplicate results like you do with the overlapping in hopping windows.

### 3. Sesion
- Session windows are different from the previous two types because they aren't defined by time, but rather by user activity:
- So with session windows, you define an `inactivityGap`. Basically, as long as a record comes in within the `inactivityGap`, your session keeps growing. So theoretically, if you're keeping track of something and it's a very active key, your session will continue to grow.

### 4. Sliding
- A sliding window is bound by time, but its endpoints are determined by user activity. So you create your stream and set a maximum time difference between two records that will allow them to be included in the first window.
- The window doesn't continuously advance, as with a hopping window, but advances based on user activity.

### 5. Grace Periods
- With the exception of session windows, which are behavior-driven, all windows have the concept of a _grace period_. A grace period is an extension to the size of a window. Specifically, it allows events with timestamps greater than the window-end (but less than the window-end plus the grace period) to be included in the windowed calculation.


## Time concepts
### Events time
- With the exception of session windows, which are behavior-driven, all windows have the concept of a _grace period_. A grace period is an extension to the size of a window. Specifically, it allows events with timestamps greater than the window-end (but less than the window-end plus the grace period) to be included in the windowed calculation.

### Log-Append time
- With log-append time, when the record arrives at the broker, the broker will override the timestamp of the producer record with its own timestamp (the current time of the broker environment) as it appends the record to the log.

### Timestamps Drive the Action in Kafka Streams
- The windowing operations that you learned about in the [Windowing module](https://developer.confluent.io/learn-kafka/kafka-streams/windowing/ "Windowing") are driven by record timestamps, not by wall-clock time. In Kafka Streams, the earliest timestamp across all partitions is chosen first for processing, and Kafka Streams uses the `TimeStampExtractor` interface to get the timestamp from the current record.
- The default behavior is to use the timestamp from a `ConsumerRecord`, which has a timestamp set by either the producer or the broker. The default implementation of `TimeStampExtractor` is `FailOnInvalidTimestamp`, which means that if you get a timestamp less than zero, it will throw an exception. If you want to use a timestamp that is embedded in the record key or value itself, you can provide a custom `TimeStampExtractor`.

### Stream Time
- Kafka Streams uses the concept of _stream time_:

![[../../../../../_images/Kafka/stream-time-1.png]]
- Stream time, by definition, is the largest timestamp seen so far, and it only moves forward, not backward. If an _out-of-order_ record arrives (meaning a record that is earlier than the current stream time, but still within the window plus the grace period), stream time stays where it is.
- _Late_ records have timestamps outside of the combined window time and grace period. The delay of a record is determined by taking the stream time minus the event timestamp.

![[../../../../../_images/Kafka/late-record-1.png]]

## Processor API

- So far you've learned about the Kafka Streams DSL, which gives you maximum developer velocity when building an application, but is very opinionated. Sometimes you need to accomplish things that aren't provided to you by the DSL, and that's where the Processor API comes in. It gives you maximum flexibility, but you are responsible for all of the details.
- The Processor API gives you access to state stores for custom stateful operations. In the [[Stateful]] Operations module, you learned about state and the DSL, which uses under-the-hood state stores that you can't directly access. With the Processor API, you create a store that you pass in, so you have full access to the store: You can pull out all of the records, you can do a range query, you can do all sorts of things. You can also programmatically call _commit_, and you can control how often it's called.

### Punctuation

- Punctuation gives you the ability to schedule arbitrary operations. When you schedule a punctuator, it can use either stream-time processing or wall-clock-time processing.

#### Stream-Time Processing
- With stream-time processing, you schedule punctuation for an arbitrary action that you want to fire periodically—every 30 seconds, for example—and the action is driven by the timestamps on the records.

#### Wall-Clock-Time Processing
- You can also schedule punctuation according to wall-clock time. Under the hood, Kafka Streams uses a consumer, and consumers call `poll()` to get records. Wall-clock time advances with the `poll()` calls. So wall-clock time advancement depends in part on how long it takes you to return from a `poll()` loop.

## Processor [[../../../../../Software Engineering/Web Development/API]]

- So far you've learned about the Kafka Streams DSL, which gives you maximum developer velocity when building an application, but is very opinionated. Sometimes you need to accomplish things that aren't provided to you by the DSL, and that's where the Processor API comes in. It gives you maximum flexibility, but you are responsible for all of the details.
- The Processor API gives you access to state stores for custom stateful operations. In the [[Stateful]] Operations module, you learned about state and the DSL, which uses under-the-hood state stores that you can't directly access. With the Processor API, you create a store that you pass in, so you have full access to the store: You can pull out all of the records, you can do a range query, you can do all sorts of things. You can also programmatically call _commit_, and you can control how often it's called.

### Punctuation
- Punctuation gives you the ability to schedule arbitrary operations. When you schedule a punctuator, it can use either stream-time processing or wall-clock-time processing.

#### Stream-Time Processing
- With stream-time processing, you schedule punctuation for an arbitrary action that you want to fire periodically—every 30 seconds, for example—and the action is driven by the timestamps on the records.

#### Wall-Clock-Time Processing
- You can also schedule punctuation according to wall-clock time. Under the hood, Kafka Streams uses a consumer, and consumers call `poll()` to get records. Wall-clock time advances with the `poll()` calls. So wall-clock time advancement depends in part on how long it takes you to return from a `poll()` loop.

### Building Streams Applications with the Processor API

- Kafka Streams applications with the Processor API are typically built as follows:
	- Add source node(s)
	- Add N number of processors, child nodes of the source node(s) (child nodes can have any number of parents)
	- Optionally create StoreBuilder instances and attach them to the processor nodes to give them access to state stores
	- Add sink node(s), to which one or more parent nodes forward records
- When you create nodes, you need to provide a name for each one, since you use the name of one node as the parent name of another. As mentioned, a node can have more than one child node, but you can be selective about the nodes to which you forward records

#### Creating a Topology with the Processor API
- Creating a topology involves straightforward code:

``` java
Topology topology = new Topology();
topology.addSource(“source-node”, “topicA”, ”topicB”);
topology.addProcessor(“custom-processor”, new CustomProcessorSupplier(storeName), “source-node”);
toplogy.addSink(“sink-node”, “output-topic”, “custom-processor”);
```

- First you create an instance and add a source node. Then you add a custom processor, whose parent is the source node. Finally, you create a sink node, whose parent is the custom processor. For any one of these nodes, you can have multiple parent nodes and multiple child nodes—this is just a simple example.

# Internal

## Tasks
- Kafka Streams uses the concept of a _task_ as a work unit. It's the smallest unit of work within a Kafka Streams application instance. The number of tasks is driven by the number of input partitions. For example, if you have a Kafka Streams application that only subscribes to one topic, and that topic has six partitions, your Kafka Streams application would have six tasks. But if you have multiple topics, the application takes the highest partition count among the topics.

## Thread
- Tasks are assigned to `StreamThread`(s) for execution. The default Kafka Streams application has one `StreamThread`. So if you have five tasks and one `StreamThread`, that `StreamThread` will work records for each task in turn. However, in Kafka Streams, you can have as many threads as there are tasks. So for five tasks, you could configure your application to have five threads. Each task gets its own thread, and any remaining threads are idle.

![[../../../../../_images/Kafka/kafka_stream_tasks_topic.svg]]

## Instances
- With respect to task assignment, application instances are similar to tasks. If you have an input topic with five partitions, you could spin up five Kafka Streams instances that all have the same application ID, and each application would be assigned and process one task. Spinning up new applications provides the same increase in throughput as increasing threads. Just like with threads, if you spin up more application instances than tasks, the extra instances will be idle, although available for failover. The advantage is that this behavior is dynamic; it doesn't involve shutting anything down. You can spin up instances on the fly, and you can take down instances.

## Consumer Group Protocol
- Because Kafka Streams uses a Kafka consumer internally, it inherits the dynamic scaling properties of the _consumer group protocol_. So when a member leaves the group, it reassigns resources to the other active members. When a new member joins the group, it pulls resources from the existing members and gives them to the new member. And as mentioned, this can be done at runtime, without shutting down any currently running applications.
- So you should set up as many instances as you need until all of your tasks are accounted for. Then in times when traffic is reduced, you can take down instances and the resources will be automatically reassigned.

![[../../../../../_images/Kafka/kafka_stream_consumer_group_protocol.jpg]]

# Kafka Stream vs [[Apache Spark]] & [[Data/Data Engineering/Data Proccessing/Apache Flink]]
# Kafka Stream vs ksqlDB


# Refenrce
	1. A