# ☕ Machine Learning and AWS...

## AWS S3 Overview
Amazon S3 allows people to **store Objects** (any type of file format, such as CSV, JSON, Parquet, ORC, Avro, Protobuf) in **Buckets** (same as directories);
- Objects have a Key. The Key is the FULL path...
- Buckets have a _globally unique name_;
```
<my_bucket>/my_file.txt
```
```
<my_bucket>/my_filder/another_folder/my_file.txt
```
- This will be interesting when we look at _partitioning_ (related to Kinesis and Athena);
- Max object size is 5TB;
- Object Tags (key / value pair - up to 10) -- useful for security/lifecycle;
<br><br><br>
---
## AWS S3 for Machine Learning
- Backbone for many AWS ML Services (eg.: SageMaker);
- Create a "Data Lake" (hole purpose of data engineering is);
    - Infinite size, no provisioning
    - 99.999999999% durability - data secure and not loose objects with time
    - Decoupling of storage (S3) to compute (EC2, Amazon Athena, Amazon Redshift Spectrum, Amazon Rekognition, and AWS Glue) - the data in S3 can be use with all those compute tools
- Centralized Architecture - all data in one place
- Object Storage > means that supports any file format (CSV, JSON, Parquet, ORC, Avro, Protobuf)
<br><br><br>
---
## AWS S3 Data Partitioning
- Pattern for speeding up range queries (eg.: AWS Athena)
```
By date: s3://bucket/my_data_set/year/month/day/hour/data_00.csv

"/year/month/day/hour/" is the practition
```
```
By product: s3://bucket/my_data_set/product-id/data_01.csv
```
- It is possible to define whatever partitioning strategy
- Data partitioning will be handled by some tools we use (AWS Glue)