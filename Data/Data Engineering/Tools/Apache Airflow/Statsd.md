---
---
# Airflow Statsd

> Là thành phần của [[Apache Airflow]], có nhiệm vụ đo đạc các runtime metrics của [[Apache Airflow]] và gửi output ra log hay 1 [[websocket]]

- Phần code của statsd ở file: airflow/stats.py

# Architecture

1. TimeProtocol và StatsLogger classes là [[Protocol Class]], dành cho type cheking
3. Timer class -> Class dùng để đo thời gian
4. SummyStatsLogger class
5. 
