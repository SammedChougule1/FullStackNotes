# ADO.NET

### arch
- Connected : bridges the gap between the disconnected DataSet/ DataTable objects and the physical database
- Disconnected : get data into 
DataAdapter, disconnect the database, manipulate the DataAdapter and 
resubmit the data. It is fast and robust

### Main components class
- DataSet : a container which gets the data from one 
or more then one tables from the database (disconnected arch)
- DataReader : read the data returned by a SELECT command. It is read only(connected arch)
- DataAdapter : bridges the gap between the disconnected DataSet/ DataTable objects and the physical database       