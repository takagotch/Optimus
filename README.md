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

import pandas as pd
pdf = pd.DataFrame({'A': {0: 'a', 1: 'b', 2: 'c', 3:'d'},
  'B': {0: 1, 1: 3, 2: 5,3:7},
    'C': {0: 2, 1: 4, 2: 6,3:None},
    'D': {0: '1980/04/10',1:'1980/04/10',2:'1980/04/10',3:'1980/04/10'}})
s_pdf = op.create.df(pdf=pdf)
s_pdf.table()


def func(value, arg):
  return "this was a number"
new_df = df\
  .rows.sort("rank", "desc")\
  .withColumn('new_age', df.age)\
  .cols.lower(["names", "function"])\
  .cols.date_transform("date arrval", "yyyy/MM/dd", "dd-MM-YYYY")\
  .cols.years_between("date arrival", "yyyy/MM/dd", "dd-MM-YYYY")\
  .cols.remove_accents("date arrival", "dd-MM-YYYY", output_cols = "from arrival")\
  .cols.remove_special_chars("names")\
  .rows.drop(df["rank"]>8)\
  .cols.rename(str.lower)\
  .cols.trim("*")\
  .cols.unnest("japanese name", output_cols="other names")\
  .cols.unnest("last position seen",separator=",", output_cols="pos")\
  .cols.drop(["last position seen", "japanese name", "date arrival", "cybertronian", "nulltype"])

df.table()

new_df.table()


from pyspark.sql import functions as F
def func(col_name, attr):
  return F.upper(F.col(col_name))


output_df = df.cols.apply(input_cols="names", output_cols=None,func=func)
output_df.table()

output_df = df.cols.apply(input_cols="names", output_cols="names_up",func=func)
output_df.table()

output_df = df.cols.apply(input_cols=["names", "function"], output_cols="_up", func=func)
output_df.table()

output_df =df.cols.apply(input_cols=["names","function"], output_cols=["names_up","function_up"],func=func)
output_df.table()

def func(value, args):
  return value + args[0] + args[1]
df.cols.apply("height(ft)",func,"int",[1,2]).table()


from pyspark.sql import functions as F
def func(col_name, args):
  return F.col(col_name)/20
df.cols.apply("height(ft)", func=func, args=20).table()

op.output("ascii")
op.output("html")

df = op.load.csv("https://raw.githubuercontent.com/ironmussa/Optimus/master/examples/data/Meteorite_Landings.csv").h_repartition()


op.profiler.run(df, "mass (g)", infer=False)

op.profiler.run(df, "name", infer=False)

op.profiler.run(df, "year", infer=True)

op.profiler.run(df, "mass (g)", infer=False, relative_error =1, approx_count=True)
df = op.load.excel("../examples/data/titanic3.xls")
df = df.rows.drop_na(["age","fare"])

df.plot.frequency("age")
df.plot.scatter(["fare", "age"], buckets=30)

df.plot.box("age")
df.plot.correlation("*")

df.outliers.tukey("age").select().table()
df.outliers.tukey("age").drop().table()
df.outliers.tukey("age").info()
df.outliers.z_score("age", threshold=2).drop()
df.outliers.modified_z_score("age", threshold = 2).drop()
df.outliers.mad("age", threshold = 2).drop()

from optimus import Optimus
op= Optimus(verbose=True)


from credentials import *
db = op.connect(
  db_type=DB_TYPE,
  host=HOST,
  database= DATABASE,
  user= USER,
  password = PASSWORD,
  port=PORT)
db.tables(limit="all")

db.table.show("*", 20)
db_ = db.table_to_df("places_interest").table()
db.df_to_table(df, "new_table")

df = op.load.json("https://raw.githubusercontent.com/ironmussa/Optimus/master/examples/data/too.json")

import requsts
def func_request(params):
  url = "https://jsonplaceholder.typicode.com/todos/" + str(params["id"])
  return requests.get(url)

def func_response(response):
  return response["title"]
  
e = op.enrich(host="localhost", port=27017, db_name="jazz")
df_result = e.run(df, func_request, func_response, calls= 60, period = 60, max_tries = 8)

df_result.table("all")
df_result.table()

df = op.read.csv("../examples/data/random.csv",header=True, sep=";")
from optimus.ml import keycollision as keyCol

df_kc = keyCol.fingerprint_cluster(df, 'STATTE')
df_kc.table()
df_kc.table()

keyCol.fingerprint_cluster(df, "STATE").to_json()
df_kc = keyCol.n_gram_fingerprint_cluster(df, "STATE", 2)
df_kc.table()
df_kc.table()

keyCol.n_gram_fingerprint_cluster(df, "STATE", 2).to_json()

from optimus.ml import distancecluster as dc
df_dc = dc.levenshtein_matrix(df, "STATE")
df_dc.table()

df_dc=dc.levenshtein_filter(df,"STATE")
df_dc.table()
df_dc.table()


df_dc = dc.levenshtein_cluster(df, "STATE")
df_dc.table()
df_dc.table()

dc.to_json(df, "STATE")


df_cancer = op.load.csv("https://raw.githubusercontent.com/ironmussa/Optimus/master/tests/data_cancer.csv")
columns = ['diagnosis', 'redius_mean', 'texture_mean', 'perimeter_mean', 'area_mean', 'smoothness_mean',
  'compactness_mean', 'concavity_mean', 'concave points_mean', 'symmetry_mean',
  'franctal_dimension_mean']
df_predict, rf_model = op.ml.random_forest(df_cancer, columns, "diagnosis")
```

```
```

```
```


