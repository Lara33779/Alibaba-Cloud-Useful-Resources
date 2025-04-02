Jupyter Notebook is a powerful interactive development tool that allows you to write and run code via a web interface and view results in real time, eliminating the need for precompilation or separate script execution. This topic describes how to efficiently establish a development environment for EMR Serverless Spark interaction.

# Background information

Apache Livy facilitates Spark interaction through RESTful API calls, streamlining communication between Spark and the application server. For more information about the Livy API, see REST API.

Jupyter Notebook offers various methods to interact with EMR Serverless Spark, as detailed in the table below. Select a method based on your business needs.

| Method | Scenarios |
| --- | --- |
| Method 1: Quickly start the environment using a Docker image | Choose this method to rapidly establish a standalone development environment or to replicate identical settings across different machines. |
| Method 2: Start the environment using the sparkmagic plug-in | The sparkmagic plug-in for Jupyter Notebook interacts with Spark through RESTful APIs and supports Livy, Livy Lighter, and Ilum protocols. Configure the sparkmagic plug-in and invoke EMR Serverless Spark's Livy API to create a development environment for remote Spark cluster interaction. |

# Prerequisites
- Depending on the chosen method for interactive development, perform the corresponding operation:
  - Method 1: Quickly start the environment using a Docker image: Ensure Docker is installed. For more information, see Docker official documentation.
  - Method 2: Start the environment using the sparkmagic plug-in: Jupyter Notebook must be installed and running. For more information, see Project Jupyter | Installing Jupyter. In this example, Jupyter Notebook and Python 3.8 are utilized.
    In this example, we use Jupyter Notebook and Python 3.8.
- A workspace must be created. For more information, see Create a Workspace.

# Method 1: quickly start the environment by using a Docker image
## Step 1: Create a gateway and access token
1. Create and start a gateway.

   a. Go to the **Gateways** page.
   
     ⅰ. Log on to the EMR console.
     
     ⅱ. In the left-side navigation pane, choose **EMR Serverless > Spark**.
     
     ⅲ. On the **Spark** page, find the desired workspace and click the name of the workspace.
      
     ⅳ. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Gateways**.
      
   b. On the **Gateways** page, click the **Livy Gateways** tab.
   
   c. On the **Livy Gateways** tab, click **Create Livy Gateway**.
   
   d. On the Create Livy Gateway page, configure the **Name** parameter and click **Create**. In this example, set the **Name** parameter to Livy-gateway.
       You can configure other parameters based on your business requirements. For more information, see Manage Gateways.
   
   e. On the **Livy Gateways** tab, find the created gateway and click **Start** in the **Actions** column.

2. Create a token.

   a. On the **Gateways** page, find the gateway Livy-gateway and click **Tokens** in the Actions column.

   b. On the Tokens tab, click **Create Token**.

   c. In the **Create Token** dialog box, configure the **Name** parameter and click **OK**.

   d. Copy the token.

> **Important**
After the token is created, you must immediately copy the token. After you leave the page, you can no longer view the token. If your token expires or is lost, reset the token or create another token.

## Step 2: pull and start the image by using Docker

1. Execute the following command to pull the image.
```html
docker pull emr-registry-registry.cn-hangzhou.cr.aliyuncs.com/serverless-spark-public/emr-spark-jupyter:latest
```
2. Run the command below to start the image.

```html
docker run -p <host_port>:8888 emr-registry-registry.cn-hangzhou.cr.aliyuncs.com/serverless-spark-public/emr-spark-jupyter:latest <endpoint> <token>
```

The parameters are described in the following table:

| Parameter | Description |
| --- | --- |
| ```<host_port>``` | The port of the host. |
| ```<endpoint>``` | The endpoint of the Livy gateway. On the **Livy Gateway** page, click the name of the created Livy gateway and view the endpoint on the **Overview** tab.
| ```<token>``` | 	The token that you copy from Step 1. |

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-the-sparkmagic-plugin-of-jupyter-notebook-to-interact-with-serverless-spark?utm_content=g_1000402641)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR?utm_content=g_1000403124"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>
