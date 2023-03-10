<div align="center">

-----
A Benchmark for Real-Time Relational Data Feature Extraction.

[**What is FEBench?**](#-what-is-febench)
| [**Getting Started**](#%EF%B8%8F-quickstart)
| [**Contributing**](#%EF%B8%8F-contributing)
</div>

## Community

We deeply appreciate the invaluable effort contributed by our dedicated team of developers, supportive users, and esteemed industry partners.

- [Intel](https://www.intel.com/)
- [National University of Singapore](https://nus.edu.sg/)
- [Tsinghua University](https://www.tsinghua.edu.cn/en/)
- [4Paradigm](https://en.4paradigm.com/index.html)
- [OpenMLDB](https://github.com/4paradigm/OpenMLDB)
- [Apache Flink](https://flink.apache.org/)

## 💡 What is FEBench?

As online AI inference services have been rapidly deployed in many emerging applications such as fraud detection and recommendation, we have witnessed various systems developed for real-time feature extraction (RTFE) to compute real-time features over incoming new data tuples. Also, the RTFE procedures can be expressed in SQL like languages. 

However, there is no any study about the workload characteristics and benchmarks for RTFE, specifically in comparison with existing database workloads and benchmarks (such as TPC-C). Thus, we have cooperated with our industry partners and built a real-time feature extraction benchmark named FEBench. FEBench consists of selected datasets, query templates, and testing framework. We utilize FEBench to investigate the effectiveness of feature extraction systems and find all the tested systems (e.g., Flink, OpenMLDB) have their own problems in different aspects (e.g., overall latency, tail latency, and concurrency performance). 

See the detailed [technical report](https://github.com/decis-bench/febench/blob/main/report/febench.pdf)!

## ⚡️ Quickstart

This repository includes the following parts: (1) AI features, (2) OpenMLDB evaluation, (3) Flink evaluation.

### Part 1 (AI Features)

In the *features* folder: Check out the features utilized in each of the 6 AI tasks, which are generated by the commercial automated ML tool [HCML](https://en.4paradigm.com/product/hypercycle_ml.html) (the simplified version is available at *https://github.com/4paradigm/AutoX*).

### Part 2 (OpenMLDB Evaluation)

Step 1: Clone the repository

Step 2: Download the datasets and move the data files to the dataset directory

  ```sh
  wget -r -np -R "index.html*"  http://119.28.136.39/download/febench/data/; cp -r <dataset directory> ./dataset
  ```

> Note the data files are in parquet format.

Step 3: Start the cluster and enter the *OpenMLDB* folder

Step 4: Update the settings (the OpenMLDB cluster, data/query locations) in the configuration file (./conf/conf.properties)

  ```sh
DATASET_ID=5  
DATASET_NUM=6
...

HOST=127.0.0.1:xxxx
...

DATABASE_C3=q3_db
DEPLOY_NAME_C3=q3_db_service
DATA_FOLDER_C3=../dataset/Q3/
DEPLOY_SQL_C3=./fequery/Q3/Q3_deploy_benchmark.sql
CREATE_SQL_C3=./fequery/Q3/Q3_create_benchmark.sql
DROP_SQL_C3=./fequery/Q3/Q3_drop_benchmark.sql
...

  ```

> Note you can start a docker for ease of environment management.


Step 5: Run the testing script

5.1 Run the compile_test.sh file (for the first time),

```bash
./compile_test.sh <dataset_ID>
```

5.2 Otherwise,

```bash
./test.sh <dataset_ID>
```

![image](./imgs/openmldb-jmh.png)


### Part 3 (Flink Evaluation)

Repeat the 1-5 steps in *OpenMLDB Evaluation*. And there are a few new issues:

1. In Step 3, additionally start a disk-based storage engine (e.g., RocksDB) to persist the Flink table data.

2. In Step 5, indicate the <dataset_ID> when running the *compile_test.sh* script; and no parameter when running *test.sh*, e.g., 

```bash
./compile_test.sh 3 // compile and run the test of task3
./test.sh // rerun the test of task3
```
3. You need to rerun compile_test.sh if you modify the conf.properties. This is not needed for *OpenMLDB Evaluation*.

![image](./imgs/flink-jmh.png)

## ✉️ Contributing
FEBench is developed as an open platform to attract industry and academia to collaborate on the benchmark and further development of RTFE. Please leave your comments in the [Issues](https://github.com/decis-bench/febench/issues) if you would like to get involved or contribute!
