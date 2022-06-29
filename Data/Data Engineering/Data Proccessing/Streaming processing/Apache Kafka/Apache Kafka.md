---
tags:
  - Apache
  - Kafka
  - Queues
alias:
  - Kafka
---
# Why ?

## Ưu điểm của Kafka
- Distributed, resilient architecture, fault tolerant
- Horizontal scalability
	- ~ 100 brokers $10^6$
	- -> $n*10^6$  messages / seconds
	- low latency  (~ mili second)
	- high performance

## Use cases
- Messaging system
- Activity tracking
- Gathers matrics for many locations
- Collect log
- Streaming processing

# Kafka Architecture
- Kafka core concept là event
- Ví dụ về event:
	- purchase order
	- payment
	- click on link ...
- Mỗi event là 1 record trong Kafka

![[../../../../../_images/Kafka/event-in-stream-processing.png]]

- Mỗi record bao gồm các thông tin:
	- timestamp
	- key (optional)
	- value
	- Headers (optional)
- Payload thường được nhúng trong value
- key thường được sử dụng cho các mục đích:
	- Enforcing ordering 
	- Xác định topic sẽ lưu record
	- key-based storage or compaction
- 

## Record Schema
- key và value đều lưu dưới dạng byte arrays -> dễ dàng encode bằng nhiều cách khác nhau sử dụng [[Serializer]] (tương ứng [[Deserializer]])
- Dạng format hay dùng nhất trong Kafka là [[Avro]]

## Kafka topics

![[../../../../../_images/Kafka/kafka-topics.png]]
- Tương đương với table trong [[Relational Database|RDBMS]] -> Stream of data
- It's a concept for organizing events of the same type, together
- Topics là immutable, append-only

## Topic Partitions

![[../../../../../_images/Kafka/partition-offsets.png]]

- Partiotion là thành phần con của topic, là phân chia lưu trữ về mặt logic của topic. 1 topic được chia tành nhiều partition => là cách thức distribute the storage and processing events in a topic.
- 1 topic có thể gồm >= 1 partitions (không giới hạn số lượng). Các partition này có thể ở trong cùng 1 node hoặc khác node
- Các messages được lưu trong 1 topic sẽ có id tăng dần, được gọi là offset -> unique identifier của message. Bắt đầu từ 0, *and offsets are never reused*. Giá trị offset này sẽ tăng dần với việc các message được thêm vào partition. Việc sử dụng offset đem lại nhiều tác dụng, đặc biệt là consumers keeping track of which events have been processed
- Thứ tự của các message trong partition sẽ được đảm bảo ordered về mặt thời gian nếu không tính đến lagging
- Partition là main unit trong việc lưu trữ. Partition cũng là main unit trong việc xử lý song song. Bởi vì: **Events can be produced to a topic in parallel by writing to multiple partitions at the same time. Likewise, consumers can spread their workload by individual consumer instances reading from different partitions. If we only used one partition, we could only effectively use one consumer instance**. => *dẫn đến việc*: nếu chỉ sử dụng 1 partition thì số lượng consumer > 1 không đem lại hiệu quả về mặt tính toán

## Broker

- Kafka cluster được tạo thành từ nhiều servers khác nhau. Mỗi server được gọi là 1 broker(*From a physical infrastructure standpoint, [[Apache Kafka]] is composed of a network of machines called brokers*). Broker là nơi lưu trữ và xử lý partition
- Mỗi broker được định danh bởi 1 số int, gọi là ID. Brokers là ngang hàng -> connect đến 1 broker bất kỳ là connect đến toàn bộ cluster.
- 1 Kafka cluster được chia là 2 thành phần:
	- *control plane*: handles management of all the metadata in the cluster
	- *data plane*: deals with the actual data that we are writing to and reading from Kafka

![[../../../../../_images/Kafka/inside-the-apache-kafka-broker.png]]

- Client requests đước chia làm 2 loại: produce requests and fetch requests. Procedure request tức là procude message -> ghi data vào topic. Fetch request thì ngược lại, là request lấy data từ topic. 2 loại request này đều phải trải qua 1 số bước khá giống nhau.

### *Produce requets*

#### _Partition Assignment_

