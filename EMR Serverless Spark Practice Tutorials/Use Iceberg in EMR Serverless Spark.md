Iceberg is an open table format for data lakes. You can use Iceberg to quickly build your own data lake storage service on Hadoop Distributed File System (HDFS) or Alibaba Cloud Object Storage Service (OSS). This topic describes how to read data from and write data to an Iceberg table in EMR Serverless Spark.

## Prerequisites
A workspace is created. For more information, see Create a Workspace.

## Procedure
> **Note** You can use only a Spark SQL job or a notebook to read data from and write data to Iceberg tables. In this topic, a Spark SQL job is used.

## Step 1: Create a session
1. Go to the Sessions page.

  a. Log on to the EMR console.

  b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

  c. On the **Spark** page, click the name of the workspace that you want to manage.

  d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

2. On the **SQL Sessions** tab, click **Create SQL Session**.

3. On the **Create SQL Session** page, add the following code in the **Spark Configuration** section based on the catalog that you use and click **Create**. For more information, see Manage SQL sessions.

Catalogs are required when you read data from or write data to Iceberg in EMR Serverless Spark. You can specify a catalog based on your business requirements.

- Catalog types

| **Type** | **Description** |
| --- | --- |
| Iceberg catalog | The catalog used to manage metadata in the Iceberg format. You can use Iceberg catalogs only to query data from and write data to Iceberg tables. |
- Data Lake Formation (DLF) 1.0 catalogs, Hive Metastore catalogs, and file system catalogs are supported. You can specify a catalog based on your business requirements.
- To access an Iceberg table, you must specify the table name in the ```<catalogName>.<Database name>.<Table name>``` format.
> **Important** ```<catalogName>``` specifies the name of the catalog. You can specify a catalog name based on your business requirements. We recommend that you use the default catalog name iceberg.
 
| **spark_catalog** | The default catalog of a workspace, which can be used to query data from Iceberg tables and non-Iceberg tables. |
| --- | --- |

- The default catalog of a workspace is used.
For information about how to use an external Hive Metastore as a catalog, see use EMR Serverless Spark to connect to an external Hive Metastore.
- To access an Iceberg table or a non-Iceberg table, you must specify the table name in the ```<Database name>.<Table name>``` format.

- Catalog configuration
  - Use an Iceberg catalog

DLF 1.0
Metadata is stored in DLF 1.0.

```html
spark.sql.extensions                         org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions
spark.sql.catalog.<catalogName>               org.apache.iceberg.spark.SparkCatalog
spark.sql.catalog.<catalogName>.catalog-impl  org.apache.iceberg.aliyun.dlf.hive.DlfCatalog
```
  - Use a Spark catalog

```html
spark.sql.extensions                     org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions
spark.sql.catalog.spark_catalog          org.apache.iceberg.spark.SparkSessionCatalog
```

4. Find the created session and click Start in the Actions column.

## Step 2: Read data from and write data to an Iceberg table
1. Go to the data development page of EMR Serverless Spark.

In the left-side navigation pane of the **EMR Serverless Spark** page, click **Data Development**.

2. On the **Development** tab, click the ```+``` icon.

3. In the **Create** dialog box, set the Name parameter to users_task, use the default value SparkSQL for the Type parameter, and then click **OK**.

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-iceberg-in-emr-serverless-spark?utm_content=g_1000402735)   



