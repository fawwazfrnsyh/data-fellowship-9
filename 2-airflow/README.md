## Data Ingestion
Load data from data source to Big Query using Apache Airflow

### Tech
1. Google Cloud Composer
2. Google Cloud Storage
3. Google Big Query
4. Apache Airflow

### File
1. data_ingestion_gcs_dag.py
2. data source from www.nyc.gov/site/tlc/about/tlc-trip-record-data.page#

## Setup
### 1. Google Cloud Composer
Composer is a managed Airflow environment on Google Cloud Platfrom (GCP). With Composer, you don't have to spend more time managing Airflow; You can focus on writing DAGs. 

Steps to activate the environment:
1. Activate Composer API if you haven't. You can refer to this link on how to: https://cloud.google.com/endpoints/docs/openapi/enable-api#console
2. Open Google Cloud Console, on Navigation Menu go to Big Data > Composer, or simply search it at the search bar.
3. Click **CREATE button** to create new Composer environment (I choose Composer 1)
4. Fill some required fields there. This projects use this machine config:
- Name and Location: You can write or choose freely in this section (I choose location "asia-southeast1", and I recommend to always use the same location for all process)
- Image Version: composer-1.20.7-airflow-2.2.5
- Node count: 3
- Zone: I use asia-central-a
- Machine Type: n1-standard-1
- Disk size (GB): 30 GB
- Service account: choose your Compute Engine service account.
- Cloud SQL machine type: db-n1-standard-2
- Web server machine type: composer-n1-webserver2
- Environment Variable : **ENVIRONMENT"** and **production**
5. Then, click the **CREATE** button
6. You will need to wait a few minutes (20-40 minutes) for the environment to be successfully created. After the environment has been created successfully you will see like this:

![Screenshot 2023-03-09 223816](https://user-images.githubusercontent.com/107783827/224074938-4ef69730-4f70-499e-9ae5-82c227c5a399.png)

### 2. Google Cloud Storage
If the environment has been created successfully, then in GCS a bucket will appear automatically as below. (Note: the name of the bucket will adjust to the location when creating composer)

![image](https://user-images.githubusercontent.com/107783827/224075800-925c4063-d9aa-4a2e-b7f5-7ffe7d2c7013.png)
- Open the bucket, find the **dags** folder
- Upload the DAG file into the dags folder

### 3. Google BigQuery
1. On your GCP console, go to BigQuery. You can find it on More Products > Analytics > BigQuery, or simply search it at the search bar.
2. Create dataset >> fill the Dataset field >> Click **CREATE DATASET** (or you can use an existing dataset)
3. We can skip to create table because it will generate automatically

### 4. Apache Airflow
#### Setup Variable
1. Return to composer and open the previously created environment, then open the **ENVIRONMENT CONFIGURATION**
2. Find the **Airflow Web UI** and click the link. It will takes you to Airflow UI
3. Make sure the DAG file that has been uploaded to the bucket appears in Airflow
4. Go to the Admin >> Variables and add new record

![image](https://user-images.githubusercontent.com/107783827/224084572-51967234-eebb-4ffa-8750-687a2dd4720d.png)

Here are instructions for filling in variables
- ALL_KEYWORD_BQ_OUTPUT_TABLE : [your bigquery table].output >> (note: same as in the DAG file)
- BIGQUERY_DATASET : [your gcp project id].[your dataset]
- BUCKET_NAME : [your bucket for DAG]
- EVENTS_BQ_TABLE : [your bigquery table].event >> (note: same as in the DAG file)
- GCS_STG_LOCATION : [your bucket for staging] >> (note: you can create new folder in the bucket)
- GCS_TEMP_LOCATION : [your bucket for temp] >> (note: you can create new folder in the bucket)
- MOST_SEARCH_KEYWORDS_BQ_OUTPUT_TABLE : [your bigquery table].search >> (note: same as in the DAG file) 
- PROJECT_ID : [your gcp project id]

#### Monitoring
1. After the variables is set, you can run the DAG by clicking on the arrow button and choose **Trigger DAGs**.
2. You can click on the DAG name to see the process when the ingestion is running. There is also a color description to find out whether ingesting has been successful or failed.

![image](https://user-images.githubusercontent.com/107783827/224080952-49e8d131-7c0f-4755-8e3a-3b7c2dea4b80.png)

## Check the Data
1. Open BigQuery. If the process is successful, it will be a new table and new data from the data source.

![image](https://user-images.githubusercontent.com/107783827/224088524-825d0d6f-c604-4a9b-865b-252e1075dd77.png)

2. You can check the data by querying in the query editor

![image](https://user-images.githubusercontent.com/107783827/224088904-79c78dd0-28ab-4f2d-8354-1931bdabe5a0.png)