- Mỗi khi producer gửi message bất kì đến Kafka, nó sẽ được phân chia vào 1 partition nhất định theo các ràng buộc sau:
	- If the record has a key -> key này sẽ được hash và rồi tính toán (thường là phép module) ra ra partition mà message sẽ được lưu vào. Với việc sử dụng [[Hash function|hash]] và các phép toán => message với cùng key sẽ luôn đi vào cùng 1 partition.
	- If the record has no key then a partition strategy is used to balance the data in the partitions (thường dùng [[Round robin]])

![[../../../../../_images/Kafka/assign-record-to-topic-partition.png]]

#### _Record batching_

![[../../../../../_images/Kafka/records-accumulated-into-record-batches.png]]

- Thông thường sending 1 records trong 1 request có thể dẫn đến overhead of repeated network requests -> Producer thường sẽ gom nhiều message chung 1 partition vào 1 lần gửi. Các message trong 1 batch được buffer lại trên [[RAM]] và được gọi là records batch (như hình trên). *Batching also provides for much more effective compression, when compression is used.*
- The producer has two main configurations that it uses to determine when to send batches to the broker:
	- Đầu tiên: `batch.size` -> xác định số lượng minimum message -> số message tối thiểu cần đạt để gửi request.
	- Thứ hai: `linger.ms` -> maximum amount of time to wait.
- Chỉ cần đạt được 1 trong 2 điều kiện trên là request sẽ được gửi tới broker.

#### *Network Thread Adds Request to Queue*

![[../../../../../_images/Kafka/network-thread-adds-request-to-queue.png]]

- Request sẽ được gửi đến broker’s socket. Sau đó 1 network thread từ pull sẽ tiếp nhận data và đưa vào xử lý. *Network thread này sẽ handle **mọi** request từ client đó*. *The network thread will read the data from the socket buffer, form it into a produce request object, and add it to the request queue*

#### *I/O Thread Verifies and Stores the Batch*

![[../../../../../_images/Kafka/io-thread-verifies-record-batch-and-stores.png]]

