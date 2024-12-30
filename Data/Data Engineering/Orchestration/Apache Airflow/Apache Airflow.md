# Apache Airflow


## 1. Why
	1. Purposes

## 2. Architecture

# Configuration
- Webserver secret key -> use to used to run your flask app. All server must using same secret_key, to create [[CSRF]] token. The webserver key is also used to authorize requests to Celery workers when logs are retrieved
- fernet_key -> store password of **airflow connections** in database




## DAG File Processing

Quy trình xử lý các file DAG của Airflow như sau:

![[airflow_dag_file_processor.png]]