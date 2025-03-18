E-MapReduce (EMR) Serverless Spark provides built-in MaxCompute data sources based on the Spark DataSource V2 API. If you want to connect a job to MaxCompute, you need to only add specific configurations when you develop the job. This topic describes how to read data from and write data to MaxCompute in EMR Serverless Spark.

# Background information
MaxCompute, formerly known as ODPS, is a fast and fully managed computing platform for large-scale data warehousing. MaxCompute can process up to exabytes of data. MaxCompute supports batch computing and storage of structured data and provides various data warehousing solutions and big data analysis and modeling services. For more information about MaxCompute, see What is MaxCompute?
## Prerequisites
- An EMR Serverless Spark workspace is created. For more information, see Create a workspace.
- A MaxCompute project is created. For more information, see Create a MaxCompute project.
## Limits
The Spark version that is included in the engine version of a session must be 3.3.x. Example: esr-2.4.1 (Spark 3.3.1, Scala 2.12).
## Procedure
### Step 1: Create a session to connect to the MaxCompute project
You can create an SQL session or a notebook session to connect to the MaxCompute project. For more information about sessions, see Manage sessions.
Create an SQL session to connect to the MaxCompute project
1. Go to the Sessions page.

    a. Log on to the EMR console.

    b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

    c. On the **Spark** page, click the name of the workspace that you want to manage.

    d. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

2. On the **SQL Sessions** tab, click **Create SQL Session**.

3. On the Create SQL Session page, configure the parameters and click **Create**. The following table describes the parameters.

