# ☕ Machine Learning and AWS...

Hi! Welcome to my Machine Learning on AWS study notes!! 👋 I strongly hope this document is as useful to you as it is to me. Have fun!!  🎉

<br>

# 1. Data Engineering

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

### 📌 Kinesis Analytics, Conceptually...

Amazon Kinesis Data Streams + Amazon Kinesis Dara Firehose --> Amazon Kinesis Data Analytics --> Analytics Tools, Outputs


### 📌 Kinesis Analytics, In more depth...
Input Stream(s) (Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose) + Reference Table (Amazon S3) --> Amazon Kinesis Data Analytics ("SELECT STREAM (ItemID, count(*) FROM SourceStream GROUP BY ItemID)") --> Output Streams (Amazon Kinesis Data Streams + Amazon Kinesis Data Firehose --> Amazon S3 + Amazon Redshift) or Error Stream (query goes wrong)

### 📌 Kinesis Data Analytics..
- Use cases
    - Streaming ETL: select columns, make simple transformations, on streaming data (ex.: reduce size of data set by selecting columns, making that with simple transformations in streaming data)
    - Continuous metric generation: live leaderboard for a mobile game
    - Responsive analytics: look for certain criteria and build alerting (filtering)
- Features
    - Pay only for resources consumed (but its not cheap)
    - Serverless: scales automatically
    - Use IAM permissions to access streaming source and destinations
    - SQL for Flink to write the computation
    - Schema discovery
    - Lambda can be used for pre-processing
    
### 📌 Machine Learning on Kinesis Data Analytics..
- Algorithms to remember...
    - RANDOM_CUT_FOREST
        - SQL function used for anomaly detection on numeric columns in a stream
        - Example: detect anomalous subway ridership during the NYC marathon
        - Uses recent history to compute model 
    - HOTSPOTS
        - locate and return information about relatively dense regions in your data
        - Example: a collection of overheated servers in a data
        
### 📌 Kinesis Video Stream
- Producers
    - Security camera, body-worn camera, AWS DeepLens, smartphone camera, audio feeds, images, RADAR data, RTSP camera.
    Producer SDK + DeepLens ---> Kinesis Video Stream
    - One producesr per video stream
- Video playback capability
- Consumers
    - build your own (MXNet, Tensorflow)
    - AWS SageMaker
    - Amazon Rekognition Video
- Keep data for 1 hour to 10 years - data retention


### 📌 Kinesis Video Stream Use Cases
Kinesis Video Stream -- 1. Consume video stream in real-time -->
Fargate (Docker) -- 2. Checkpoint Stream Processing status (Consumer) --> 3. Send decoded frames for ML-based inference
<br> --> Fargate (Docker) -- 4. Publish inference results --> Kinesis Data Streams -- 5. Notifications --> AWS Lambda


### 📌 Kinesis Summary -- Machine Learning
- Kinesis Data Stream: create real-time machine learning applications
- Kinesis Data Firehose: ingest massive data near-real time
- Kinesis Data Analytics: real-time ETL/ML algorithms on streams
- Kinesis Video Stream: real-time video stream to create ML applications

<br>

## 2. Glue Data Catalog & Crawlers
- Metadata repository for all your tables
    - Automated Schema Inference
    - Schemas are versioned
    Amazon S3 -- index --> AWS Glue Data Catalog
- Integrate with Athena or Redshift Spectrum (schema & data discovery)
    Amazon S3 -- index --> AWS Glue Data Catalog -- use schema --> AmazonRedshift/Amazon Athena --> Amazon QuicjSight/Amazon EMR
- The Glue Crawlers can help build the Glue Data Catalog

### 📌 Glue Data Catalog - Crawlers
- Creawlers go through your data to infer schemas and partitions 
- Works JSON, Parquet, CSV, relational store
- Crawlers work for: S3, Amazon Redshift, Amazon RDS
- Run the Crawler ona Schedule or On Demand
- Need an IAM role/credentials to access the data stores

