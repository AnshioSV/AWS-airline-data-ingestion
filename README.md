# AWS Flight Data Pipeline

This project demonstrates a fully serverless data pipeline built using AWS services to automate the ingestion and transformation of flight data. The architecture incorporates multiple AWS components to seamlessly process, enrich, and load data into Redshift for further analysis.

## Stack
- **Amazon S3**: For storing input CSV files.
- **Amazon Redshift**: As the data warehouse, with tables under the `airlines` schema.
  - **airports_dim**: A dimension table with constant data about airports.
  - **daily_flights_fact**: A fact table for daily flight data (initially empty).
- **Amazon EventBridge**: To trigger the pipeline when a new CSV file is uploaded.
- **AWS Glue**: For data cataloging and ETL.
- **AWS Step Functions**: To orchestrate the workflow.
- **IAM**: For managing access and permissions.

## Workflow
1. **S3 Bucket Setup**:
   - An S3 bucket is configured to store CSV files under a specific folder.

2. **Redshift Schema and Tables**:
   - Two tables are created in Redshift under the `airlines` schema:
     - **airports_dim**: A constant table with airport details.
     - **daily_flights_fact**: An initially empty table for storing enriched flight data.

3. **EventBridge Rule**:
   - An EventBridge rule monitors the S3 bucket for new `.csv` files in the specified folder.
   - When a file is added, the rule triggers a Step Function execution.

4. **Step Functions Orchestration**:
   - The workflow starts a Glue Crawler to catalog the newly added CSV file.
   - Once the Crawler completes, the workflow triggers a Glue ETL job (`flight data ingestion`).

5. **Glue ETL Job**:
   - The ETL job reads the CSV data from S3.
   - It performs a join operation with the `airports_dim` table in Redshift using the `airport_id` field to enrich the data with airport names and state details.
   - The enriched data is ingested into the `daily_flights_fact` table in Redshift.

## Key Features
- Fully serverless pipeline leveraging AWS managed services.
- Automated detection and processing of new files in S3.
- Data enrichment using Redshift and Glue.
- Scalable and efficient workflow orchestration with Step Functions.

## How to Use
1. Configure an S3 bucket with the folder structure for CSV uploads.
2. Create the `airlines` schema and the required tables in Redshift.
3. Set up an EventBridge rule to monitor the S3 bucket.
4. Deploy the Step Function and Glue resources.
5. Upload a CSV file to the specified S3 folder and watch the pipeline in action!

---
This pipeline can be extended or modified to support additional data sources, transformations, or target systems. Contributions and feedback are welcome!

