Implement Doris read and write operations in EMR Serverless Spark
===========================================================================
Leveraging the official Spark Connector from Apache Doris, EMR Serverless Spark can be configured to connect to Doris during development. This topic explains how to execute data read and write operations with Doris within the EMR Serverless Spark environment.

**Background information**

Apache Doris is a high-performance, real-time analytics database suitable for report analysis, ad hoc queries, and data lake federated query acceleration. For more information, see Introduction to Apache Doris.

EMR Serverless Spark is a high-performance Lakehouse product compatible with open-source Spark, offering fully managed enterprise-level data platform services. Integrating Apache Doris with EMR Serverless Spark enables efficient data read, write, and analysis operations, facilitating a complete data processing workflow.

**Prerequisites**
----------------------------------

* Airflow is installed and started. For more information, see [Installation of Airflow](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html).

* A workspace is created. For more information, see [Create a workspace](t2488607.md#).

**Limits**
The Serverless Spark engine must be version esr-2.5.0, esr-3.1.0, esr-4.1.0, or later.

**Usage notes**
--------------------------------

You cannot call the EmrServerlessSparkStartJobRunOperator operation to query job logs. If you want to view job logs, you must go to the EMR Serverless Spark page and find the job run whose logs you want to view by job run ID. Then, you can check and analyze the job logs on the **Logs** tab of the job details page or on the Spark Jobs page in the **Spark UI**.

**Procedure**
------------------------------

### **Step 1:** Configure Apache Airflow

1. Download [airflow_alibaba_provider-0.0.3-py3-none-any.whl](https://help-static-aliyun-doc.aliyuncs.com/file-manage-files/en-US/20241206/fwrfkp/airflow_alibaba_provider-0.0.3-py3-none-any.whl).

2. Install the airflow-alibaba-provider plug-in on each node of Airflow.

   The airflow-alibaba-provider plug-in is provided by EMR Serverless Spark. It contains the EmrServerlessSparkStartJobRunOperator component, which is used to submit jobs to EMR Serverless Spark.

   ```sh
   pip install airflow_alibaba_provider-0.0.3-py3-none-any.whl
   ```
3. Add a connection.

   Use the CLI
   ----------------------------

   Use the Airflow command-line interface (CLI) to run commands to establish a connection. For more information, see [Creating a Connection](https://airflow.apache.org/docs/apache-airflow/stable/howto/usage-cli.html#creating-a-connection).

   ```sh
   airflow connections add 'emr-serverless-spark-id' \
       --conn-json '{
           "conn_type": "emr_serverless_spark",
           "extra": {
               "auth_type": "AK", # The AccessKey pair is used for authentication. 
               "access_key_id": "<yourAccesskeyId>", # The AccessKey ID of your Alibaba Cloud account. 
               "access_key_secret": "<yourAccesskeyKey>", # The AccessKey secret of your Alibaba Cloud account. 
               "region": "<yourRegion>"
           }
       }'
   ```

  ```

### **Step 2:** Configure **DAGs**

Apache Airflow provides Directed Acyclic Graphs (DAGs), which allow you to declare how jobs should run. The following are examples of how to call the EmrServerlessSparkStartJobRunOperator operation to run different types of Spark jobs in Apache Airflow.
### Submit a JAR package

Use an Airflow task to submit a precompiled Spark JAR job to EMR Serverless Spark.

```python
from __future__ import annotations

from datetime import datetime

from airflow. models. dag import DAG
from airflow_alibaba_provider.alibaba.cloud.operators.emr import EmrServerlessSparkStartJobRunOperator

# Ignore missing args provided by default_args
# mypy: disable-error-code="call-arg"

DAG_ID = "emr_spark_jar"

with DAG(
    dag_id=DAG_ID,
    start_date=datetime(2024, 5, 1),
    default_args={},
    max_active_runs=1,
    catchup=False,
) as dag:
    emr_spark_jar = EmrServerlessSparkStartJobRunOperator(
        task_id="emr_spark_jar",
        emr_serverless_spark_conn_id="emr-serverless-spark-id",
        region="cn-hangzhou",
        polling_interval=5,
        workspace_id="w-7e2f1750c6b3****",
        resource_queue_id="root_queue",
        code_type="JAR",
        name="airflow-emr-spark-jar",
        entry_point="oss://<YourBucket>/spark-resource/examples/jars/spark-examples_2.12-3.3.1.jar",
        entry_point_args=["1"],
        spark_submit_parameters="--class org.apache.spark.examples.SparkPi --conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark.driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None
    )

    emr_spark_jar
```

### Submit an SQL file

Run SQL commands in Airflow DAGs.

```python
from __future__ import annotations

from datetime import datetime

from airflow. models.dag import DAG
from airflow_alibaba_provider.alibaba.cloud.operators.emr import EmrServerlessSparkStartJobRunOperator

# Ignore missing args provided by default_args
# mypy: disable-error-code="call-arg"

ENV_ID = os.environ.get("SYSTEM_TESTS_ENV_ID")
DAG_ID = "emr_spark_sql"

with DAG(
    dag_id=DAG_ID,
    start_date=datetime(2024, 5, 1),
    default_args={},
    max_active_runs=1,
    catchup=False,
) as dag:
    emr_spark_sql = EmrServerlessSparkStartJobRunOperator(
        task_id="emr_spark_sql",
        emr_serverless_spark_conn_id="emr-serverless-spark-id",
        region="cn-hangzhou",
        polling_interval=5,
        workspace_id="w-7e2f1750c6b3****",
        resource_queue_id="root_queue",
        code_type="SQL",
        name="airflow-emr-spark-sql",
        entry_point=None,
        entry_point_args=["-e","show tables;show tables;"],
        spark_submit_parameters="--class org.apache.spark.sql.hive.thriftserver.SparkSQLCLIDriver --conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark. driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None,
    )

    emr_spark_sql

```

### Submit an SQL file from OSS

Run the SQL script file obtained from OSS.

```python
from __future__ import annotations

from datetime import datetime

from airflow. models.dag import DAG
from airflow_alibaba_provider.alibaba.cloud.operators.emr import EmrServerlessSparkStartJobRunOperator

# Ignore missing args provided by default_args
# mypy: disable-error-code="call-arg"

DAG_ID = "emr_spark_sql_2"

with DAG(
    dag_id=DAG_ID,
    start_date=datetime(2024, 5, 1),
    default_args={},
    max_active_runs=1,
    catchup=False,
) as dag:
    emr_spark_sql_2 = EmrServerlessSparkStartJobRunOperator(
        task_id="emr_spark_sql_2",
        emr_serverless_spark_conn_id="emr-serverless-spark-id",
        region="cn-hangzhou",
        polling_interval=5,
        workspace_id="w-ae42e9c92927****",
        resource_queue_id="root_queue",
        code_type="SQL",
        name="airflow-emr-spark-sql-2",
        entry_point="",
        entry_point_args=["-f", "oss://<YourBucket>/spark-resource/examples/sql/show_db.sql"],
        spark_submit_parameters="--class org.apache.spark.sql.hive.thriftserver.SparkSQLCLIDriver --conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark. driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None
    )

    emr_spark_sql_2

```

### Submit a Python script from OSS

Run the Python script file obtained from OSS.

```python
from __future__ import annotations

from datetime import datetime

from airflow. models.dag import DAG
from airflow_alibaba_provider.alibaba.cloud.operators.emr import EmrServerlessSparkStartJobRunOperator

# Ignore missing args provided by default_args
# mypy: disable-error-code="call-arg"

DAG_ID = "emr_spark_python"

with DAG(
    dag_id=DAG_ID,
    start_date=datetime(2024, 5, 1),
    default_args={},
    max_active_runs=1,
    catchup=False,
) as dag:
    emr_spark_python = EmrServerlessSparkStartJobRunOperator(
        task_id="emr_spark_python",
        emr_serverless_spark_conn_id="emr-serverless-spark-id",
        region="cn-hangzhou",
        polling_interval=5,
        workspace_id="w-ae42e9c92927****",
        resource_queue_id="root_queue",
        code_type="PYTHON",
        name="airflow-emr-spark-python",
        entry_point="oss://<YourBucket>/spark-resource/examples/src/main/python/pi.py",
        entry_point_args=["1"],
        spark_submit_parameters="--conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark.driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None
    )

    emr_spark_python

```
                                                                                                                     |