### 📌 Glue and S3 Partitions
- Glue crawler will extract partitions based on how your S3 data is organized
- Think up front about how you will be querying your data lake in S3
- Exemple: devices send sensor data every hour
    - Do you query primarily by time ranges?
        - If so, organize your buckets as s3://my-bucket/dataset/yyyy/mm/dd/device
    - Do you query primarily by device?
        - If so, organize your buckets as s3://my-bucket/dataset/device/yyyy/mm/dd

## 3. Glue ETL
- Transform Data, Clean Data, Enrich Data (before doing analysis)
    - Generate ETL code in Python or Scala, you can modify the code
    - Can provide your own Spark or PySpark scripts
    - Target can be S3, JDBC (RDS, Redshift), or in Glue Data Catalog
- Fully managed, cost effective, pay only for the resources consumed
- Jobs are run on a serverless Spark platform 
- Glue Scheduler to schedule the jobs
- Glue Triggers to automate job runs based on "Events"

### 📌 Glue ETL - Transformations
- Bundled Transformations
    - DropFields, DropNullFields - remove (null) fields
    - Filter - specify a function to filter records
    - Join - to enrich data
    - Map - add fields, delete fields, perform external lookups
- Machine Learning Transformations
    - FindMatches ML: identify duplicate or matching records in your dataset, even when the records do not have a common unique identifier and no fields match exactly
- Format conversions: CSV, JSON, Avro, Parquet, ORC, XML 
- Apache Spark transformations (example: K-Means)

<br>

## 4. AWS Data Stores for Machine Learning
- Redshift
    - Data Warehousing, SQL Analytics (OLAP - Online Analytical Processing) 
    - Load data from S3 to Redshift
    - Use Redshift Spectrum to query data directly in S3 (no loading)
- RDS, Aurora
    - Relational Store, SQL (OLTP, Online Transaction Processing)
    - Must provision servers in advance
- Dynamo DB 
    - NoSQL data store, serverless, provision read/write capacity
    - Useful to store a machine learning model served by your application
- S3
    - Object storage
    - Serverless, infinite storage
    - Easy Integration 
- ElasicSearch
    - Indexing of data
    - Search amongst data ponits
    - Clickstram analytics
- ElastiCache
    - Caching mechanism
    - Not really used for Machine Learning
    
## 5. AWS Data Pipelines
- Features
    - Service to move data to another place, it is an ETL service
    - Destinations include S3, RDS, DynamoDB, Redshift and EMR
    - Manages task dependencies (it has a S3 inside it to process)
    - Retries and notifies on failures
    - Data sources may be on-premises
    - Highly available
- Data Pipeline example
    - Considering that we have data inside a RDS/DynamoDB, and we want to move it to a S3 Bucket to use this data whith SageMaker; we can use the AWS Data Pipeline to generate EC2 Instances (managed by Data Pipeline)
- AWS Data Pipeline vs Glue
    - Glue
        - Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
        - Glue ETL - Do not worry about configuring or managing the resources
        - Data Catalog to make the data available to Athena or Redshift Spectrum
    - Data Pipeline
        - Orchestration service
        - More control over the environment, compite resources that run code, & code 
        - Allows access to EC2 or EMR instances (creates resources in your own account)

## 6. AWS Batch
- Run batch jobs as Docker images
- Dynamic provisioning of the instances (EC2 & Spot Instances)
- Optimal quantity and type based on volume and requirements
- No need to manage clusters, fully serverless
- Just pay for the underlying EC2 instances
- Schedule Batch Jobs using CloudWatch Events
- Orchestrate Batch Jobs using AWS Step Functions

<br>

- AWS Batch vs Glue
    - Glue
        - Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
        - Glue ETL - Do not worry about configuring or managing the resources
        - Data Catalog to make the data available to Athena or Redshift Spectrum
    - Batch
        - For any computing job regardless of the job (must provide Docker image)
        - Resources are created in your account, managed by Batch
        - For any non-ETL related work, Batch is probably better
        
## 6. AWS DMS - Data Migration Service
- Quickly and securely migrate databases to AWS, resilient, self healing
- The source database remains available during the migration
- Supports
    - Homogeneous migrations: ex Oracle to Oracle
    - Heterogeneous migrations: ex Microsoft SQL Server to Aurora
- Continous Data Replication using CDC 
- You must create an EC2 instance to perform the replication tasks

