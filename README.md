### Introduction

With this project, we hope to assist Sparkify, a music streaming startup, in moving its user base and song database operations to the cloud. In particular, I construct an ETL pipeline that pulls their data from **AWS S3** (data storage), stages tables on **AWS Redshift** (data warehouse with *columnar storage*), and runs **SQL** statements to create the analytics tables from these staging tables.

### Datasets

Two open **S3 buckets** are given for the datasets utilised in this project. Information about songs and artists is in one bucket, while user activity is covered in the second bucket (which song are listening, etc.. ). JSON files make up the objects in both buckets.

Data will be ingested and modified using the Redshift service; in fact, we will read the JSON files inside the buckets and copy their content to our staging tables using the 'COPY' command.

### Database Schema

Two staging tables are used to "copy" the JSON file into the S3 buckets.

#### Staging Table

+ **staging_songs** - info about songs and artists
+ **staging_events** - actions done by users (which song are listening, etc.. )

created a star schema that is well-suited for song play analysis queries. This comprises the tables below.

#### Fact Table

+ Records with the page 'NextSong' are known as "songplays," which are records in event data related to song plays.

#### Dimension Tables

+ **users** - users in the app
+ **songs** - songs in music database
+ **artists** - artists in music database
+ **time** - timestamps of records in **songplays** broken down into specific units

### Data Warehouse Configurations and Setup

- In your AWS account, create a new "IAM user," grant it administrator access, and attach policies.
- To create clients for 'EC2', 'S3', 'IAM', and 'Redshift', use the access key and secret key.
- Make an "IAM Role" that allows Redshift access to an "S3 bucket" (ReadOnly).
- Create a "RedShift Cluster" and fill out the configuration file with the "DWH_ENDPOIN(Host address)" and "DWH_ROLE_ARN."

### ETL Pipeline

- Made tables to house the information from "S3 buckets."
- Transferring data from staging tables in the Redshift Cluster to "S3 buckets."
- data was added from the staging tables into the fact and dimension tables. 

### Project Structure

+ `create_tables.py` - This script will drop old tables (if exist) ad re-create new tables.
+ `etl.py` - This script executes the queries that extract `JSON` data from the `S3 bucket` and ingest them to `Redshift`.
+ `sql_queries.py` - This file contains variables with SQL statement in String formats, partitioned by `CREATE`, `DROP`, `COPY` and `INSERT` statement.
+ `dhw.cfg` - Configuration file used that contains info about `Redshift`, `IAM` and `S3`

### To Run

- Run "create_tables.py" to create tables.

- Run "etl.py" to start the ETL process.
