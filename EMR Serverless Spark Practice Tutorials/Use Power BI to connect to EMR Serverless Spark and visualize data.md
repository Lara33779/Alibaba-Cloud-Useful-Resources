Power BI is a centralized, extendable self-service and enterprise business intelligence (BI) platform. You can use Power BI to connect to data sources for data modeling, visualized analysis, and personalized reporting. This topic describes how to connect to E-MapReduce (EMR) Serverless Spark to visualize and analyze data.

## Limits
You cannot access the data catalogs or tables of Paimon and Iceberg.

## Prerequisites
- A Power BI client is downloaded and installed. For more information, see Microsoft Power BI.
 
- A Spark Thrift Server and its token are created. For more information, see the Create a Spark Thrift Server and Create a token sections in the "Manage Spark Thrift Servers" topic.

## (Optional) Prepare test data
In this example, the user_behavior table is used. You can skip the operations in this section if test data is prepared.

1. Download the test data file named user.csv and upload the file to Alibaba Cloud Object Storage Service (OSS).

In this topic, the test data file is upload to ```oss://emr-oss-hdfs/spark/user_behavior/user.csv.``` For information about how to upload a file, see Upload objects.

2. Create an SQL session.

  a. Go to the Sessions page.
  
 ⅰ. Log on to the EMR console.

 ⅱ. In the left-side navigation pane, choose **EMR Serverless > Spark**.

 ⅲ. On the **Spark** page, click the name of the workspace that you want to manage.
  
 ⅴ. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

  b. On the **SQL Sessions** tab, click **Create SQL Session**.

  c. On the **Create SQL Session** page, configure the parameters based on your business requirements and click **Create**.

For more information, see Manage SQL sessions.

3. Create an SQL job.

a. In the left-side navigation pane of the **EMR Serverless Spark** page, click **Data Development**.

b. On the **Development** tab, click **Create**.

c. In the **Create** dialog box, set the Name parameter to user_behavior, use the default value SparkSQL for the Type parameter, and then click **OK**.

d. On the user_behavior tab, copy and paste the following code to the code editor:

```html
CREATE TABLE user_behavior (
    user_id INT,
    item_id INT,
    behavior_type INT,
    user_geohash STRING,
    item_category INT,
    time STRING
)USING CSV
  Location 'oss://emr-oss-hdfs/spark/user_behavior';
   
  SELECT * FROM user_behavior limit 1000;
```

e. Select a database from the **Default Database** drop-down list and a started SQL session from the **SQL Sessions** drop-down list. In this example, the database named default is selected.

f. Click **Run**.

![image](https://github.com/user-attachments/assets/f4670f03-ec8c-42fc-97fe-471c9282dcf4)

## Use Power BI to connect to EMR Serverless Spark
This section describes how to connect to EMR Serverless Spark by using different methods. You can select a method based on your business requirements. If you use Open Database Connectivity (ODBC) to connect to EMR Serverless Spark, you must install and configure ODBC.

### Use ODBC to connect to EMR Serverless Spark
1. Download and install the Microsoft Spark ODBC driver.

2. Configure the ODBC driver.

   a. Open the ODBC driver. On the **User DSN** tab, click **Add**.
   
   b. In the **Create New Data Source** dialog box, select **Microsoft Spark ODBC Driver** and click **Finish**.
   
   c. In the **Microsoft Spark ODBC Driver DSN Setup** dialog box, configure the required parameters that are described in the following table and use the default values for other parameters.

   | **Parameter** | **Description** |
   | --- | --- |
   | **Date Source Name** | The name of the data source. Example: serverless_spark_test. |
   | **Host(s)** | The **internal endpoint** or the **public endpoint** of the Spark Thrift Server that you created. ![image](https://github.com/user-attachments/assets/196b97bd-406a-45ba-8ea7-63aa77c2326d) |
   | **Mechanism** | The authentication method. Select **User Name and Password**. |
   | **User Name** | The name of the token that you created on the **Tokens** tab of the Spark Thrift Server. |
   | **Password** | The token that you copied from the **Tokens** tab of the Spark Thrift Server. |
   
   d. Click **HTTP Options**. In the dialog box that appears, enter ```/cliservice``` in the **HTTP Path** field and click **OK**.

   ![image](https://github.com/user-attachments/assets/4c8e23ec-dcd5-4664-875b-dd65f3321a0b)

   e. Optional. Click **Advanced Options**. In the dialog box that appears, select **Get Tables With Query** and click **OK**.

   > **Note** If Paimon or Iceberg tables exist in the default catalog, we recommend that you perform this operation.
   
![image](https://github.com/user-attachments/assets/3adf05f3-206d-402f-bad6-3948c8fddc75)

f. In the **Microsoft Spark ODBC Driver DSN Setup** dialog box, click **Test** to test the connectivity.

If the returned information contains SUCCESS, the ODBC driver is connected to the specific Serverless Spark Thrift Server.
![image](https://github.com/user-attachments/assets/457e167d-0c8d-4c07-8933-e15318e5d6d1)

g. In the Microsoft Spark ODBC Driver DSN Setup dialog box, click OK to save the DSN configurations.

3. Connect to EMR Serverless Spark.
   
a. On the **Home** tab, click **Get data from other sources** in the **Select a data source or start with a blank report** section.
   
b. On the **All** tab of the **Get Data** dialog box, enter **ODBC** in the search box, click ODBC, and then **click** Connect.

   ![image](https://github.com/user-attachments/assets/446b9744-c5fc-4e61-ab06-e6b8556bbcdb)

c. In the dialog box that appears, select the serverless_spark_test data source form the drop-down list and click **OK**.

   ![image](https://github.com/user-attachments/assets/a91fcb49-8d9f-4d06-8e46-f805fabbf1c4)

d. On the **Windows** tab of the dialog box that appears, select **Use my current credentials** and click **Connect**.

4. In the **Navigator** dialog box, select the table that you want to import and click **Load**.

![image](https://github.com/user-attachments/assets/7eb1c1a8-2d0d-4bf5-bb8d-842a2edd125f)


For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-power-bi-to-connect-to-emr-serverless-spark-and-visualize-data?utm_content=g_1000402736)   

   # Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>


