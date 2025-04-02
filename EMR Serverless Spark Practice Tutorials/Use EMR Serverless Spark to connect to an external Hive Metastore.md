E-MapReduce (EMR) Serverless Spark allows you to connect to an external Hive Metastore. This way, you can access the data that is stored in the Hive Metastore with ease. This topic describes how to configure settings in EMR Serverless Spark to connect to an external Hive Metastore to implement efficient management and utilization of data resources in a work environment.

## Prerequisites
A workspace and an SQL compute are created. For more information, see Create a workspace and Manage SQL computes.

## Limits
- To use the Hive Metastore service, you must restart the existing compute service in your workspace.

- After you specify a Hive Metastore as a default catalog, your workflow tasks automatically depend on the Hive Metastore.

## Procedure
### Step 1: Prepare the Hive Metastore service
> **Note** In this example, the Hive Metastore that is deployed in EMR on ECS is used as an external service. If a Hive Metastore has been deployed in your virtual private cloud (VPC), skip this step.

1. On the EMR on ECS page, create a DataLake cluster that contains the Hive service and for which the **Metadata** parameter is set to **Built-in MySQL**. For more information, see Create a cluster.

2. Log on to the master node of the DataLake cluster in SSH mode. For more information, see Log on to a cluster.

3. Run the following command to open the Hive CLI:

```html
hive
```

4. Run the following commands to create a table named dw_users that points to Object Storage Service (OSS) and write data to the table:

```html
 CREATE TABLE `dw_users`(
  `name` string)
LOCATION
  'oss://<yourBucket>/path/to/file';

INSERT INTO dw_users select 'Bob';
```

### Step 2: Create a network connection

1. Go to the Network Connection page.

  a. Log on to the EMR console.

  b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

  c. On the **Spark** page, click the name of the desired workspace.

  d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose Admin > **Network Connection**.

2. On the **Network Connection** page, click **Create Network Connection**.

3. In the **Create Network Connection** dialog box, configure parameters and click **OK**. The following table describes the parameters.


| **Parameter** | **Description** |
| --- | --- |
| **Name** | The name of the network connection. |
| **VPC** | The VPC in which your EMR cluster resides. |
| **vSwitch** | The vSwitch that is deployed in the VPC in which the EMR cluster is deployed. |

If **Succeeded** is displayed in the **Status** column of the connection, the network connection is created.

![image](https://github.com/user-attachments/assets/6356f0e4-cb1d-40b1-832e-8793bcd32ef8)

### Step 3: Enable the port of the Hive Metastore
1. Obtain the CIDR block of the vSwitch that you specified when you created the network connection.

You can log on to the VPC console and obtain the CIDR block of a desired vSwitch on the **vSwitch** page.
![image](https://github.com/user-attachments/assets/f42108ab-1a0f-4cca-a65e-d08a0798a06c)

2. Configure security group rules.

a. Log on to the EMR console.

b. On the **EMR** on **ECS** page, find the desired cluster and click the cluster name in the Cluster ID/Name column.

c. In the Security section of the **Basic Information**, click the link to the right of **Cluster Security Group**.

d. On the **Security Group Details** tab, click **Add Rule**, configure the **Port Range** and **Authorization Object** parameters, and then click **Save**.
| **Parameter** | **Description** |
| --- | --- |
| **Port Range** | The port number. Enter 9083. |
| **Authorization Object** | The CIDR block of the vSwitch obtained in the previous step. 
> **Important** To prevent attacks from external users, we recommend not setting the Authorization Object parameter to 0.0.0.0/0.

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/connect-emr-serverless-spark-to-the-hive-metastore-service?utm_content=g_1000402732)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

