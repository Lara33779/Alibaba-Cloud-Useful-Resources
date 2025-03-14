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
     
     ⅱ. In the left-side navigation pane, choose EMR Serverless > Spark.
     
     ⅲ. On the Spark page, find the desired workspace and click the name of the workspace.
     
     ⅳ. In the left-side navigation pane of the EMR Serverless Spark page, choose Operation Center > Gateways.
     
  b. On the Gateways page, click the Livy Gateways tab.

  c. On the Livy Gateways tab, click Create Livy Gateway.

  d. On the Create Livy Gateway page, configure the Name parameter and click Create. In this example, set the Name parameter to Livy-gateway.

     You can configure other parameters based on your business requirements. For more information, see Manage gateways.

  e. On the Livy Gateways tab, find the created gateway and click Start in the Actions column.

  To be continued, stay tuned!
