---

tags:
  - SerDes
---
# Why ?


# Topology

## Serialization
- Serialization bao gồm cả deserializing và serializing
- Cần có Serialization bởi vì: Kafka broker chỉ làm việc với chuỗi [[../../../../../Hardware/bytes]]
- Kafka lưu trữ các giá trị theo bytes. Khi 1 consumer lấy dữ liệu, [[Apache Kafka|Kafka]] sẽ gửi về chuỗi bytes đó. Để consumer hiểu được nội dung của message -> cần có Deserializer để decode thành object
- Tương tự, khi gửi dữ liệu lên [[Apache Kafka|Kafka]] thì cần có Serializer để encode bject.

### Deserializer

![[../../../../../_images/Kafka/deserializer.png]]
Deserializer nhận vào 1 chuỗi các [[../../../../../Hardware/bit]] và -> là 1 object 

### Serializer

![[../../../../../_images/Kafka/serializer.png]]
Serializer ngược với Dersializer: nhận 1 object vào -> 1 chuỗi [[../../../../../Hardware/bit]]

# Kafka Stream SerDes
- To bring data into Kafka Streams, you provide SerDes for your topic’s key and value in the `Consumed` configuration object.
  
``` java
StreamsBuilder builder = new StreamsBuilder()
KStream<String, MyObject> stream = builder.stream("topic",
Consumed.with(Serdes.String(), customObjectSerde)
```

## Custom SerDes
- Tạo custom SerDes
  
``` java
Serde<T> serde = Serdes.serdeFrom( new CustomSerializer<T>, new CustomDeserializer<T>); 
```

- Để tạo custom SerDes thì cần tạo Custom Serializer và Deserializer trong [[Apache Kafka|Kafka]] client library
- Ngoài ra Kafka cũng có sẵn 1 số SerDes như: `SpecificAvroSerde`, `GenericAvroSerde`, JSON, Protobuf, ...