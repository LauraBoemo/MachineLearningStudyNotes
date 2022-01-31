# ☕ Machine Learning and AWS...

Hi! Welcome to my Machine Learning on AWS study notes! 👋 I strongly hope this document is as useful to you as it is to me. Have fun! 🎉

<br>

## 1. Where it all starts... Amazon S3

### 📌 AWS S3 Overview
Amazon S3 allows people to **store Objects** (any type of file format, such as CSV, JSON, Parquet, ORC, Avro, Protobuf, etc; as long as it contains a maximum size of 5TB) in **Buckets** (same as directories);

✏️ To Remember...
- Every object has a Key. The Key is the FULL path...
    - This full path will be interesting when we look at **partitioning** (related to Kinesis and Athena);
    - Those tags can be associated with the Object Tag; which are helpful to classify data, security and lifecycle;
- Buckets have a _globally unique name_;

🌮 Examples...
```
<my_bucket>/my_file.txt
```
```
<my_bucket>/my_filder/another_folder/my_file.txt
```
<br>

### 📌 AWS S3 for Machine Learning
S3 is the backbone for many AWS ML Services (eg.: SageMaker). It is helpful to create a **Data Lake**, a few factors for this are...

- S3 has infinite size with no provisioning (which means it is possible to create a centralized architecture - maintaining all the data in one place).
    - By the way, it does not only about maintaining but also about safety: S3 has 99.999999999% of durability.
- Decoupling of storage (S3) to compute (EC2, Amazon Athena, Amazon Redshift Spectrum, Amazon Rekognition, and AWS Glue) - the data in S3 can be used with all those compute tools, **which create a massive Data Lake in S3**.

<br>

### AWS S3 Data Partitioning
AWS S3 allows us to create a pattern for speeding up range queries (eg.: AWS Athena), called **partitioning** - there are infinite ways to define whatever partitioning strategy; which will be handled by some AWS tools, like AWS Glue.

🌮 Examples...

```
By date: 

s3://bucket/my_data_set/year/month/day/hour/data_00.csv

"/year/month/day/hour/" is the practition
```
```
By product: 

s3://bucket/my_data_set/product-id/data_01.csv

"/product-id/" is the practition
```
<br>

### 📌 AWS S3 Storage Tiers & Lifecycle Rules

📝 There are different storage tiers on S3...
- Amazon S3 Standart - General Purpose and default **<< Important for AWS ML**
```
    - Durability -- 99.999999999%
    - Availability -- 99.99%
    - AZ -- >= 3
    - Concurrent facility fault tolerance -- 2
```
- Amazon S3 Standart - Infrequent Access (IA)
- Amazon S3 One Zone - Infrequent Access
- Amazon S3 Intelligent Tiering
- Amazon Glacier **<< Important for AWS ML**
```
    - Durability -- 99.999999999%
    - Availability -- NA
    - AZ -- >= 3
    - Concurrent facility fault tolerance -- 1
```

Is possible set lifecycle rules in S3, it moves the data between different tiers, to save storage cost.

🌮 Examples...

```
General Purpose > Infrequent Access > Glacier
```
📝 2 ways to set lifecycle rules...
1. Transition Actions
    - Objects are transitioned to another storage class.
        - "Move objects to Standart IA class 60 days after creation and then move to Glacier for archiving after 6 months"
2. Expiration Actions
    - S3 deletes expired objects on our behalf
        - Access log files can be set to delete after a specified period of time

<br>

### 📌 AWS S3 Encryption for Objects 

📝 There are 4 methods of encrypting objects in S3...
- SSE-S3 - Encrypts S3 objects using keys handled & managed by AWS **<< Important for the AWS ML**
- SSE-KMS - use AWS Key Management Service to manage encryption keys **<< Important for the AWS ML**
    - Additional security (user must have access to KMS key)
    - Audit trail for KMS usage
- SSE-C - when you want to manage your own encryption keys
- Client Side Encryption

But, from a ML perspective, SSE-S3 and SSE-KMS will be most likely used. So...

- SSE-S3
    - Object -- HTTP/S + Header -- | *Amazon S3 --> Object + S3 Managed Data Key -- Encryption --> Bucket | 
    
- SSE-KMS
    - Object -- HTTP/S + Header -- | *Amazon S3 --> Object + KMS Customer Master Key (CMK) -- Encryption --> Bucket | 
    
<br>

### 📌 AWS S3 Security Pt.1

📝 Some types of S3 Security...

1. User-based. 
    - In this case, we can create IAM Policies (with identity access, which API calls should be allowed for a specific user); **<< Important for the AWS ML**
2. Resource Based 
    - Bucket Policies - bucket wide rules from the S3 console - allow cross account; **<< Important for the AWS ML**
    - Object Access Control List (ACL) - finer grain; 
    - Bucket Access Control List (ACL) - less common;

But, from a ML perspective, User-based and Bucket Policies will be most likely used. So...

- S3 Bucket Policies
    - JSON Based policies
        - Resources: buckets and objects;
        - Actions: set of API to Allow or Deny;
        - Effect: allow / deny;
        - Principal: the account or usr to apply the policy to
    - Use S3 bucket for policy to
        - Grant public access to the bucket;
        - Force objects to be encrypted at upload;
        - Grant access to another account (Cross Account)

