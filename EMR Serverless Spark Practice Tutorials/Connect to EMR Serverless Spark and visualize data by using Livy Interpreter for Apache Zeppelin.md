Apache Zeppelin provides an interactive development environment that enables users to write code, run queries, and perform data visualization and analysis in a web UI. This topic describes how to connect to E-MapReduce (EMR) Serverless Spark by using Livy Interpreter for Apache Zeppelin to efficiently build and optimize an interactive development environment.

## Prerequisites
- An EMR Serverless Spark workspace is created. For more information, see Create a workspace.

- Apache Zeppelin is installed and started. For more information, see Apache Zeppelin official documentation.

## Procedure
### Step 1: Create a gateway and a token
1. Create and start a gateway.
   
a. Go to the **Gateways** page.

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
     > **Important** After the token is created, you must immediately copy the token. After you leave the page, you can no longer view the token. If your token expires or is lost, reset the token or create another token.
     
### Step 2: Configure Livy Interpreter for Apache Zeppelin
1. Log on to Apache Zeppelin, click the username in the upper-right corner, and then select **Interpreter** from the drop-down list.

![image](https://github.com/user-attachments/assets/0189e30e-262c-4ff0-85bf-ffce7109783a)

2.Click + **Create** in the upper-right corner and set the required parameters to create an interpreter.

| **Parameter** | **Description** |
| --- | --- |
| **Interpreter Name** | Enter a custom name, such as mylivy. |
| **Interpreter Group** | Set this parameter to **livy**. |

3. After you set the Interpreter Group parameter to **livy**, configure the required parameters.

![image](https://github.com/user-attachments/assets/5c7fc4ca-90d8-4711-82cb-2a1602541c79)
The following table describes the required parameters. You can also configure other parameters based on your business requirements. For more information, see Apache Zeppelin official documentation.
| **Parameter** | **Description** |
| **zeppelin.livy.url** | The URL of the Livy gateway. Enter the URL in the ```http://{endpoint}``` format. ```{endpoint}``` indicates the **internal endpoint** of the Livy gateway that you created.![image](https://github.com/user-attachments/assets/88458143-dd6f-4f43-9099-81c5766be156) |
| **zeppelin.livy.session.create_timeout** | The maximum wait time for Apache Zeppelin to create a session. Unit: seconds. We recommend that you set this parameter to 600. |
| **zeppelin.livy.http.headers** | The custom header of the HTTP request. You need to click the ```+``` icon to add the configuration and enter ```x-acs-spark-livy-token:{token}. {token}``` is the token that you created on the **Token Management** tab. |

4. Click **Save** in the lower part of the page to save the settings.

### Step 3: Create a notebook for data analytics
1. In the top navigation bar, click **Notebook**. Then, select **Create new note**.

2. Enter a custom note name and select mylivy from the Default Interpreter drop-down list.
![image](https://github.com/user-attachments/assets/43d3a745-a575-4b20-bcff-94c42b1602f2)

3. Click **Create**.

4. Enter the following code in the created notebook to start a Spark session.

The time required for the first startup is 1 to 3 minutes. If you enter ```%pyspark```, the Python environment is used. If you enter ```%spark```, the Scala environment is used.

```html
%pyspark
```

After the Spark session is started, you can view the link to the Spark UI and execute the code. You can use Python and Scala code together.

![image](https://github.com/user-attachments/assets/9dc3d044-0fa2-486a-981f-521088bfa34b)

5. Enter the following code in the new notebook to query the available databases in the current Spark environment.

```html
%pyspark

spark.sql("show databases").show()
```

The following figure shows the returned information.
![image](https://github.com/user-attachments/assets/83fd0359-eb46-40f2-a325-1f7eafac4f6f)

6. Optional. View session information.
After you create a Spark session by using the Livy interface, you can view information about the Spark session, such as the session ID and status, on the Sessions tab of a specified Livy gateway.

a. On the **Livy Gateways** tab, find the desired Livy gateway and click the name of the gateway.

b. Click the **Sessions** tab.

On the Sessions tab, you can view information about the Spark session that is created by using the Livy interface.
![image](https://github.com/user-attachments/assets/8fd3ce88-cd2d-440e-b864-6760918c4d10)

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/connect-to-emr-serverless-spark-and-visualize-data-through-zeppelin-livy-interpreter?utm_content=g_1000402737)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR?utm_content=g_1000403124"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>




   



  
