This topic describes how to use the spark-submit CLI to submit a Spark job after E-MapReduce (EMR) Serverless Spark is connected to Elastic Compute Service (ECS).

# Prerequisites
- Java Development Kit (JDK) V1.8 or later is installed.
- If you want to use a RAM user to submit Spark jobs, make sure that the RAM user is added to a Serverless Spark workspace as a member and assigned the developer role or a role that has higher permissions. For more information, see Manage users and roles.
# Procedure
## Step 1: Download and install the spark-submit tool for EMR Serverless
1. Click emr-serverless-spark-tool-0.1.0-bin.zip to download the installation package.
2. Upload the installation package to an ECS instance. For more information, see Upload and download files.
3. Run the following command to decompress the installation package and install the spark-submit tool:
```html
unzip emr-serverless-spark-tool-0.1.0-bin.zip
```
## Step 2: Configure parameters
> **Important**
> If the SPARK_CONF_DIR environment variable is configured in the environment where the spark-submit tool is installed, you must store the configuration file in the directory specified by the SPARK_CONF_DIR environment variable. For example, for EMR clusters, the directory is /etc/taihao-apps/spark-conf in most cases. Otherwise, an error is reported.
1. Run the following command to modify the configuration of the connection.properties file:
```html
vim emr-serverless-spark-tool-0.1.0/conf/connection.properties
```
2. Configure parameters in the file based on the following sample code. The parameters are specified in the key=value format.
```html
accessKeyId=yourAccessKeyId
accessKeySecret=yourAccessKeySecret
# securityToken=yourSecurityToken
regionId=cn-hangzhou
endpoint=emr-serverless-spark.cn-hangzhou.aliyuncs.com
workspaceId=w-xxxxxxxxxxxx
resourceQueueId=dev_queue
# networkServiceId=xxxxxx
releaseVersion=esr-2.2 (Spark 3.3.1, Scala 2.12, Java Runtime)
```
The following table describes the parameters:

| Parameter | Required | Description |
| --- | --- | --- |
| accessKeyId | Yes | The AccessKey ID of the Alibaba Cloud account or RAM user that is used to run the Spark job. |
| accessKeySecret | Yes | The AccessKey secret of the Alibaba Cloud account or RAM user that is used to run the Spark job. |
| securityToken | No | The Security Token Service (STS) token of the RAM user. **Note** This parameter is required only for STS authentication. |
| regionId | Yes | The region ID. In this example, the China (Hangzhou) region is used. |
| endpoint | Yes | The endpoint of EMR Serverless Spark. For more information, see Endpoints. In this example, the public endpoint in the China (Hangzhou) region emr-serverless-spark.cn-hangzhou.aliyuncs.com is used. **Note** If the ECS instance cannot access the Internet, you must use the virtual private cloud (VPC) endpoint of EMR Serverless Spark.|
| workspaceId | Yes | The ID of the EMR Serverless Spark workspace. |
| resourceQueueId | No | 	The name of the queue. Default value: dev_queue. |
| networkServiceId | No | The name of the network connection. **Note** This parameter is required only if the Spark job needs to access VPC resources. For more information, see Configure network connectivity between EMR Serverless Spark and a data source across VPCs. |
| releaseVersion | No | The version of EMR Serverless Spark. Example: esr-2.2 (Spark 3.3.1, Scala 2.12, Java Runtime). |
