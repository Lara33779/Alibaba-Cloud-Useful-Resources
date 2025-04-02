# Use EMR Serverless Spark to submit a PySpark streaming job

With the rapid development of big data, the stream processing technology has become essential for real-time data analysis. E-MapReduce (EMR) Serverless Spark is a powerful and scalable platform that simplifies real-time data processing and saves the trouble of managing servers, thereby improving efficiency. This topic describes how to use EMR Serverless Spark to submit a PySpark streaming job. You can understand the usability and maintainability of EMR Serverless Spark in stream processing based on this topic.

## Prerequisites

An EMR Serverless Spark workspace is created. For more information, see [Create a workspace](https://www.alibabacloud.com/help/en/emr/emr-serverless-spark/getting-started/create-a-workspace?spm=a2c63.p38356.0.i21).

## Procedure

### Step 1: Create a Dataflow cluster and generate messages

1. Log on to the EMR console and create a Dataflow cluster that contains the Kafka service on the EMR on ECS page. For more information, see [Create a cluster](https://www.alibabacloud.com/help/en/emr/emr-on-ecs/user-guide/create-a-cluster-on-the-emr-on-ecs-page?spm=a2c63.p38356.0.i27).
2. Log on to the master node of the EMR cluster. For more information, see [Log on to a cluster](https://www.alibabacloud.com/help/en/emr/emr-on-ecs/user-guide/log-on-to-an-emr-cluster?spm=a2c63.p38356.0.i30).
3. Run the following command to switch the directory:
   
```bash
cd /var/log/emr/taihao_exporter
```

4. Run the following command to create a topic:

```bash
# Create a topic named taihaometrics, with 10 partitions and a replica factor of 2. 
kafka-topics.sh --partitions 10 --replication-factor 2 --bootstrap-server core-1-1:9092 --topic taihaometrics --create
```

5. Run the following command to send messages:

```
# Use the kafka-console-producer CLI to send messages to the taihaometrics topic. 
tail -f metrics.log | kafka-console-producer.sh --broker-list core-1-1:9092 --topic taihaometrics
```
### Step 2: Create a network connection
1. Go to the Network Connections page.

   a. In the left-side navigation pane of the EMR console, choose **EMR Serverless > Spark**.

   b. On the **Spark** page, find the desired workspace and click the name of the workspace.

   c. In the left-side navigation pane of the **EMR Serverless Spark** page, click **Network Connections**.

2. On the **Network Connections** page, click **Create Network Connection**.

3.In the **Create Network Connection** dialog box, configure parameters and click **OK**. The following table provides details:

| Parameter | Description |
| --- | --- |
| Name | The name of the network connection. Example: connection_to_emr_kafka. |
| VPC | The virtual private cloud (VPC) in which your EMR cluster resides. If no VPC is available, click Create VPC to create a VPC in the VPC console. For more information, see Create and manage a VPC. |
| vSwitch | A vSwitch that is deployed in the VPC in which the EMR cluster is deployed. |

If Succeeded is displayed in the Status column of the connection, the network connection is created.

### Step 3: Configure security group rules for the EMR cluster
1. Obtain the CIDR block of the vSwitch to which a cluster node is connected.
On the **Nodes** tab, click the name of a node group to view the associated vSwitch. Then, log on to the VPC console and obtain the CIDR block of the vSwitch on the **vSwitch** page.
![image](https://github.com/user-attachments/assets/d4e45125-2a0f-4422-b411-8e5d3d29a878)
2. Configure security group rules.
  a. On the **EMR** on **ECS** page, find the desired cluster and click the name of the cluster.
  b.In the Security section of the **Basic Information** tab, click the link to the right of **Cluster Security Group**.
  c. On the Security Group Details page, click Add Rule, configure the Port Range and Authorization Object parameters, and then click **Save** in the Actions column.

| Parameter | Description |
| --- | --- |
| Port Range | The port number. In this example, enter 9092. |
| Authorization Object | The CIDR block of the vSwitch obtained in the previous step. Important To prevent attacks from external users, we recommend that you do not set the Authorization Object parameter to 0.0.0.0/0.|

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/submit-a-pyspark-stream-task-through-serverless-spark?spm=a2c63.p38356.help-menu-28066.d_3_2.b5956f9b1ndoxd?utm_content=g_1000402584)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

