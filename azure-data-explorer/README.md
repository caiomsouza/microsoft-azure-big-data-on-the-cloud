# Azure Data Explorer (Kusto)

Azure Data Explorer is a fast, fully managed data analytics service for real-time analysis on large volumes of data streaming from applications, websites, IoT devices, and more. Ask questions and iteratively explore data on the fly to improve products, enhance customer experiences, monitor devices, and boost operations. Quickly identify patterns, anomalies, and trends in your data. Explore new questions and get answers in minutes. Run as many queries as you need, thanks to the optimized cost structure. <BR>

https://azure.microsoft.com/en-us/services/data-explorer/<BR>


### ADX key takeaways
- Use cases: Log and telemetry data | Time Series | IoT | Log Analytics | Big Data Warehouse (Big Data Query Engine)
- Ad-hoc Exploration | Query Platform | Query capability, extreme Fast Query Platform designed for billions of rows (truly big data)
- Cache (Hot cache - SSD and Memory)
- No need to partition the data in ADX, it will take care for you. By default partition by time.
- Automatic Indexing
- When required, you can define your partition strategy together with the ADX Product Group team
- ADX Data Modelling: Flat Tables | Denormalized data to avoid joins
- Append-only data stores
- Fully-managed services
- Serverless, scalable PaaS
- Near/real-time analytics
- High volume, velocity, variety (structured, semi-structured, free-text)
- Relational query model:Filter, aggregate, join, calculated columns
- Posible to stop the service and stop paying for compute


### Things to consider 
- Learn a new language called Kusto 
- Insert only | after you load the table, you can not update a column or a record 
- Workaround (Keep history of the records | Force you to keep history)
- Only way to get data from ADX is using ADX, data stored in a proprietary format in ADX internal Blob Store.

### ADX vs SQL DW
- SQL DW: You can update and delete
- SQL DW: Design to build Data Warehouses (Star Schema - Fact and dimensions tables)
- ADX: Flexibility to deal with unstrucutured 

### Migrations
- ELT (Elastic Search) migration to ADX

## Hands on Demo 

### ADX - Code to create a table
```
.create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)
```

### ADX - Code to Ingest data into ADX table
```
.ingest into table StormEvents h'https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D' with (ignoreFirstRecord=true)
```

![Ingest](https://github.com/caiomsouza/microsoft-azure-big-data-on-the-cloud/blob/master/azure-data-explorer/images/IngestKustoTable.jpg)



### ADX - Code to Query a table
```
StormEvents
| sort by StartTime desc
| take 100
```

![Query](https://github.com/caiomsouza/microsoft-azure-big-data-on-the-cloud/blob/master/azure-data-explorer/images/KustoQuery.jpg)


### Config Power BI + ADX
![ADX Power BI](https://github.com/caiomsouza/microsoft-azure-big-data-on-the-cloud/blob/master/azure-data-explorer/images/PowerBI0.png)

### Tutorial: Visualize data from Azure Data Explorer in Power BI
https://docs.microsoft.com/en-us/azure/data-explorer/visualize-power-bi

## More Hands on Labs 

### Workshop to build scalable architectures with Azure Data Explorer (ADX)
https://github.com/neilmillingtonmicrosoft/Cloud.Scale.Analytics.ADX

### Azure Data Explorer - Kafka Integration - Hands On Lab series
https://github.com/anagha-microsoft/adx-kafkaConnect-hol

## Kafka + Data Lake + EventHub + Spark + Databricks 

### Ingest data from Kafka into Azure Data Explorer
https://docs.microsoft.com/en-us/azure/data-explorer/ingest-data-kafka

### Query data in Azure Data Lake using Azure Data Explorer
https://docs.microsoft.com/en-us/azure/data-explorer/data-lake-query-data

### Quickstart: Ingest sample data into Azure Data Explorer
https://docs.microsoft.com/en-us/azure/data-explorer/ingest-sample-data

### ADX Dashboards
https://preview.dataexplorer.azure.com/dashboards

### Ingest data from Event Hub into Azure Data Explorer
https://docs.microsoft.com/en-us/azure/data-explorer/ingest-data-event-hub#create-a-target-table-in-azure-data-explorer

### Azure Data Explorer Connector for Apache Spark
https://docs.microsoft.com/en-us/azure/data-explorer/spark-connector

### Connect to Azure Data Explorer from Azure Databricks by using Python
https://docs.microsoft.com/en-us/azure/data-explorer/connect-from-databricks

### ADX Concurency (Query limits)
https://docs.microsoft.com/en-us/azure/data-explorer/kusto/concepts/querylimits


### Features 
1. Low latency, adhoc querying service over big data
- Analytics database, with a hierarchical cache paradigm 
- Hot cache (in-memory + disk cache)
- Cold cache (blob storage)
2. Indexing, Consistency, Compression, Column store
- Automatic indexing
- Strong consistency
- Proprietary compressed column store format
3. Fit for purpose, big data solution for-
- Analytics on IoT telemetry
- Analytics on all manners of time series data 
- Analytics on all manners of logs
- Text search
- Geo-spatial analytics
- Advanced analytics
4. Kusto Query Language
- Rich parsing (json, XML and more) and analytics functionality
- Support for joins, aggregations, cross-database, cross-cluster queries
5. Server-side Programmability
- Transformations at ingestion time with update policy functionality
- Support for stored functions, materialized views (preview)
6. Geo-spatial querying
- Covered further on
7. Rich connector ecosystem
- Integration with Azure service and open source services for ingestion and visualization
8. Visualization support
- PowerBI, ADX dashboard (preview), Grafana, Kibana, Redash, Tableau, Sisense …
- Support for direct query for PowerBI
9. Advanced Analytics capabilities
- Time series functions, anomaly detection, forecasting, regression functions out of the box
- Ability to train models on ADX directly, and to score models as part of KQL queries
- Azure Machine Learning service integration for bring your own model
- In-line Python and R support
10. Support for stream ingestion
- Supports Event Hub for stream ingestion
11. Tools
- Web UI, Windows desktop UI, Notebook integration
12. SDKs in multiple languages
- .Net, Java, Python, Node, Go…
13. Automated Information Lifecycle Management
- Automated configurable policy-based eviction from hot cache, automated purge based on retention policy 
14. Scalable PaaS
- Cluster computing serverless PaaS, with scale up/down, out/in with options for auto-scale
15. Continuous export to data lake & External tables
- For historical data; For cost-optimization and deeper insights, join hot data in ADX with data in data lake
16. Azure Data Share integration
- Usage isolation, data sharing in-place and read-only