<br>

### 📌 S3 Default Encryption VS Bucket Policies
The old way to enable Default Encryption was to use a bucket policy and refuse any HTTP command without the proper header, the new way is to use the "default encryption" option in S3 (JSON). Note: Bucket Policies are evaluated before "default encryption".

<br>

### 📌 AWS S3 Security Pt.2
📝 Other tools for security in S3...
- Networking - VPC Endpoint Gateway:
    - Allow traffic to stay within my VPC (instead of going through public web);
    - Make sure your privte services (AWS SageMaker) can access S3
    - **Very important for AWS ML**
- Logging and Audit:
    - S3 access logs can be stored in other S3 bucket;
    - API calls can be logged in AWS CloudTrail (consult who did and why);
- Tagged Based (IAM Policies + Bucket Policies)
    - Example: Add tag Classification=PHI to your objects;

<br>


## 2. Kinesis Data Stream & Kinesis Data Firehose 

### 📌 AWS Kinesis Overview
#### ⭐ Extremely Important Subject for AWS ML ⭐<br>
Kinesis is a managed alternative to Apache Kafka. It is great for applications logs, metrics, IoT, and clickstreams; also great for "real-time" big data and streaming processing frameworks (Spark, NiFi, etc...). In Kinesis, all the data is automatically replicated synchronously to 3 AZ.

📝 Kinesis products...
- Kinesis Streams: low latency straming ingest at scale;
- Kinesis Analytics: perform real-time analytics on streams using SQL;
- Kinesis Firehose: load strams into S3, Redshift, ElasticSearch & Splunk;
- Kinesis Video Streams: meant for streaming video in real-time;

From a archtecture point of view Kinesis looks like...
1. Click streams, IoT devices, Metrics & Logs will be onboard by "Amazon Kinesis Streams";
2. Then we perfom some real time analytics with them, using "Amazon Kinesis Analytics";
3. Then, finally, we move those analytics to "Amazon Kinesis Firehose" that will put all data in Amazon S3 bucket or Amazon Redshift;

```
Obs.: "Amazon Kinesis Streams", "Amazon Kinesis Analytics" and "Amazon Kinesis Firehose" are "Amazon Kinesis"
```

### 📌 Kinesis Data Streams 
📝 Overview...
- Streams are divided in ordered Shards/Partitions.
- Shards have to be provisioned in advance (capacity panning).
- More Shards = More capacity and speed.
    <br>-- Producers --> | Shard 1 / Shard 2 / Shard 3 | -- Consumers -->
- Data retention is 24 hours by default, can go up to 365 days.
- Ability to reprocess/replay data.
- Multiple applications can consume the same stream.
- Once data is inserted in Kinesis, it can't be deleted (immutability).
- Records can be up to 1MB in size.

📝 Limits to know...
- Producer
    - 1MB/s or 1000 messages/s at write PER SHARD
    - "ProvisionedThroughputException" otherwise
- Consumer Classic
    - 2 MB/s at read PER SHARD across all consumers
    - 5 API calls per second PER SHARD across all consumers
- Data Retention
    - 24 hours data retention by default
    - Can be extended to 365 days

Kinesis Stream is great fo real time streaming applications. It is something that you will need provision in advance.

### 📌 Kinesis Data Firehose
Kinesis Data Firehose is to store data in different places.
<br><br>We have producers (Applications, Client, SDK, KPL, Kinesis Agent) <br>--> The data they produce will be read by Kinesis Data Streams (most common), or Amazon CloudWatch (Logs & Events), or AWS IoT <br>--> And then finally processed bt Kinesis Data Firehose, recorded one by one, up to 1MB each one. And if you want to transform de record, we have Lambda Function for Data Transformation in this step <br>--> After that is created a Batch Writes of all the information and move it to the target destination <br>--> The destinations can be...
- 3rd-party Partner Destinations
    - Datadog
    - Splunk
    - New Relic
    - MongoDB
- AWS Destinations
    - Amazon S3
    - Amazon Redshift (COPY through S3)
    - Amazon ElasticSearch
- Custom Destinations
    - HTTP Endpoint

📝 Overview...
- Fully Managed Service, no administration
- Near Real Time (60 seconds latency minimum for non full batches)
- Data Integration into Redshift / Amazon S3 / ElasticSearch / Splunk
- Automatic Scaling
- Supports many data formats
- Data Conversions from CSV / JSON to Parquet / ORC (only for S3)
- Data transformation through AWS Lambda (ex.: CSV => JSON)
- Supports compression when target is Amazon S3 (GZIP, ZIP, and SNAPPY)
- Pay for the amount of data going through Firehose 

### 📌 Kinesis Data Streams vs Firehose
- Streams
    - Going to write custom code (producer / consumer) - real time applications
    - Real time (~200ms latency for classic, ~70ms latency for enhaced fan-out)
    - Must manage scaling (shard splitting (more shard) / merging (less shard))
    - Data Storage for 1 to 365 days, replay capability and multi consumers
- Firehose
    - Delivery service, **ingestion** service.
    - Fully managed, send to S3, Splunk, Redshift, ElasticSearch
    - Serverless data transformations with Lambda
    - **Near** real time (lowest buffer time is 1 minute)
    - Automated Scaling
    - No data storage
