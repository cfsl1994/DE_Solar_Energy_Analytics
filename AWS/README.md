# Solar Energy Analytics
## Steps

#### AWS S3:
###### General configuration RAW ZONE
    - name -> YOUR RAW BUCKET ZONE
    - folder -> solar_energy_analytics 

###### General configuration STAGE ZONE
    - name -> YOUR STAGE BUCKET ZONE
    - folder -> solar_energy_analytics
        - sub folder -> cbs_data 
        - sub folder -> sp_data

###### General configuration ANALYTICS ZONE
    - name -> YOUR ANALYTICS BUCKET ZONE
    - folder -> solar_energy_analytics 

#### Follow the instructions here: https://aws.amazon.com/es/blogs/big-data/enrich-datasets-for-descriptive-analytics-with-aws-glue-databrew/

#### AWS GLUE
###### Data Catalog
    - name -> sp_catalog_db

###### Data Crawler
    - name -> crawler_raw
    - iam role -> YOUR IAM ROLE
    - database -> sp_catalog_db
    - table prefix -> raw_
    - data source type -> S3
    - data source -> s3://YOUR RAW BUCKET ZONE/solar_energy_analytics 
    - parameters -> recrawl all

###### DataBrew Datasets
    - name -> cbs-sp-cap-nl-dataset
    - data type -> data catalog table
    - source -> data catalog
    - data catalog -> raw_cbs_sp_cap_nl_csv
    - csv location -> s3://YOUR RAW BUCKET ZONE/solar_energy_analytics/cbs_sp_cap_nl.csv

    - name -> cbs-regions-nl-dataset
    - data type -> data catalog table
    - source -> data catalog
    - data catalog -> raw_cbs_regions_nl_csv
    - csv location -> s3://YOUR RAW BUCKET ZONE/solar_energy_analytics/cbs_regions_nl.csv

    - name -> sp-dataset
    - data type -> data catalog table
    - source -> data catalog
    - data catalog -> raw_sp_data_csv
    - csv location -> s3://YOUR RAW BUCKET ZONE/solar_energy_analytics/sp_data.csv

    - name -> transformed-cbs-dataset
    - data type -> awsgluedatabrew_transformed_cbs_data
    - source -> data catalog
    - data catalog -> awsgluedatabrew_transformed_cbs_data
    - csv location -> s3://YOUR STAGE BUCKET ZONE/solar_energy_analytics/cbs_data/YOUR FILE GENERATED

    - name -> transformed-sp-dataset
    - data type -> awsgluedatabrew_transformed_sp_data
    - source -> data catalog
    - data catalog -> awsgluedatabrew_transformed_sp_data
    - csv location -> s3://YOUR STAGE BUCKET ZONE/solar_energy_analytics/sp_data/YOUR FILE GENERATED

###### DataBrew Recipes
    - Recipe 1 -> recipe-1-transform-cbs-data.json
    - Recipe 2 -> recipe-2-transform-sp-data.json
    - Recipe 3 -> recipe-3-curate-sp-cbs-data.json

###### DataBrew projects and jobs
    - name -> 1-transform-cbs-data
    - associated dataset -> cbs-sp-cap-nl-dataset
    - attached recipe -> recipe-1-transform-cbs-data
    - jobs -> 1-transform-cbs-data-job

    - name -> 2-transform-sp-data
    - associated dataset -> sp-dataset
    - attached recipe -> recipe-2-transform-sp-data
    - jobs -> 2-transform-sp-data-job

    - name -> 3-curate-sp-cbs-data
    - associated dataset -> transformed-sp-dataset
    - attached recipe -> recipe-3-curate-sp-cbs-data
    - jobs -> 3-curate-sp-cbs-data-job

#### AWS QUICKSIGHT
    - The analysis of the data will be at your discretion.
    - Athena as the data source
    - Data source name -> sp_data_source
    - catalog -> AWSDataCatalog
    - database -> sp_catalog_db

## Architecture-Diagram
![Architecture-Diagram](diagram-aws.png)