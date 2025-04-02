Apache Airflow is a powerful workflow automation and scheduling tool that allows developers to orchestrate, schedule, and monitor the running of data pipelines. E-MapReduce (EMR) Serverless Spark provides a serverless computing environment for processing large-scale data processing jobs. This topic describes how to use Apache Airflow to enable automatic job submission to EMR Serverless Spark. This way, you can automate job scheduling and running to manage data processing jobs more efficiently.

# Prerequisites
- Airflow is installed and started. For more information, see Installation of Airflow.
- A workspace is created. For more information, see Create a workspace.
# Usage notes
You cannot call the EmrServerlessSparkStartJobRunOperator operation to query job logs. If you want to view job logs, you must go to the EMR Serverless Spark page and find the job run whose logs you want to view by job run ID. Then, you can check and analyze the job logs on the Logs tab of the job details page or on the Spark Jobs page in the Spark UI.
# Procedure
## Step 1: Configure Apache Airflow
1. Download airflow_alibaba_provider-0.0.3-py3-none-any.whl.
2. Install the airflow-alibaba-provider plug-in on each node of Airflow. The airflow-alibaba-provider plug-in is provided by EMR Serverless Spark. It contains the EmrServerlessSparkStartJobRunOperator component, which is used to submit jobs to EMR Serverless Spark.
```html]
   pip install airflow_alibaba_provider-0.0.3-py3-none-any.whl
   ```
3. Add a connection.

### Use the CLI

Use the Airflow command-line interface (CLI) to run commands to establish a connection. For more information, see Creating a Connection.

```html
airflow connections add 'emr-serverless-spark-id' \
    --conn-json '{
        "conn_type": "emr_serverless_spark",
        "extra": {
            "auth_type": "AK", # The AccessKey pair is used for authentication. 
            "access_key_id": "<yourAccesskeyId>", # The AccessKey ID of your Alibaba Cloud account. 
            "access_key_secret": "<yourAccesskeyKey>", # The AccessKey secret of your Alibaba Cloud account. 
            "region": "<yourRegion>"
        }
    }'
```

### Use the UI

You can manually create a connection with the Airflow web UI. For more information, see Creating a Connection with the UI.

On the **Add Connection** page, configure the parameters.

![image](https://github.com/user-attachments/assets/3710bea1-b33c-45fb-bdbf-c52eb225beda)

The following table describes the parameters:

| Parameter | 	Description |
| --- | --- |
| Connection Id | The connection ID. In this example, enter emr-serverless-spark-id. |
| Connection Type | 	The connection type. In this example, select **Generic**. If Generic is not available, you can also select **Email**. |
| Extra | The additional configuration. In this example, enter the following content |
```html
{
            "auth_type": "AK", # The AccessKey pair is used for authentication. 
            "access_key_id": "<yourAccesskeyId>", # The AccessKey ID of your Alibaba Cloud account. 
            "access_key_secret": "<yourAccesskeyKey>", # The AccessKey secret of your Alibaba Cloud account. 
            "region": "<yourRegion>"
        }
```
## Step 2: Configure DAGs
Apache Airflow provides Directed Acyclic Graphs (DAGs), which allow you to declare how jobs should run. The following are examples of how to call the EmrServerlessSparkStartJobRunOperator operation to run different types of Spark jobs in Apache Airflow.

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-apache-airflow-to-submit-tasks-to-emr-severless-spark?utm_content=g_1000402628)        

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

