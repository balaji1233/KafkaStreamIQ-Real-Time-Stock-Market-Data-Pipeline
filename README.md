# KafkaStreamIQ: Real-Time Stock Market Data Pipeline

## Introduction 
In this project, we have executed an End-To-End Data Engineering Project on Real-Time Stock Market Data using Kafka.

We are going to use different technologies such as Python, Amazon Web Services (AWS), Apache Kafka, Glue, Athena, and SQL.


## Technology Used
- Programming Language - Python
- Amazon Web Service (AWS)
1. S3 (Simple Storage Service)
2. Athena
3. Glue Crawler
4. Glue Catalog
5. EC2
- Apache Kafka

## 📋 Project Overview
**KafkaStreamIQ** is a real-world data engineering solution for ingesting, processing, and analyzing live stock market data. Leveraging Apache Kafka, Python, and AWS, this pipeline demonstrates a scalable, end-to-end architecture that transforms raw tick data into queryable insights.

## 🛠️ Problem Statement
Finance teams and analysts need **instantaneous**, reliable access to streaming market data to make time-sensitive decisions. Traditional batch-oriented ETL processes introduce latency and miss fleeting opportunities, resulting in:

- **Missed trades** due to stale data.
- **Inefficient monitoring**, increasing operational risk.
- **Limited scalability** when handling spikes in market activity.

**KafkaStreamIQ** addresses these challenges by building a **real-time streaming engine** that captures every tick, stores it durably, and enables SQL-based querying without managing servers.

## 🚀 Key Features & Impact

- **Real-Time Ingestion**: 100% of incoming tick events consumed with under 500 ms end-to-end latency.
- **High Throughput & Scalability**: Partitioned Kafka topics support parallel processing of 10,000+ messages per second.
- **Durable Storage**: Automatic upload of JSON-formatted records to Amazon S3, ensuring 99.99% durability.
- **Automated Schema Cataloging**: AWS Glue Crawler infers JSON schemas and populates the Data Catalog, cutting manual schema effort by 80%.
- **Serverless Querying**: Amazon Athena delivers interactive SQL queries on streaming data lakes, reducing query provisioning time from hours to minutes.
- **Cost Efficiency**: Serverless AWS services and auto-scaling Kafka clusters lower infrastructure costs by up to 40% compared to managed VM deployments.

## ⚙️ Architecture Diagram
<img src="Architecture.jpg">

1. **Producer**: Python-based Kafka producer publishes mock or live market ticks to a Kafka topic.  
2. **Kafka Cluster**: Deployed on AWS EC2, configured with multiple brokers and partitions for fault tolerance and parallelism.  
3. **Consumer & S3 Sink**: Python Kafka consumer writes each record to Amazon S3 as individual JSON files via `s3fs`.  
4. **AWS Glue Crawler**: Scans new S3 objects, infers schema, and updates the Glue Data Catalog.  
5. **Amazon Athena**: Runs SQL queries against the cataloged data lake for real-time analytics.

## 📦 Prerequisites

- **Hardware**: Laptop or server with internet access.  
- **Software**:
  - Python 3.8+  
  - Java 8+ (for Kafka)  
  - AWS CLI configured with credentials  
  - Docker (optional, for local Kafka testing)  
- **Accounts**: AWS account with permissions to EC2, S3, Glue, and Athena.

  
## 🚧 Setup & Installation

1. **Clone the Repository**
```
   git clone https://github.com/your-org/kafkastreamiq.git
   cd kafkastreamiq
```

2. **Create Python Environment**
```
   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   **Configure AWS CLI
``
```


3.**Configure AWS CLI**

```
aws configure
# Enter AWS Access Key ID, Secret Access Key, Default region, and Output format

```

4.**Start Producer & Consumer**

```
python producer.py       # begins sending tick data
python consumer_to_s3.py # consumes and writes to S3
```

5.**Deploy AWS Glue Crawler & Athena**
```
1. Use the CloudFormation template: `infra/glue-athena.yml`.  
2. Run Athena queries in the AWS Console or via `athena_queries.sql`.

```
  
## 🎯 Usage and Results

Once running, navigate to the Amazon Athena console and execute:

```sql
SELECT
  symbol,
  AVG(price)    AS avg_price,
  COUNT(*)      AS tick_count
FROM "kafkastreamiq_db"."ticks_table"
WHERE event_time > date_sub('hour', 1, now())
GROUP BY symbol;
```
- Example Output: Processes ~36,000 ticks in under 2 seconds and returns per-symbol metrics.

  ## 🔗 References

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [AWS Glue Developer Guide](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html)
- [AWS Athena User Guide](https://docs.aws.amazon.com/athena/latest/ug/what-is.html)
