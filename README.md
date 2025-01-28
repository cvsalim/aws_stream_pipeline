Streaming Data Pipeline on AWS

This project sets up a streaming data pipeline for processing music listening data. The pipeline uses AWS services such as Kinesis, Glue, and S3 to handle data ingestion, transformation, and storage.

Architecture
Data Source: The input consists of a static dataset (songs.csv) and streaming user activity (streams.csv).
Data Ingestion: Kinesis is used to stream user activity data (streams.csv).
Processing: Apache Spark (via AWS Glue) processes the streamed and static data for analytics.
Output: Aggregated results are stored in S3 as Parquet files for downstream analytics.

Steps

1. Upload Static Data to S3
Upload songs.csv to your S3 bucket (e.g., s3://<bucket-name>/raw/songs.csv).
S3 buckets for storing:
Raw data (e.g., s3://<bucket-name>/raw/).
Processed data (e.g., s3://<bucket-name>/processed/).
Checkpoints for Spark streaming jobs.

2. Create a Kinesis Data Stream
Go to the Kinesis console.
Click Create Data Stream.
Provide a name (e.g., user_song_streams).
Set the number of shards (e.g., 1).
Click Create Stream.

3. Set Up the Data Producer
Install dependencies:
bash, Copiar, Editar

Update the script kinesis-producer.py with:
The correct region_name and stream_name.
Path to your streams.csv file.
Run the producer: bash, Copiar, Editar
python kinesis-producer.py

4. Configure Glue and Spark for Data Processing
Glue Setup:

Create an AWS Glue IAM Role with necessary permissions for Kinesis and S3.
Update the script streaming-etl.py with:
The Kinesis stream ARN.
S3 paths for the static dataset, processed data, and checkpoints.
Package your script as a .zip file and upload it to an S3 bucket (e.g., s3://<bucket-name>/scripts/streaming-etl.zip).

Run the Glue Job:

Go to the Glue console.
Create a new Glue job with the following configurations:
Script location: S3 path of your streaming-etl.zip.
IAM Role: The role created earlier.
Worker type: Choose an appropriate worker type (e.g., G.1X).
Spark parameters: Adjust --conf settings as needed.
Start the Glue Job:

Run the job and monitor its progress.

5. Monitor the Pipeline
Use the Kinesis console to monitor stream metrics.
Verify processed data in the S3 processed/ path.
Check Glue job logs in CloudWatch Logs for debugging.
Output
Aggregated metrics are written to S3 in Parquet format at s3://<bucket-name>/processed/.

Example Output

Window Time: Time window for aggregation.
Track ID: The song ID.
Total Listens: Total number of streams.
Unique Users: Number of unique users.
Average Listen Duration: Average duration of listening.
