# ☕ Machine Learning and AWS...

## Where it all starts... Amazon S3

<br>

### AWS S3 Overview
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

### AWS S3 for Machine Learning
S3 is the backbone for many AWS ML Services (eg.: SageMaker). It is helpful to create a **Data Lake**, a few factors for this are...

- S3 has infinite size with no provisioning (which means it is possible to create a centralized architecture - maintaining all the data in one place).
    - By the way, it does not only about maintaining but also about safety: S3 has 99.999999999% of durability.
- Decoupling of storage (S3) to compute (EC2, Amazon Athena, Amazon Redshift Spectrum, Amazon Rekognition, and AWS Glue) - the data in S3 can be used with all those compute tools, **which create a massive Data Lake in S3**.

<br>

### AWS S3 Data Partitioning
AWS S3 allow us to create a pattern for speeding up range queries (eg.: AWS Athena), called **partitioning** - there are infinites ways define whatever partitioning strategy; which will be handled by some AWS tools, like AWS Glue 
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