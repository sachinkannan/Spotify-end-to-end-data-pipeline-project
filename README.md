# Spotfiy Data Pipeline End-To-End Python Data Engineering Project

## Implement Complete Data Pipeline Data Engineering Project using Spotify

* Integrating with Spotify API and extracting Data
* Deploying code on AWS Lambda for Data Extraction
* Adding trigger to run the extraction automatically
* Writing transformation function
* Building automated trigger on transformation function
* Store files on S3 properly
* Building Analytics Tables on data files using Glue and Athena

## Objective
* Designing a fully-automated ETL pipeline on AWS to extract Spotify's weekly Top Songs - Global playlist and store the Artist, Album and Song data on AWS Glue Data Catalog tables.

## Solution Architecture
![Solution Architecture](https://github.com/sachinkannan/Spotify-end-to-end-data-pipeline-project/blob/main/spotify_pipeline-architecture.png)

## Implementation of Services
### Spotify API
![](https://github.com/sachinkannan/Spotify-end-to-end-data-pipeline-project/blob/main/spotify_api.png)
One would need developer app credentials (Client ID, Client Secret) to transact with the Spotify Web API. 

## AWS Services Used in this Project
![](https://github.com/sachinkannan/Spotify-end-to-end-data-pipeline-project/blob/main/aws_services_used_1.png)
![](https://github.com/sachinkannan/Spotify-end-to-end-data-pipeline-project/blob/main/aws_services_used_2.png)

### AWS Lambda - Data Extraction

Create the Lambda Function for Data Extraction spotify_api_data_extract which runs on an appropriate Python runtime (eg. Python 3.8+).
This function will be executed according to the schedule setup by the EventBridge Rule.
Add the data extraction code to the created lambda_function.py. Ensure that the spotipy package is available in connected Lambda Layer for it to be accessed in the function environment.

### Amazon S3 - for Raw and Transformed data

Create an S3 bucket spotify-etl-project-demo which will manage both raw data (from the Spotify API) as well as the processed structured data to be used downstream.
Create two primary folders: raw_data/ and transformed_data/.
raw_data/: will contain subfolders - processed/, to_processed/ which will contain and manage the raw playlist data.
transformed_data/: will contain subfolders - artist_data/, album_data/, song_data/ which will contain and manage the artist, album and song structured data.

### Amazon EventBridge

Create new EventBridge Rule to run at a given schedule (eg. every 1 day, week).
Set the trigger's target to be the Lambda function for Data Extraction from the Spotify API.

### AWS Lambda - Data Transformation

Create the Lambda Function for Data Transformation spotify_transformation_load_function which runs on an appropriate Python runtime (eg. Python 3.8+).
This function will be triggered whenever a new file is dropped into the raw_data/to_processed/ folder of the spotify-etl-project-demo S3 bucket.
The code will extract the artist, album and song data and save as structured data in the transformed folder, as well as manage the raw data by moving it into the processed folder.
Add the data transformation code to the created lambda_function.py. Ensure that the AWSSDKPandas-Python38 AWS Lambda Layer is added to the function for the pandas package to be accessed in the function environment.

### AWS Glue Crawler - infer schema & Amazon Athena - analytics

Create respective Glue Crawlers to infer schema of transformed artist, album and song data and to store their metadata in Glue Data Catalog.
Set the respective S3 data folders as the data source for the crawlers; also specify the schema metadata like headers, column datatypes by creating the corresponding Glue Classifiers. This will allow the crawler to correctly parse the files in the data source.
The output could be a specific database in the Glue Data Catalog.
Running the created Crawlers could be based on a specific schedule or even manually (eg. a day after new data arrives).
The Data Catalog database tables can be viewed and queried on Amazon Athena.
