# About the Data
[UBC's SkySpark IoT platform](https://skyspark.energy.ubc.ca/), managed by UBC Energy and Water Services (EWS), collects data on weather and UBC buildings every 15 minutes. The Urban Data Lab (UDL) is streaming the SkySpark data into an InfluxDB instance to increase its accessibility and usability.
![](https://github.com/UBC-UrbanDataLab/SkySpark_data/blob/master/images/InfluxDB_UI_Example.PNG)

## InfluxDB 2.0 Instance
Public users (read permissions only) can log in to our **InfluxDB 2.0 User Interface** http://206.12.92.81:8086/ with the following credentials
- Username:`public02`
- Password:`public02`

Public users can also use this authorization token `omUybYZ3QkGvuXXy0VwT-7hoO2SEFzhckXJ5k32K_GvG47yHQAi9JzZ1bii6r1HD5NKux3ZhHlKAyUfj6i61bA==` 
to access this InfluxDB database from [InfluxDB command line interface](https://docs.influxdata.com/influxdb/v2.0/) or [InfluxDB API client libraries](https://docs.influxdata.com/influxdb/v2.0/tools/client-libraries/). 

[The Python tutorial](https://github.com/UBC-UrbanDataLab/SkySpark_data/blob/master/SKYSPARK2%20Tutorial%20v1.ipynb) demonstrates querying the InfluxDB database using the `influxdb-client` Python module. Please [contact UDL](https://urbandatalab.io/) if you have any questions.

## Database Structure
The `SKYSPARK` database is a hitorical copy of the SkySpark data in 2020, and the live streaming has been terminated. Please refer to the diagram below for its structure.
![](https://github.com/UBC-UrbanDataLab/SkySpark_data/blob/master/images/SKYSPARK%20Structure.JPG)

Currently, UDL is developing the database  `SKYSPARK2` which will provide cleaner data on more buildings.

**[InfluxDB concepts](https://docs.influxdata.com/influxdb/v2.0/reference/key-concepts/)**: database, point, measurement, tag, field, timestamp

**Notes**

1. For each data point on SkySpark, the `id` in `POINT` is the same as the `uniqueID` in `READINGS`.
2. A SkySpark data point can have up to 155 tags (recorded in the `POINT` measuremt). Each data point usually has the following tagging information so we also record these tags in `READINGS` to simplify queries.
   * siteRef: a site is a facility (usually a building) 
   * groupRef: a group refers to a system or a floor in a building
   * typeRef: type of the data point  
   * equipRed: information on the equipment
   * navName: information interpreted and added by SkySpark managers
   * unit: the unit of the reading value
3. Each data point has the reading values stored in one of the following fields in `READINGS`:
   * val_num: float values
   * val_bool: boolean values
   * val_str: string values
   * val_unk: value format unknown and stored as string
4. Timestamps are in UTC on InfluxDB: `time` in `READINGS` is the time the reading values are recorded, and `time` in `POINT` is a constant value to allow overwriting the existing data (this happens at 2am daily).
