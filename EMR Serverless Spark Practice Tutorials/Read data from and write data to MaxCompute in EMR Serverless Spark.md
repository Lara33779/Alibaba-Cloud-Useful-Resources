E-MapReduce (EMR) Serverless Spark provides built-in MaxCompute data sources based on the Spark DataSource V2 API. If you want to connect a job to MaxCompute, you need to only add specific configurations when you develop the job. This topic describes how to read data from and write data to MaxCompute in EMR Serverless Spark.

# Background information
MaxCompute, formerly known as ODPS, is a fast and fully managed computing platform for large-scale data warehousing. MaxCompute can process up to exabytes of data. MaxCompute supports batch computing and storage of structured data and provides various data warehousing solutions and big data analysis and modeling services. For more information about MaxCompute, see What is MaxCompute?
## Prerequisites
- An EMR Serverless Spark workspace is created. For more information, see Create a workspace.
- A MaxCompute project is created. For more information, see Create a MaxCompute project.
## Limits
The Spark version that is included in the engine version of a session must be 3.3.x. Example: esr-2.4.1 (Spark 3.3.1, Scala 2.12).
## Procedure
### Step 1: Create a session to connect to the MaxCompute project
You can create an SQL session or a notebook session to connect to the MaxCompute project. For more information about sessions, see Manage sessions.
Create an SQL session to connect to the MaxCompute project
1. Go to the Sessions page.

    a. Log on to the EMR console.

    b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

    c. On the **Spark** page, click the name of the workspace that you want to manage.

    d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

2. On the **SQL Sessions** tab, click **Create SQL Session**.

3. On the Create SQL Session page, configure the parameters and click **Create**. The following table describes the parameters.

| Parameter | Description |
| --- | --- |
| Name | The name of the SQL session. In this example, the SQL session is named mc_sql_compute. |
| Spark Configuration | The Spark configurations that are used to connect to the MaxCompute project. |

```
spark.sql.catalog.odps                        org.apache.spark.sql.execution.datasources.v2.odps.OdpsTableCatalog
spark.sql.extensions                          org.apache.spark.sql.execution.datasources.v2.odps.extension.OdpsExtensions
spark.sql.catalog.odps.enableNamespaceSchema  true
spark.sql.sources.partitionOverwriteMode      dynamic
spark.hadoop.odps.project.name                <project_name>
spark.hadoop.odps.end.point                   http://service.cn-hangzhou-vpc.maxcompute.aliyun-inc.com/api
spark.hadoop.odps.access.id                   <accessId>
spark.hadoop.odps.access.key                  <accessKey>
```

| Spark Configuration | Configure the following parameters based on your business requirements: |
- ```<project_name>```: the name of the MaxCompute project.
- ```http://service.cn-hangzhou-vpc.maxcompute.aliyun-inc.com/api```: the endpoint of the MaxCompute project. For more information, see Endpoints.
- ```<accessId>```: the AccessKey ID of the Alibaba Cloud account that is used to access MaxCompute.
- ```<accessKey>```: the AccessKey secret of the Alibaba Cloud account that is used to access MaxCompute.

Create a notebook session to connect to the MaxCompute project

1. Go to the Notebook Sessions tab.

  a. Log on to the EMR console.

  b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

  c. On the **Spark** page, find the desired workspace and click the name of the workspace.

  d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

  e. Click the **Notebook Sessions** tab.

2. Click Create **Notebook Session**.

3. On the Create Notebook Session page, configure the parameters and click **Create**. The following table describes the parameters.

| **Parameter** | **Description** |
| **Name** | The name of the notebook session. In this example, the notebook session is named mc_notebook_compute. |
| **Spark Configuration** | The Spark configurations that are used to connect to the MaxCompute project. |

```
spark.sql.catalog.odps                        org.apache.spark.sql.execution.datasources.v2.odps.OdpsTableCatalog
spark.sql.extensions                          org.apache.spark.sql.execution.datasources.v2.odps.extension.OdpsExtensions
spark.sql.catalog.odps.enableNamespaceSchema  true
spark.sql.sources.partitionOverwriteMode      dynamic
spark.hadoop.odps.project.name                <project_name>
spark.hadoop.odps.end.point                   http://service.cn-hangzhou-vpc.maxcompute.aliyun-inc.com/api
spark.hadoop.odps.access.id                   <accessId>
spark.hadoop.odps.access.key                  <accessKey>
```
Configure the following parameters based on your business requirements:
- ```<project_name>```: the name of the MaxCompute project.
- ```http://service.cn-hangzhou-vpc.maxcompute.aliyun-inc.com/api```: the endpoint of the MaxCompute project. For more information, see Endpoints.
- ```<accessId>```: the AccessKey ID of the Alibaba Cloud account that is used to access MaxCompute.
- ```<accessKey>```: the AccessKey secret of the Alibaba Cloud account that is used to access MaxCompute.

### Step 2: Query data from or write data to the MaxCompute project
For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:

👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/implement-starrocks-read-and-write-operations-in-emr-serverless-spark?utm_content=g_1000402688)

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

