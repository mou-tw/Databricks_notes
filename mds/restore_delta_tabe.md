## prepare data
```
from pyspark.sql.types import StructType,StructField, StringType, IntegerType

data1 = [("1","James",3000),
    ("1","Michael",4000),
    ("1","Robert",4000),
    ("1","Maria",4000),
    ("1","Jen",5000),
    ("2","Jack",2000),
    ("2","Alex",2000),
    ("2","Mary",2000),     
  ]

data2 = [
    ("2","Jack",3000),
    ("2","Alex",3000),
    ("2","Mary",4000),     
    ("3","John",2000),     
    ("3","Walker",2000),     
    ("3","Jalan",2000),     
  ]


schema = StructType([ \
    StructField("No",StringType(),True), \
    StructField("Name",StringType(),True), \
    StructField("salary", IntegerType(), True) \
  ])
 
df1 = spark.createDataFrame(data=data1,schema=schema)
df2 = spark.createDataFrame(data=data2,schema=schema)

```
## save/overwrite data as delta format

```
df1.write.mode('overwrite').saveAsTable('ods.delta_rollback_test')
df2.write.mode('overwrite').saveAsTable('ods.delta_rollback_test')
```

## show delta table modify history
```
%sql
describe history ods.delta_rollback_test
```
![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/restore_delta_1.JPG  )


## check data
```
%sql 
select * from ods.delta_rollback_test
```
![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/restore_delta_2.JPG  )


## restore data to past version
```
%sql 
RESTORE TABLE ods.delta_rollback_test TO VERSION AS OF 0
```

## check modigy history again
```
%sql
describe history ods.delta_rollback_test
```

![pic](https://github.com/mou-tw/Databricks_notes/blob/main/img/restore_delta_3.JPG  )

## check data
```
%sql 
select * from ods.delta_rollback_test
```
