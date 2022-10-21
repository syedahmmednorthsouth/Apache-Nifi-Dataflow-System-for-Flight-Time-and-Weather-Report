# Apache-Nifi-Dataflow-System-for-Flight-Time-and-Weather-Report
 
 ## overview 
 
 In this project, I'll demonstrate how to write data from two distinct APIs to many databases and send slack alerts.
 
 ![overviw](https://github.com/syedahmmednorthsouth/Apache-Nifi-Dataflow-System-for-Flight-Time-and-Weather-Report/blob/main/attachment/overview.png)
 
 
## full nifi template

Aviationstack.com will be our first API to use. Through this API, we may get real-time flight information.

Visualcrossing.com will serve as our second API.

We may retrieve the weather information for any city using this API.

From the information we obtained from the first API, we will choose a city and retrieve the flight details.

Along with the data we receive from the other API, we will also include the local weather for the chosen city.

Call the processor from which we will retrieve data from the API first.

![fullNifi](https://github.com/syedahmmednorthsouth/Apache-Nifi-Dataflow-System-for-Flight-Time-and-Weather-Report/blob/main/attachment/nifi.png)

## steps 

![invokehttp](https://github.com/syedahmmednorthsouth/Apache-Nifi-Dataflow-System-for-Flight-Time-and-Weather-Report/blob/main/attachment/invokehttp.png)


Data is returned in this kind of JSON format when the Processor has completed.


### Results from the API.
Then, we used the SplitJSON processor to divide the data into "days."
We have access to every record in this fashion.



### Processor for SplitJSON
The data will now be sent to three distinct databases.

### MongoDB
Using the PutMONGO processor, we will write the data from the SplitJSON processor to mongodb.



### MongoDBControllerService and PutMongo
This is the result.


## PostgreSQL
We decide the records we'll utilize before we write the data to PostgreSQL.
For this, we employ the JoltTransformJSON processor.



### JoltTransformJSON
We must now translate the JSON data into DML (Insert) instructions before adding the data to PostgreSQL as a record.
The PostgreSQL database's information is then entered.


### Service for DBCPConnectionPool and ConvertJSONtoSQL
We were able to update the database with our records.


##  Google BigQuery
The data will now be imported into the BigQuery environment. But first, the JSON data will be read and converted to AVRO format, and then it will be added to BigQuery in CSV format. In the following step, we choose the schema name we require before doing these activities.


The schema name required for the following step
From the Record Reader area, we choose JSONTreeReader.
From the Record Writer area, we choose CSVRecordSetWriter.


ConvertRecord
The schema name and schema registry from the previous step are entered into the JSONTreeReader service when it is opened.


JsonTreeReader
The records are then converted to AVRO format by opening the AvroSchemaRegistery section.
