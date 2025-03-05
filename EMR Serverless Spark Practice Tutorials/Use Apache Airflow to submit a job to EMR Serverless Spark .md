Apache Airflow is a powerful workflow automation and scheduling tool that allows developers to orchestrate, schedule, and monitor the running of data pipelines. E-MapReduce (EMR) Serverless Spark provides a serverless computing environment for processing large-scale data processing jobs. This topic describes how to use Apache Airflow to enable automatic job submission to EMR Serverless Spark. This way, you can automate job scheduling and running to manage data processing jobs more efficiently. {#b62d44e7f4gk3}

**Prerequisites**
----------------------------------

* Airflow is installed and started. For more information, see [Installation of Airflow](https://airflow.apache.org/docs/apache-airflow/stable/installation/index.html){#456f88e52cj1s}.


* A workspace is created. For more information, see [Create a workspace](t2488607.md#){#3bdf17bfe2sex}.


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

   Use the Airflow command-line interface (CLI) to run commands to establish a connection. For more information, see [Creating a Connection](https://airflow.apache.org/docs/apache-airflow/stable/howto/usage-cli.html#creating-a-connection){#93f29ab539z36}. {#74f21e2ff37lc}

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

   {#1c54b078eeu2x}

   Use the UI {#cacab93e11u0w}
   ---------------------------

   You can manually create a connection with the Airflow web UI. For more information, see [Creating a Connection with the UI](https://airflow.apache.org/docs/apache-airflow/stable/howto/connection.html#creating-a-connection-with-the-ui)

   On the **Add Connection** page, configure the parameters.

   ![image](../images/p800766.png)

   The following table describes the parameters:

   |---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
   | **Parameter**       | **Description**                                                                                                                                                                                                                                                                                                                                                                                                   |
   | **Connection Id**   | The connection ID. In this example, enter emr-serverless-spark-id.                                                                                                                                                                                                                                                                                                                                                |
   | **Connection Type** | The connection type. In this example, select **Generic** . If Generic is not available, you can also select **Email** .                                                                                                                                                                                                                                                                                           |
   | **Extra**           | The additional configuration. In this example, enter the following content: {#b19786848dkhw} ```sh { "auth_type": "AK", # The AccessKey pair is used for authentication. "access_key_id": "<yourAccesskeyId>", # The AccessKey ID of your Alibaba Cloud account. "access_key_secret": "<yourAccesskeyKey>", # The AccessKey secret of your Alibaba Cloud account. "region": "<yourRegion>" } ``` {#c3bfeaf593uqg} |

   {#2ee57026375s0}{#6923f3d76afpd}
{#6923f3d76afpd}

{#d80895d63ahk4}

### **Step 2:** Configure **DAGs** {#4e7075e92bli0}

Apache Airflow provides Directed Acyclic Graphs (DAGs), which allow you to declare how jobs should run. The following are examples of how to call the EmrServerlessSparkStartJobRunOperator operation to run different types of Spark jobs in Apache Airflow. {#d025271874vq3}

### Submit a JAR package {#89a6e01dd3hz2}

Use an Airflow task to submit a precompiled Spark JAR job to EMR Serverless Spark. {#0e3064baa5kd1}

```python
from __future__ import annotations

from datetime import datetime

from airflow.models.dag import DAG
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

{#0236abbbe8fna}

### Submit an SQL file {#4c73eb18f9xoc}

Run SQL commands in Airflow DAGs. {#e5156c1fcdj40}

```python
from __future__ import annotations

from datetime import datetime

from airflow.models.dag import DAG
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
        spark_submit_parameters="--class org.apache.spark.sql.hive.thriftserver.SparkSQLCLIDriver --conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark.driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None,
    )

    emr_spark_sql

```

{#6b050368960sk}

### Submit an SQL file from OSS {#757913ad5f9ks}

Run the SQL script file obtained from OSS. {#92a6fa7b642ow}

```python
from __future__ import annotations

from datetime import datetime

from airflow.models.dag import DAG
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
        spark_submit_parameters="--class org.apache.spark.sql.hive.thriftserver.SparkSQLCLIDriver --conf spark.executor.cores=4 --conf spark.executor.memory=20g --conf spark.driver.cores=4 --conf spark.driver.memory=8g --conf spark.executor.instances=1",
        is_prod=True,
        engine_release_version=None
    )

    emr_spark_sql_2

```

{#6f412a0bd3cx4}

### Submit a Python script from OSS {#3d1afe9bc7i2p}

Run the Python script file obtained from OSS. {#6b461b217bpwe}

```python
from __future__ import annotations

from datetime import datetime

from airflow.models.dag import DAG
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

{#49144e9c19iyg}

The following table describes the parameters: {#b7678149c52cu}
{#900c45141fopr}{#a9e2e4b6a10x6}{#51757f97f8qia}{#ad3191ee7f57b}{#f426811990kd9}{#dba60928441n2}{#8735ae32behwu}{#ae8dbb0ff8mwe}{#906d0a9b040ms}{#5c2eecc9e3yh6}{#5b464bbc1b6jo}{#72b950edc0ern}{#77cb7342ccpv4}{#122224409fh3m}

|--------------------------------|----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Parameter**                  | **Type** | **Description**                                                                                                                                                                                                                                                                   |
| `task_id`                      | `str`    | The unique identifier of the Airflow task.                                                                                                                                                                                                                                        |
| `emr_serverless_spark_conn_id` | `str`    | The ID of the connection between Airflow and EMR Serverless Spark.                                                                                                                                                                                                                |
| `region`                       | `str`    | The region in which the EMR Spark job is created.                                                                                                                                                                                                                                 |
| `polling_interval`             | `int`    | The interval at which Airflow queries the state of the job. Unit: seconds.                                                                                                                                                                                                        |
| `workspace_id`                 | `str`    | The unique identifier of the workspace to which the EMR Spark job belongs.                                                                                                                                                                                                        |
| `resource_queue_id`            | `str`    | The ID of the resource queue used by the EMR Spark job.                                                                                                                                                                                                                           |
| `code_type`                    | `str`    | The job type. SQL, Python, and JAR jobs are supported. The meaning of the entry_point parameter varies based on the job type.                                                                                                                                                     |
| `name`                         | `str`    | The name of the EMR Spark job.                                                                                                                                                                                                                                                    |
| `entry_point`                  | `str`    | The location of the file that is used to start the job. JAR, SQL, and Python files are supported. The meaning of this parameter varies based on `code_type`.                                                                                                                      |
| `entry_point_args`             | `List`   | The parameters that are passed to the Spark application.                                                                                                                                                                                                                          |
| `spark_submit_parameters`      | `str`    | The additional parameters used for the `spark-submit` command.                                                                                                                                                                                                                    |
| `is_prod`                      | `bool`   | The environment in which the job runs. If this parameter is set to True, the job runs in the production environment. In this case, the `resource_queue_id` parameter must be set to the ID of the corresponding resource queue in the production environment, such as root_queue. |
| `engine_release_version`       | `str`    | The version of the EMR Spark engine. Default value: esr-2.1-native, which indicates an engine that runs Spark 3.3.1 and Scala 2.12 in the native runtime.                                                                                                                         |

{#5e374c61eesfa}
