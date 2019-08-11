### optimus
---
https://github.com/ironmussa/Optimus

```py
from optimus import Optimus
op= Optimus(verbose=True)


from pyspark.sql import SparkSession
from optimus import Optimus

spark = SparkSession.builder.appName('optimus').getOrCreate()
op= Optimus(spark)


df = op.load.csv("../examples/data/foo.csv")
df = op.load.json("../examples/data/foo.json")
df = op.load.json("https://raw.githubusercontent.com/ironmusa/Optimus/master/examples/data/foo.json")
df = op.load.parquet("../examples/data/foo.parquet")
df = op.load.excel("../examples/data/titanic3.xls")

df.save.csv("data/foo.csv")
df.save.json("data/foo.json")
df.save.parquet("data/foo.parquet")
op= Optimus(reqpositories = "myrepo", packages="org.apache.spark:spark-avro_2.12:2.4.3", jars="my.jar", driver_class_path="this_is_a_jar_class_path.jar", verbose=True)


from pyspark.sql.types import *
from datetime import date, datetime

df = op.create.df(
  [
    ("names", "str", True),
    ("height(ft)", "int", True),
    ("function", "str", True),
    ("rank", "int", True),
    ("age", "int", True),
    ("weight(t)", "float", True),
    ("japanese name", ArrayType(StringType()), True),
    ("last position seen", "str", True),
    ("date arrival", "str", True),
    ("last date seen", "str", True),
    ("attributes", ArrayType(FloatType()), True),
    ("DateType"),
    ("Tiemstamp"),
    ("Cybertronian", "bool", True),
    ("NullType", "null", True),
  ],
  [
    (),
  ], infer_schema = True).h_repartition(1)

df.table()









```

```
```

```
```