- AWS DMS vs Glue 
    - Glue
        - Glue ETL - Run Apache Spark code, Scala or Python based, focus on the ETL
        - Glue ETL - Do not worry about configuring or managing the resources
        - Data Catalog to make the data available to Athena or Redshift Spectrum
    - AWS DMS
        - Continuous Data Replication
        - No data transformation
        - Once the data is in AWS, you can use Glue to transform it

## 7. AWS Step Functions
- Use to design workflows
- Easy visualizations
- Advanced Error Handling and Retry mechanism outside the code 
- Audit of the history of workflows
- Ability do "Wait" for an arbitrary amount of time
- Max execution time of a State Machine is 1 year

<br>

# 2. Exploratory Data Analysis

## 1. Features engineering 

- All about features engineering in exploratory data analysis
- Python is becoming ubiquitous. Libraries...
    - PANDA
        - For slicing and dicing data
        - Data frames series (table of the data, like Excel), Series (unidimentional role for the data frame), interoperates with Numpy (from tables to array)
        - Examples... 
            1. "df[['Years of Experience', 'Hired']][:5]" --> Extrct 5 lines of tose two columns of the data frame.
            2. degree_counts = df = ['Level of Education'] value_counts() --> Counts the number of values
        - Basically Panda extract some parts of the data
    - MATPLOTLIB
        - Used for data visualization, deal with data distribution and outliners
    - SEABORM 
        - Like Matplotlib but more flexible, "hot" maps
    - PEARPLOTS
        - Some plots for data visualization, seaborn
    - SCIKIT_LEARN
        - Python library for Machine Learning models --> Decision three - cascate of decision points 
``` 
from sklearn.ensemble import RandomForestClassifier

clf = RandomForestClassifier(n_estimators=10)
clf = clf.fit(X, y)
# X = attributes, y = labels

# Predict employment of an employed 10-year veteran
print (clf.predict([[10, 1, 4, 0, 0, 0]]))
# ...and an unemployed 10-year veteran
print (clf.predict([[10, 1, 4, 0, 0, 0]]))

# Result
[1] (Hired)
[0] (Not hired)
``` 

## 2. Types of Data 

- Many Flavors of Data - different kinds of data. Tge type of data you're dealing with will dictate what kind of algorithm you might use to create a machine learning model from it.
- Major Types of Data
    - Numerical
    - Logical
    - Ordinal

### 2.1 Numerical   
- Represents some sort of quantitative measurement
    - Height of people, page load times, stock prices, etc
- Discrete Data
    - Integer based; often counts of some event
        - How many purchases did a customer make in a year?
        - How many times did I flip "heads"?
- Continuous Data
    - Has an infinite number of possible values, like fractions
        - How much time did it take for a user to check out?
        - How much rain fell on a given day?

### 2.2 Logical   
- Qualitative data that has no inherent mathematical meaning
    - Gender, Yes/No (binary), Race, State of Residence, Product Category, Political Party, etc.
- You can assign numbers to categories to represent them more compactaly, but the numbers don't have mathematical meaning

### 2.3 Ordinal   
- A mixture of numerical and categorical
- Categorical data that has mathematical meaning
- Example: movie ratings on a 1-5 scale
    - Ratings must be 1, 2, 3, 4, or 5
    - But these values have mathematical meaning; 1 means its a worse movie than a 2

## 2. Data Distributions

- A "Normal Distribution"
    - Example of a probability density function
    - "Gives you the probability of a data point falling within some given range of a given value"
- Probability Mass Function
    - When you deal with discrete data, it is a way to visualize the probability of discrete data occourring
    - Looks like a histogram because it is basically a histogram

    Looking deep into de differences...
    - The probability density function is a solid curve that describes the probability of a range of values happening with continuous data; probability mass function is the probabilities of given discrete values occurring in a dataset 
- Poisson Distribution
    - Example of probability mass function is the poisson distribution, discrete distribution
- Binomial Distribution
    - Describes the number of successes in a sequence of experiments with a yes or no question
    - Ex.: flip a coin
- Bernoulli Distribution
    - Special case of binomial distribution
    - Has a single trial (n=1)
    - Can think of a binomial distribution as the sum of bernoulli distributions

## 3. Time Series: Trends and Seasonality