- Next, a thread from the I/O thread pool will pick up the request from the queue. I/O thread sẽ thực hiện các validation (bao gồm cả tính [[CRC|CRC checksum]]. Rồi sau đó sẽ append data to the physical data structure of the partition, được gọi là **commit log**.

#### Kafka Physical Storage

![[../../../../../_images/Kafka/kafka-physical-storage.png]]

+ On disk, the commit log is organized as a collection of segments. Each segment is made up of several files. One of these, a `.log` file, contains the event data. A second, a `.index` file, contains an index structure, which maps from a record offset to the position of that record in the `.log` file.

#### Purgatory Holds Requests Until Replicated

![[../../../../../_images/Kafka/purgatory-holds-requests-being-replicated.png]]

+ Since the log data is not flushed from the page cache to disk synchronously, Kafka relies on replication to multiple broker nodes, in order to provide durability. By default, the broker will not acknowledge the produce request until it has been replicated to other brokers.
+ To avoid tying up the I/O threads while waiting for the replication step to complete, the request object will be stored in a map-like data structure called purgatory (it’s where things go to wait).
+ Once the request has been fully replicated, the broker will take the request object out of purgatory, generate a response object, and place it on the response queue.

#### Response Added to Socket

![[../../../../../_images/Kafka/response-added-to-socket-send-buffer.png]]

+ From the response queue, the network thread will pick up the generated response, and send its data to the socket send buffer. The network thread also enforces ordering of requests from an individual client by waiting for all of the bytes for a response from that client to be sent before taking another object from the response queue.

### Fetch request

![[../../../../../_images/Kafka/fetch-requests.png]]

- In order to consume records, a consumer client sends a fetch request to the broker, specifying the topic, partition, and offset it wants to consume. The fetch request goes to the broker’s socket receive buffer where it is picked up by a network thread. The network thread puts the request in the request queue, as was done with the produce request.
- The I/O thread will take the offset that is included in the fetch request and compare it with the `.index` file that is part of the partition segment. That will tell it exactly the range of bytes that need to be read from the corresponding `.log` file to add to the response object.
- However, it would be inefficient to send a response with every record fetched, or even worse, when there are no records available. To be more efficient, consumers can be configured to wait for a minimum number of bytes of data, or to wait for a maximum amount of time before returning a response to a fetch request. While waiting for these criteria to be met, the fetch request is sent to purgatory.
- Once the size or time requirements have been met, the broker will take the fetch request out of purgatory and generate a response to be sent back to the client. The rest of the process is the same as the produce request.

## Data plane: Replication Protocol

- replication is enabled at the topic level
- Replication được khai báo trong lúc tạo partition. Ngoài ra có thể set giá trị mặc định cho replication factor -> không cần define lại mỗi lần tạo topic

![[../../../../../_images/Kafka/kafka-data-replication.png]]

### Leader, Follower, and In-Sync Replica (ISR) List

- Trong số N replicas, sẽ có 1 replica được gọi là leader -> broker chứa replica đó được coi là leader của partition. Các replicas còn lại được gọi là followers.
-  Producers will write to the leader replica and the followers will fetch the data in order to keep in sync with the leader. Consumers also, generally, fetch from the leader replica, but they can be configured to fetch from followers.

![[../../../../../_images/Kafka/leader-follower-isr-list.png]]

- The partition leader, along with all of the followers that have caught up with the leader, will be part of the in-sync replica set (ISR). In the ideal situation, all of the replicas will be part of the ISR.

### Leader Epoch

- Each leader is associated with a unique, monotonically increasing number called the leader epoch. The epoch is used to keep track of what work was done while this replica was the leader and it will be increased whenever a new leader is elected.

![[../../../../../_images/Kafka/leader-epoch.png]]

### Follower Fetch Request

![[../../../../../_images/Kafka/follower-fetch-request.png]]

- Whenever the leader appends new data into its local log, the followers will issue a fetch request to the leader, passing in the offset at which they need to begin fetching.

### Follower Fetch Response

![[../../../../../_images/Kafka/follower-fetch-response.png]]

- The leader will respond to the fetch request with the records starting at the specified offset. The fetch response will also include the offset for each record and the current leader epoch. The followers will then append those records to their own local logs.

### Committing Partition Offsets

![[../../../../../_images/Kafka/committing-partition-offsets.png]]

- Once all of the followers in the ISR have fetched up to a particular offset, the records up to that offset are considered committed and are available for consumers. This is designated by the high watermark.
- The leader is made aware of the highest offset fetched by the followers through the offset value sent in the fetch requests. For example, if a follower sends a fetch request to the leader that specifies offset 3, the leader knows that this follower has committed all records up to offset 3. Once all of the followers have reached offset 3, the leader will advance the high watermark accordingly.

### Advancing the Follower High Watermark

![[../../../../../_images/Kafka/advancing-the-follower-high-watermark.png]]

- The leader, in turn, uses the fetch response to inform followers of the current high watermark. Because this process is asynchronous, the followers’ high watermark will typically lag behind the actual high watermark held by the leader.

### Handling Leader Failure

![[../../../../../_images/Kafka/handling-leader-failure.png]]

- If a leader fails, or if for some other reason we need to choose a new leader, one of the brokers in the ISR will be chosen as the new leader. The process of leader election and notification of affected followers is handled by the control plane. The important thing for the data plane is that no data is lost in the process. That is why a new leader can only be selected from the ISR, unless the topic has been specifically configured to allow replicas that are not in sync to be selected. We know that all of the replicas in the ISR are up to date with the latest committed offset.
- Once a new leader is elected, the leader epoch will be incremented and the new leader will begin accepting produce requests.

### Temporary Decreased High Watermark

![[../../../../../_images/Kafka/temporary-decreased-high-watermark.png]]

- When a new leader is elected, its high watermark could be less than the actual high watermark. If this happens, any fetch requests for an offset that is between the current leader’s high watermark and the actual will trigger a retriable `OFFSET_NOT_AVAILABLE` error. The consumer will continue trying to fetch until the high watermark is updated, at which point processing will continue as normal.

### Partition Replica Reconciliation

![[../../../../../_images/Kafka/partition-replica-reconciliation.png]]

- Immediately after a new leader election, it is possible that some replicas may have uncommitted records that are out of sync with the new leader. This is why the leader's high watermark is not current yet. It can’t be until it knows the offset that each follower has caught up to. We can’t move forward until this is resolved. This is done through a process called replica reconciliation. The first step in reconciliation begins when the out-of-sync follower sends a fetch request. In our example, the request shows that the follower is fetching an offset that is higher than the high watermark for its current epoch.

### Fetch Response Informs Follower of Divergence

![[../../../../../_images/Kafka/fetch-response-informs-follower-of-divergence.png]]

- When the leader receives the fetch request it will check it against its own log and determine that the offset being requested is not valid for that epoch. It will then send a response to the follower telling it what offset that epoch should end at. The leader leaves it to the follower to perform the cleanup.

### Follower Truncates Log to Match Leader Log

- The follower will use the information in the fetch response to truncate the extraneous data so that it will be in sync with the leader.

![[../../../../../_images/Kafka/follower-truncates-log-to-match-leader-log.png]]

### Subsequent Fetch with Updated Offset and Epoch

![[../../../../../_images/Kafka/subsequent-fetch-with-updated-offset-and-epoch.png]]

- Now the follower can send that fetch request again, but this time with the correct offset.

### Follower 102 Reconciled

![[../../../../../_images/Kafka/follower-102-reconciled.png]]\

- The leader will then respond with the new records since that offset includes the new leader epoch.

### Follower 102 Acknowledges New Records

![[../../../../../_images/Kafka/follower-102-acknowledges-new-records.png]]

- When the follower fetches again, the offset that it passes will inform the leader that it has caught up and the leader will be able to increase the high watermark. At this point the leader and follower are fully reconciled, but we are still in an under replicated state because not all of the replicas are in the ISR. Depending on configuration, we can operate in this state, but it’s certainly not ideal.

### Follower 101 Rejoins the Cluster

- At some point, hopefully soon, the failed replica broker will come back online. It will then go through the same reconciliation process that we just described. Once it is done reconciling and is caught up with the new leader, it will be added back to the ISR and we will be back in our happy place.

![[../../../../../_images/Kafka/follower-101-rejoins-the-cluster.png]]

### Handling Failed or Slow Followers

![[../../../../../_images/Kafka/handling-failed-or-slow-followers.png]]

- Obviously when a leader fails, it’s a bigger deal, but we also need to handle follower failures as well as followers that are running slow. The leader monitors the progress of its followers. If a configurable amount of time elapses since a follower was last fully caught up, the leader will remove that follower from the in-sync replica set. This allows the leader to advance the high watermark so that consumers can continue consuming current data. If the follower comes back online or otherwise gets its act together and catches up to the leader, then it will be added back to the ISR.

### Partition Leader Balancing

- As we’ve seen, the broker containing the leader replica does a bit more work than the follower replicas. Because of this it’s best not to have a disproportionate number of leader replicas on a single broker. To prevent this Kafka has the concept of a preferred replica. When a topic is created, the first replica for each partition is designated as the preferred replica. Since Kafka is already making an effort to evenly distribute partitions across the available brokers, this will usually result in a good balance of leaders.

![[../../../../../_images/Kafka/partition-leader-balancing.png]]

- As leader elections occur for various reasons, the leaders might end up on non-preferred replicas and this could lead to an imbalance. So, Kafka will periodically check to see if there is an imbalance in leader replicas. It uses a configurable threshold to make this determination. If it does find an imbalance it will perform a leader rebalance to get the leaders back on their preferred replicas.

## Apache Kafka Control Plane

## Consumer Group Protocol

## Data Durability and Availability Guarantees



# Kafka ecosystem
1. Topic là core component của Kafka. Là đơn vị lưu trữu cơ bản trong Kafka. Nó tương đương với table hay collection trong một số DBMS khác -> Dùng name để định danh, chứa các message có chung mục đích. Confluent gọi là: Named container for similar events, hay là: Durable of ***logs*** events
2. Các records được gọi là message. Chúng phải nằm trong 1 topic nhất định. Message là immutable -> Không thể thay đổi. Các messages có retention time -> config dựa vào size hoặc thời gian.
3. Topic được break thành các partiotions -> mỗi partion là đơn vị lưu trữ mức vật lý của topic. Nó cũng giống như việc partitions trong các DBMS khác -> act as distributed system.
   **Message nào lưu và partitions nào ???**:
	-  Nếu message không xác định key -> **[[../../../../../Algorithm/Round Robin]]** (các message được lưu vào các partitions tuần tự theo đủ các partitions) -> Dùng trong trường hợp muốn đảm bảo về cân bằng số lượng các message trên các partitions.
	- Nếu message có chứa key ->key đó được dùng để xác định message được lưu vào partition nào bằng cách: 
				  Hash(key) mod n = x
		 với n là số lượng partition, x chính là output, tức là số thứ tự của partition mà message sẽ được lưu vào
	- Với việc xác định key của message -> đảm bảo các message có cùng key sẽ được lưu trong cùng 1 topic, và thứ tự của các message đó cũng giống với thứ tự theo thời gian.
4. Broker:
	- A computer, inst ance, or container running the kafka process -> independent
	- Manage partitions and replecation
	- Handle write and read request
5. Replication
	- Copies of data for fault tolerance
	- Main partitions -> leader (only 1), and N-1 followers
	- Khi đọc hoặc ghi -> thao tác với leader -> chuyển đến followers
6. Producer
	- Client application
	- Puts message into topics
	- Quyết định message sẽ được lưu ở partition nào.
7. Consumer 
	- Clent application
	- Read message sẽ không destroy message đó.
	- Sử dụng tham số groupID cho consumer để scale out.
	- Nếu số consumer > số partitions thì sẽ có 1 số partition không nhận được message

	***Làm thế nào để ứng dụng sau khi restart kafka vẫn nhận ra đó là service nào và apply offset cũ ???*** *Kafka không ghi nhớ consumer* => Do người dùng config để không bị mất message.
	- Cách config: sử dụng consumer group, nếu không set consumer group => dùng giá trị mặc định, lấy message từ offset cũ, được lưu bời Kafka
	***How Kafka distinct consumer & producer ???*** => Consumer group
	

### [[Kafka Connect]]
- Ecosystem of pluggable
- Client application -> not run on brokers
- Declerative bằng [[../../../../../Storage/JSON]] để config. Ví dụ:
  
```json
{"connector.class": "io.confluent.connect.elasticsearch.ElasticsearchSinkConnector",
    "topics"                          : "my_topic",
    "connection.url"                  : "http://elasticsearch:9200",
    "type.name"                       : "_doc",
    "key.ignore"                      : "true",
    "schema.ignore"                   : "true"
}
```

- Source connectors act as Producers
- Sink connectors act as Consumers
- Connectors viết bằng [[Java]]
- Connectors hỗ trợ CLI và [[REST]] [[API]]

#### Kafka connect deployment
#### Dead Letter Queues

### Schema registry
- Theo thời gian, schema của message có thể bị thay đổi -> Cần có 1 service để kiểm tra sự thay đổi đó có phù hợp hay không -> Schema registry giải quyết việc đó
- Server process external to kafka brokers
- Maintains a databse of schemas
- Consumer/Producer API component -> consumer hay producer có thể gọi API để xem schema của message có hợp lệ hay không.
- Schema registry hỗ trợ các format:
	- JSON
	- Avro
	- Protocol bufers
- Có thể áp dụng CI/CD hoặc automation để update schema registry rules

### Kafka streams
- Với sự gia tăng của các ứng dụng sử dụng Kafka -> Consumer ngày càng trở nên phức tạp và cần giải quyết các bài toán về transform data
- Functional [[Java]] [[API]]
- Hỗ trợ: Filtering, grouping, aggregating, join, ...
- Là consumer group
- Là 1 library, not infrastructure


### ksqlDB

- A database optimized for streaming processing
- Run outside of Kafka cluster
- Sử dụng syntax giống với [[SQL]]
- Hỗ trợ command line và [[REST]] [[API]]
- Có thể sử dụng như 1 [[Java]] Library
- Kết hợp với các connectors để automate luồng sử lý data

# Alternative

## 1. Kafka vs [[PubSub]]
## 2. Kafka vs Redpanda

# Combination

## 1. [[Apache Spark]]
## 2. [[Apache Flink]]
## 3. [[OLAP]] engine

## Reference
1. a

