Collecting data from sftp to HDFS using Apache Nifi
===================================================

# What is ?

Ingest data from sftp server to HDFS using apache nifi.

# How to use this repo ?

1. Start Docker containers:
```
# docker-compose up
```
 * HDFS: http://localhost:50070
 * Nifi: http://localhost:8080/Nifi
 * sftp: `sftp -p 2200 dap@localhost`

# How to solve the problem
## Nifi Design

1. Drag `ProcessGroup` and drop to main canvas
1. Configure the `ProcessGroup`
1. Enter into group to double click `ProcessGroup`
1. Drag `Process` and drop to canvas
 1. Find and select `ListSFTP`
 1. Configure `ListSFTP` process
  * Config `host` to `localhost`
  * Config `port` to `2200`
  * Config `user` to `dap`
  * Config `password` to `dap`
1. Drag `Process` and drop to canvas
 1. Find and select `FetchSFTP`
 1. Configure `FetchSFTP` process
1. Drag `Process` and drop to canvas
 1. Find and select `PutHDFS`
 1. Configure `PutHDFS` process. `core-site.xml` and `hdfs-site.xml` are in `/opt/hdfs`.
