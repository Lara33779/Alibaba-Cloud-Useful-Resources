When using Alibaba Cloud EMR Serverless Spark Notebook, you can directly access OSS or OSS-HDFS data sources through Hadoop commands. This topic provides a detailed description of how to operate OSS/OSS-HDFS using Hadoop commands.

# Prerequisites
- EMR Serverless Spark environment preparation:
  - A Notebook session has been created using engine version esr-4.1.1 as an example. For more information, see Manage Notebook sessions.
  - A Notebook development environment has been set up. For more information, see Notebook development.
- OSS service preparation:
  - The OSS service is active and a bucket has been created. For more information, see Activate OSS service and Create a bucket.
  - To use the OSS-HDFS service, you must first activate it. For details, see Activate OSS-HDFS service.
- Permission configuration:
To access OSS/OSS-HDFS across accounts, configure the necessary permissions. For details, see How to access Alibaba Cloud OSS across accounts.

> **Note**
In this topic's example, the **Authorization Operation** configured in the OSS console is **Read/write**. Perform the authorization operation that suits your needs.

# Supported operation types

The current version supports various operations on OSS/OSS-HDFS, including but not limited to:
- ls: List files and folders in a specified OSS/OSS-HDFS path.
- mv: Move files or folders.
- cp: Copy files or folders.
- stat: Retrieve metadata for a specific file or folder.

Execute the !hadoop fs -help command to view help information.
# Access path format
The access paths for OSS/OSS-HDFS are as follows:
- OSS path format: oss://<bucketName>/<object-path>
- OSS-HDFS path format: oss://<bucketName>.<region>.oss-dls.aliyuncs.com/<object-path>

The parameters involved are described below:
- bucketName: The name of the OSS bucket, e.g., my-bucket.
- region: The region where the OSS bucket is located, e.g., cn-hangzhou.
- object-path: The file path within the OSS bucket, e.g., spark/file.txt or logs/.
# Procedure
Within Notebook development, you can perform the following operations using the !hadoop fs command.

## List OSS path contents (ls)

Use the -ls parameter to list files and folders in the specified path.

```html
!hadoop fs -ls oss://<bucketName>/<object-path>
```

- Example 1: List all files and folders in the spark path.

```html
!hadoop fs -ls oss://my-bucket/spark/
```

The following information is returned.
![image](https://github.com/user-attachments/assets/cad2ec03-4f68-4a7d-924d-3bdd547cdd22)

- Example 2: Find all files and folders containing "user" using the -ls and grep commands.

```html
!hadoop fs -ls oss://my-bucket/spark/ | grep user
```

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:

👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-hadoop-commands-in-a-notebook-to-access-oss-or-oss-hdfs?utm_content=g_1000402585)  

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

