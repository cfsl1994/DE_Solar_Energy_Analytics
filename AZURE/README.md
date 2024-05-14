# Solar Energy Analytics
## Steps

#### AZURE BLOB STORAGE:
###### General configuration RAW ZONE
    - name -> YOUR RAW ZONE BLOB STORAGE
    - type -> storage account
    - kind -> storage v2
    - resource group -> YOUR RESOURCE GROUP
    - container -> solar-energy-analytics

###### General configuration STAGE ZONE
    - name -> YOUR STAGE ZONE BLOB STORAGE
    - type -> storage account
    - kind -> storage v2
    - resource group -> YOUR RESOURCE GROUP
    - container -> solar-energy-analytics

###### General configuration ANALYTICS ZONE
    - name -> YOUR ANALYTICS ZONE BLOB STORAGE
    - type -> storage account
    - kind -> storage v2
    - resource group -> YOUR RESOURCE GROUP
    - container -> solar-energy-analytics


#### AZURE DATA FACTORY:
###### Linked Services adls_analytics_cbs_dataset
    - name -> adls_analytics_cbs_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR ANALYTICS ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Linked Services adls_raw_cbs_regions_nl_dataset
    - name -> adls_raw_cbs_regions_nl_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR RAW ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Linked Services adls_raw_cbs_sp_cap_nl_dataset
    - name -> adls_raw_cbs_sp_cap_nl_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR RAW ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Linked Services adls_raw_sp_data_dataset
    - name -> adls_raw_sp_data_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR RAW ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Linked Services adls_stage_cbs_data_dataset
    - name -> adls_stage_cbs_data_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR STAGE ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Linked Services adls_stage_sp_data_dataset
    - name -> adls_stage_sp_data_dataset
    - type -> Azure Data Lake Storage Gen2
    - connect via integration runtime -> AutoResolveIntegrationRuntime (default)
    - url -> https://YOUR STAGE ZONE BLOB STORAGE.dfs.core.windows.net/
    - storage account key -> YOUR KEY

###### Dataset analytics_cbs_dataset
    - linked service -> adls_analytics_cbs_dataset
    - file path -> solar-energy-analytics
    - compression type -> snappy

###### Dataset raw_cbs_regions_nl_dataset
    - linked service -> adls_raw_cbs_regions_nl_dataset
    - file path -> solar-energy-analytics/cbs_regions_nl.csv
    - column delimiter -> semicolon(;)
    - first row as header -> yes
    - All default

###### Dataset raw_cbs_sp_cap_nl_dataset
    - linked service -> adls_raw_cbs_sp_cap_nl_dataset
    - file path -> solar-energy-analytics/cbs_sp_cap_nl.csv
    - column delimiter -> semicolon(;)
    - first row as header -> yes
    - All default

###### Dataset raw_sp_data_dataset
    - linked service -> adls_raw_sp_data_dataset
    - file path -> solar-energy-analytics/sp_data.csv
    - column delimiter -> comma(,)
    - first row as header -> yes
    - All default

###### Dataset stage_cbs_data_dataset
    - linked service -> adls_stage_cbs_data_dataset
    - file path -> solar-energy-analytics/cbs_data/
    - column delimiter -> comma(,)
    - first row as header -> yes
    - All default

###### Dataset stage_sp_data_dataset
    - linked service -> adls_stage_sp_data_dataset
    - file path -> solar-energy-analytics/sp_data/
    - column delimiter -> comma(,)
    - first row as header -> yes
    - All default

###### Data Flow recipe_1_transform_cbs_dataflow
    - configuration -> recipe_1_transform_cbs_dataflow.json

###### Data Flow recipe_2_transform_sp_dataflow
    - configuration -> recipe_2_transform_sp_dataflow.json

###### Data Flow recipe_3_curate_sp_cbs_dataflow
    - configuration -> recipe_3_curate_sp_cbs_dataflow.json

###### Data Pipeline solar-energy-analytics-datapipeline
    - configuration -> solar-energy-analytics-datapipeline.json


#### AZURE SYNAPSE ANALYTICS:
###### Properties Data Lake DB
    - name -> solar_energy_analytics_db 
    - linked service ->  YOUR LINKED SERVICE
    - folder -> solar-energy-analytics
    - file format -> parquet

###### Properties tbl_solar_energy_analytics
    - name table -> tbl_solar_energy_analytics
    - linked service ->  YOUR LINKED SERVICE
    - folder -> solar-energy-analytics/result_recipe3.parquet
    - file format -> parquet
    - compression -> none
    - All default

## Architecture-Diagram
![Architecture-Diagram](DE_Solar_Energy_Analytics-AZURE.jpg)