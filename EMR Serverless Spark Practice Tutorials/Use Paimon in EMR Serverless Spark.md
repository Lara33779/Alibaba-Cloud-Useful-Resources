Apache Paimon is a data lake format that allows you to process data in streaming and batch modes. Apache Paimon supports high-throughput data write and low-latency data queries. For more information, see Apache Paimon. This topic describes how to read data from and write data to a Paimon table in E-MapReduce (EMR) Serverless Spark.

## Prerequisites
A workspace is created. For more information, see Create a workspace.

## Procedure
### Step 1: Create an SQL session
1. Go to the Sessions page.

  a. Log on to the EMR console.

  b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

  c. On the **Spark** page, click the name of the workspace that you want to manage.

  d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

2. On the **SQL Sessions** tab, click **Create SQL Session**.

3. On the **Create SQL Session** page, add the following code in the **Spark Configuration** section based on the catalog that you use and click Create. For more information, see Manage SQL sessions.

Catalogs are required when you read data from or write data to Paimon in EMR Serverless Spark. You can specify a catalog based on your business requirements.

- Catalog types

| **Type** | **Description** |
| --- | --- |
| Paimon Catalog | The catalog used to manage metadata in the Paimon format. You can use Paimon catalogs only to query data from and write data to Paimon tables. |

- Data Lake Formation (DLF) 1.0 catalogs, Hive Metastore catalogs, and file system catalogs are supported. You can specify a catalog based on your business requirements.

- To access a Paimon table, you must specify the table name in the ```<catalogName>.<Database name>.<Table name>``` format.

> **Important** <catalogName> specifies the name of the catalog. You can specify a catalog name based on your business requirements. We recommend that you use the default catalog name paimon.

| spark_catalog | The default catalog of Spark, which can be used to manage the metadata of Spark SQL internal tables and query data from and write data to Paimon tables or non-Paimon tables. |
| --- | --- | 

- The default catalog of a workspace is used.

For information about how to use an external Hive Metastore as a catalog, see Use EMR Serverless Spark to connect to an external Hive Metastore.

- The default catalog of a workspace is used.

For information about how to use an external Hive Metastore as a catalog, see Use EMR Serverless Spark to connect to an external Hive Metastore.

- To access a Paimon table or a non-Paimon table, you must specify the table name in the <Database name>.<Table name> format.

- Catalog configuration

  - Use a Paimon catalog

# DLF 1.0
Metadata is stored in DLF 1.0.

```html
spark.sql.extensions                            org.apache.paimon.spark.extensions.PaimonSparkSessionExtensions
spark.sql.catalog.<catalogName>                 org.apache.paimon.spark.SparkCatalog
spark.sql.catalog.<catalogName>.metastore       dlf
```
- Use a Spark catalog

```html
spark.sql.extensions               org.apache.paimon.spark.extensions.PaimonSparkSessionExtensions
spark.sql.catalog.spark_catalog    org.apache.paimon.spark.SparkGenericCatalog
```

## Step 2: Read data from and write data to tables based on Paimon and Spark catalogs
1. Go to the data development page of EMR Serverless Spark.

In the left-side navigation pane of the **EMR Serverless Spark** page, click **Data Development**.

2. On the **Development** tab, click the image icon.

3. In the **Create** dialog box, set the Name parameter to users_task, use the default value SparkSQL for the Type parameter, and then click **OK**.

4. On the users_task tab, copy the following code to the code editor.

Use a Paimon catalog
```html
-- Create a database.
CREATE DATABASE IF NOT EXISTS paimon.ss_paimon_db;             

-- Create a Paimon table.
CREATE TABLE paimon.ss_paimon_db.paimon_tbl (id INT, name STRING) USING paimon;

-- Write data to the Paimon table.
INSERT INTO paimon.ss_paimon_db.paimon_tbl VALUES (1, "a"), (2, "b");

-- Query data from the Paimon table.
SELECT * FROM paimon.ss_paimon_db.paimon_tbl ORDER BY id;

-- Drop the database.
DROP DATABASE paimon.ss_paimon_db CASCADE;
```
5. Select a database from the Default Database drop-down list and the created SQL session from the SQL Sessions drop-down list.

6. Click **Run**. The following figure shows the output.

![image](https://github.com/user-attachments/assets/d95c187d-0c11-4ef4-9065-ad9e27a7071b)

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/paimon-data-source?utm_content=g_1000402733)   

