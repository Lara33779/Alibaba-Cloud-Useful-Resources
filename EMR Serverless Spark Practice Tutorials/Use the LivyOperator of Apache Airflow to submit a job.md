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
