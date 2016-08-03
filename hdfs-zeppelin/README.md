Process data from HDFS using Apache Zeppelin
============================================

# What is ?

Process movielense sample data using Apache Zeppelin.

# How to use this repo ?

1. Start Docker containers:
```
# docker-compose up
```
 * HDFS: http://localhost:50070
 * Zeppelin: http://localhost:8888
  * Login: admin/admin

# How to solve the problem

## Prepare 

* Place [`movie lense`](https://movielens.org/) data into `data/ml`

## 

* Open `PySpark Test Notebook` from zeppelin UI.
