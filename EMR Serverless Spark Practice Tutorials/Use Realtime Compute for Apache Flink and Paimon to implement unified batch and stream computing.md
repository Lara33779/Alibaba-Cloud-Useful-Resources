This topic describes how to use Realtime Compute for Apache Flink and E-MapReduce (EMR) Serverless Spark to build a Paimon data lake analytics process. The process allows you to write data to Object Storage Service (OSS), perform interactive queries, and compact offline data. EMR Serverless Spark is fully compatible with Paimon. EMR Serverless Spark integrates Data Lake Formation (DLF), which enables metadata sharing with other cloud services, such as Realtime Compute for Apache Flink. This helps formulate a solution for unified batch and stream computing. You can run jobs and configure job parameters in a flexible manner in EMR Serverless Spark to meet various requirements for real-time analysis and job scheduling.

# Background information
## Realtime Compute for Apache Flink
Alibaba Cloud Realtime Compute for Apache Flink is a fully managed, out-of-the-box serverless Flink service that supports end-to-end development, O&M, and management. Realtime Compute for Apache Flink supports multiple billing methods. Realtime Compute for Apache Flink also delivers powerful capabilities for entire project lifecycles, including draft development, data debugging, operation and monitoring, automatic tuning, and intelligent diagnostics. For more information, see What is Alibaba Cloud Realtime Compute for Apache Flink?

## Apache Paimon
Apache Paimon is a unified data lake format. Apache Paimon can work together with Apache Flink and Apache Spark to build a real-time lakehouse architecture that supports unified batch and stream computing. Apache Paimon innovatively combines the lake format with the log-structured merge-tree (LSM) technology to support real-time stream updates and stream computing. For more information, see Apache Paimon.

# Procedure
## Step 1: Create a Paimon catalog in Realtime Compute for Apache Flink
Apache Paimon catalogs can be used to manage all Apache Paimon tables in the same warehouse directory in an efficient manner. Apache Paimon catalogs can also be used by other Alibaba Cloud services. For information about how to create and use an Apache Paimon catalog, see Manage Apache Paimon catalogs.
1. Log on to the management console of Realtime Compute for Apache Flink.
2. Find the desired workspace and click Console in the Actions column.
3. Create an Apache Paimon catalog.

   a. In the left-side navigation pane, choose Development > Scripts.

   b. On the Scripts tab, click the image.png icon to create a script.

   c. Enter the SQL code in the script editor.

   Sample code:

```html
   CREATE CATALOG `paimon` WITH (
  'type' = 'paimon',
  'metastore' = 'dlf', | 
  'warehouse' = '<warehouse>',
  'dlf.catalog.id' = '<dlf.catalog.id>',
  'dlf.catalog.accessKeyId' = '<dlf.catalog.accessKeyId>',
  'dlf.catalog.accessKeySecret' = '<dlf.catalog.accessKeySecret>',
  'dlf.catalog.endpoint' = '<dlf.catalog.endpoint>',
  'dlf.catalog.region' = '<dlf.catalog.region>',
);
```

| Parameter | Description | Required | Remarks |
| --- | --- | --- | --- |
| paimon | The name of the Apache Paimon catalog. |	Yes | Enter a custom name. |
| type | The type of the catalog. | Yes | Set the value to paimon. |
| metastore | The metadata storage type. | Yes | Set the value to dlf. You can use DLF to manage metadata in a centralized manner and implement seamless integration among engines. |
| warehouse | The data warehouse directory. | Yes | Configure this parameter based on your business requirements. |
| dlf.catalog.id | The ID of the DLF data catalog. | Yes | You can view the ID of the data catalog in the DLF console. |
| dlf.catalog.accessKeyId | The AccessKey ID that is used to access DLF. | Yes | For information about how to obtain the AccessKey pair, see Create an AccessKey pair. |
| dlf.catalog.accessKeySecret | The AccessKey secret that is used to access DLF. | Yes | For information about how to obtain the AccessKey pair, see Create an AccessKey pair. |
| dlf.catalog.endpoint | The endpoint of DLF. | Yes | For more information, see Supported regions and endpoints. > **Note** If DLF resides in the same region as Realtime Compute for Apache Flink, the VPC endpoint is used. Otherwise, the public endpoint is used. |
| dlf.catalog.region | The region in which DLF resides. | Yes | For more information, see Supported regions and endpoints. > **Note** Make sure that the value of this parameter matches the endpoint that is specified by the dlf.catalog.endpoint parameter. |

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-realtime-compute-for-apache-flink-and-paimon-to-process-batch-data-and-streaming-data-in-a-unified-manner?utm_content=g_1000402647)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR?utm_content=g_1000403124"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

