Implement Doris read and write operations in EMR Serverless Spark
===========================================================================
Leveraging the official Spark Connector from Apache Doris, EMR Serverless Spark can be configured to connect to Doris during development. This topic explains how to execute data read and write operations with Doris within the EMR Serverless Spark environment.

**Background information**
--------------------------------
Apache Doris is a high-performance, real-time analytics database suitable for report analysis, ad hoc queries, and data lake federated query acceleration. For more information, see Introduction to Apache Doris.

EMR Serverless Spark is a high-performance Lakehouse product compatible with open-source Spark, offering fully managed enterprise-level data platform services. Integrating Apache Doris with EMR Serverless Spark enables efficient data read, write, and analysis operations, facilitating a complete data processing workflow.

**Prerequisites**
----------------------------------

* A Serverless Spark workspace has been created. For details, see Create a workspace.

* A Doris cluster has been created.

If you have created a data analysis (OLAP) cluster with Doris services in EMR on ECS, see Create a cluster. This topic assumes the creation of a cluster with Doris services in EMR on ECS, hereafter referred to as the EMR Doris cluster.

**Limits**
The Serverless Spark engine must be version esr-2.5.0, esr-3.1.0, esr-4.1.0, or later.

**Usage notes**
--------------------------------

You cannot call the EmrServerlessSparkStartJobRunOperator operation to query job logs. If you want to view job logs, you must go to the EMR Serverless Spark page and find the job run whose logs you want to view by job run ID. Then, you can check and analyze the job logs on the **Logs** tab of the job details page or on the Spark Jobs page in the **Spark UI**.

**Operation flow**
------------------------------

### **Step 1:** Obtain the Doris Spark Connector JAR and upload it to OSS

Refer to the official Doris documentation for the Spark Doris Connector Spark Doris Connector, which typically includes compatibility information for different connector versions with various Spark engine versions. Confirm the compatibility between the Spark version in use and the Doris Spark Connector version.

1. Go to the GitHub repository for the Doris Spark Connector and download the appropriate version.

The Doris Spark Connector JAR package is named according to the format spark-doris-connector-spark-${spark_version}-${connector_version}.jar. For instance, if the engine version is esr-3.1.0 (Spark 3.4.3, Scala 2.12), you would download spark-doris-connector-spark-3.4-24.0.0.jar.

2. Upload the downloaded Spark Connector JAR to Alibaba Cloud OSS. For the upload process, see Simple upload.

Step 2: Create network connectivity

### **Step 2:** Create network connectivity
   
Serverless Spark must establish network connectivity with the EMR Doris cluster to access Doris services normally. For network connectivity information, see Network connectivity between EMR Serverless Spark and other VPCs.

Important
When configuring security group rules, selectively open only the necessary ports according to your needs in the Port Range. The range for ports is 1~65535. This example requires opening the HTTP port (8031), RPC port (9061), and Webserver port (8041).

### **Step 3:** Create a database and table in the EMR Doris cluster

1. Log in to the cluster using SSH. For details, see Log on to the cluster.

2. Execute the following command to connect to the EMR Doris cluster.

   ```bash
   mysql -h127.0.0.1 -P 9031 -uroot

3. Create a database and table.

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/implement-doris-read-and-write-operations-in-emr-serverless-spark?utm_content=g_1000402564)                                                                                                     
