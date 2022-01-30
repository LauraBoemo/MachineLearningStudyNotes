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
