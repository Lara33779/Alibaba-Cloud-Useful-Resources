Apache Airflow is a powerful workflow automation and scheduling tool that allows developers to orchestrate, schedule, and monitor data pipelines. E-MapReduce (EMR) Serverless Spark provides a serverless computing environment for processing large-scale data processing jobs. This topic describes how to use the LivyOperator of Apache Airflow to automatically submit jobs to EMR Serverless Spark. This way, you can automate job scheduling and running to manage data processing jobs more efficiently.

# Background information
Apache Livy can interact with Spark over a REST API. This greatly simplifies the communication between Spark and application servers. For more information about Livy REST APIs, see REST API.

# Prerequisites
- Airflow is installed and started. For more information, see Installation of Airflow.
- A workspace is created. For more information, see Create a workspace.
# Procedure
## Step 1: Create a gateway and a token
1. Create and start a gateway.

  a. Go to the Gateways page.
  
     ⅰ. Log on to the EMR console.
     
     ⅱ. In the left-side navigation pane, choose **EMR Serverless > Spark**.
     
     ⅲ. On the **Spark** page, find the desired workspace and click the name of the workspace.
     
     ⅳ. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Gateways**.
     
  b. On the **Gateways** page, click the **Livy Gateways** tab.

  c. On the **Livy Gateways** tab, click **Create Livy Gateway**.

  d. On the Create Livy Gateway page, configure the **Name** parameter and click **Create**. In this example, set the **Name** parameter to Livy-gateway.

     You can configure other parameters based on your business requirements. For more information, see Manage gateways.

  e. On the **Livy Gateways** tab, find the created gateway and click **Start** in the **Actions** column.

 2. Create a token.

a. On the **Gateways** page, find the gateway Livy-gateway and click **Tokens** in the Actions column.

b. On the Tokens tab, click **Create Token**.

c. In the **Create Token** dialog box, configure the **Name** parameter and click **OK**.

d. Copy the token.

> **Important**
After the token is created, you must immediately copy the token. After you leave the page, you can no longer view the token. If your token expires or is lost, reset the token or create another token.

## Step 2: Configure Apache Airflow
1. Run the following command to install Apache Livy in Apache Airflow:
```html
pip install apache-airflow-providers-apache-livy
```
2. Add a connection.
### Use the UI
On the web UI of Apache Airflow, find the default connection whose ID is livy_default and modify the connection properties. You can also create a connection on the web UI of Apache Airflow. For more information, see Creating a Connection with the UI.

You can modify the following connection properties:

- **Host**: Enter the **endpoint** of the created gateway.
- **Schema**: Enter **https**.
- **Extra**: Enter a JSON string. Use the token that you copied in the previous step for ``` x-acs-spark-livy-token. ```

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-livy-operator-to-submit-a-task-through-apache-airflow?utm_content=g_1000402629)         

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR?utm_content=g_1000403124"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>
