Apache Hudi is a data lake framework that allows you to update and delete data in Hadoop compatible file systems. Hudi also allows you to consume changed data. For more information, see Apache Hudi. This topic describes how to read data from and write data to a Hudi table in EMR Serverless Spark.

## Prerequisites
A workspace is created. For more information, see Create a Workspace.

## Procedure
### Step 1: Create an SQL session
1. Go to the Sessions page.
   
   a. Log on to the EMR console.

   b. In the left-side navigation pane, choose EMR Serverless > Spark.

   c. On the Spark page, click the name of the workspace that you want to manage.

   d. In the left-side navigation pane of the EMR Serverless Spark page, choose Operation Center > Sessions.

2. On the **SQL Sessions** tab, click **Create SQL Session**.

3. In the **Spark Configuration** section of the **Create SQL Session** page, add the following code and click **Create**. For more information, see Manage SQL sessions.

In the following code, the default catalog of the workspace is used. For information about how to use an external Hive Metastore as a catalog, see Use EMR Serverless Spark to connect to an external Hive Metastore.

```html
   spark.sql.extensions             org.apache.spark.sql.hudi.HoodieSparkSessionExtension
spark.sql.catalog.spark_catalog  org.apache.spark.sql.hudi.catalog.HoodieCatalog
spark.serializer                 org.apache.spark.serializer.KryoSerializer
```
### Step 2: Read data from and write data to a Hudi table
1. Go to the data development page of EMR Serverless Spark.

In the left-side navigation pane of the **EMR Serverless Spark** page, click **Data Development**.

2. On the **Development** tab, click the ```+``` icon.

3. In the **Create** dialog box, set the Name parameter to users_task, use the default value SparkSQL for the Type parameter, and then click **OK**.

4. On the users_task tab, copy the following code to the code editor:

```html
CREATE DATABASE IF NOT EXISTS ss_hudi_db;

CREATE TABLE ss_hudi_db.hudi_tbl (id INT, name STRING) USING hudi TBLPROPERTIES (
  type = 'cow',
  primaryKey = 'id'
);

INSERT INTO ss_hudi_db.hudi_tbl VALUES (1, "a"), (2, "b");

SELECT id, name FROM ss_hudi_db.hudi_tbl ORDER BY id;

DROP TABLE ss_hudi_db.hudi_tbl;

DROP DATABASE ss_hudi_db;
```

5. Select a database from the Default Database drop-down list and the created SQL session from the SQL Sessions drop-down list.

6. Click **Run**. The following figure shows the output.
![image](https://github.com/user-attachments/assets/42216ad1-9000-4afd-89d0-67551975ba70)

## References
- For information about how to develop and orchestrate SQL jobs, see Get started with the development of Spark SQL jobs.

- For more information about Hudi, see Apache Hudi.

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/hudi-data-source?utm_content=g_1000402734)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

