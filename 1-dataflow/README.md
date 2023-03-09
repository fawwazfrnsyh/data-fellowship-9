# Move Data from Google Cloud Storage to Bigquery using Dataflow
Load data from data source page into Google Big Query

## Tech
1. Google Cloud Storage
2. Google Cloud BigQuery
3. Google Cloud Dataflow

## File
1. citizen.txt
2. citizen.js
3. BIG_QUERY_SCHEMA.JSON

## Setup
## 1. Cloud Storage
1. Open your GCP console, choose Cloud Storage. You can find it on More Products > Storage > Cloud Storage, or simply search it at the search bar.
2. Click **CREATE BUCKET** button. Then fill some fields such as:
 - Name your bucket (example: testing_session1)
 - Choose the location (I recommend to always use the same location for all process)
 - Leave default for the rest of fields (just click continue)
 - Click **CREATE**
 - Your bucket will be created and showed on GCS Browser
 
 ![Screenshot 2023-03-09 205758](https://user-images.githubusercontent.com/107783827/224048620-ec659d30-1841-4200-b1a7-039779dbb5e5.png)
 
 3. Upload files to bucket. You can find the file in the dataflow-function folder
 
## 2. BigQuery
1. On your GCP console, go to BigQuery. You can find it on More Products > Analytics > BigQuery, or simply search it at the search bar.
2. Create dataset

![Screenshot 2023-03-09 210654](https://user-images.githubusercontent.com/107783827/224049808-320a5fdc-b212-42cc-97a5-35f52c13a46c.png)

3. Fill the Dataset field such as:
 - Dataset ID (example: testing_dataflow)
 - Data location. (I recommend to always use the same location for all process)
4. Click **CREATE DATASET**
6. After the dataset is ready, you can Create table

![Screenshot 2023-03-09 211201](https://user-images.githubusercontent.com/107783827/224051002-9043df90-5f0f-49fc-98eb-c999074bdad5.png)

7. Fill the Table field such as:
 - table name
 - schema (can be skipped or you can create by using text -> you can check in "BIGQUERY_SCHEMA_EDIT in dataflow function folder)

## 4. Dataflow
1. On your GCP console, go to Dataflow. You can find it on More Products > Analytics > Dataflow, or simply search it at the search bar.
2. Make sure that you have enabled API.
3. Click **CREATE JOB FROM TEMPLATE**
4. Fill the field such as:
  - Jobname
  - Regional endpoint: (same location as before)
  - Dataflow template:
    You can chosee between Process Data in Bulk (batch) or Process Data Continuesly (stream)
    Then choose "Text Files on Cloud Storage to BigQuery"
  - Javascript path: open your js file in bucket, and copy the gsutil URL.
  - Json path: open your json file in bucket
  - Javascript UDF name: the function to call from your JavaScript file (in this project, the function to call javascript is "transform")
  - Bigquery output table (browse your output table)
  - Cloud storage input path : open your txt file in bucket
  - Temporary Bigquery directory (create your temp folder on bucket: format gss://(your bucket)/(folder name)/ )
  - Temporary location (create your temp folder on bucket: format gss://(your bucket)/(folder name)/ )
   
5. Click **RUN JOB**
7. Check the table output in BigQuery to ensure the data has been loaded.
