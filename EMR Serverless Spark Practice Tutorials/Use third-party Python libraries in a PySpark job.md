In most cases, third-party Python libraries are used to enhance the data processing and analysis capabilities of PySpark jobs. This topic describes how to use Conda and Python EXecutable (PEX) to integrate third-party Python libraries into the Serverless Spark environment. This helps ensure that jobs remain stable and flexible in distributed computing scenarios.

# Background information
Conda is a cross-platform package and environment management system. You can use Conda to easily create, save, load, and switch between environments that have different Python versions and library dependencies. PEX is a tool that you can use to package a Python application and the corresponding dependencies into an executable file.
# Prerequisites
- An Elastic Compute Service (ECS) instance that uses the Alibaba Cloud Linux 3 OS is created and connected to the Internet. For more information, see Create an instance on the Custom Launch tab.
> **Note**
You can also use an idle node in an existing E-MapReduce (EMR) cluster that is created on the EMR on ECS page.
- A workspace is created. For more information, see Create a workspace.
# Limits
You must install Python 3.8 or later. In this example, Python 3.8 is used.
# Use Conda
## Step 1: Create and deploy a Conda environment
1. Run the following commands to install Miniconda:
```html
wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh

./Miniconda3-latest-Linux-x86_64.sh -b
source miniconda3/bin/activate
```
2. Build a Conda environment that uses Python 3.8 and NumPy.
```html
conda create -y -n pyspark_conda_env -c conda-forge conda-pack numpy python=3.8
conda activate pyspark_conda_env
conda pack -f -o pyspark_conda_env.tar.gz
```
## Step 2: Upload resource files to OSS
1. Click kmeans.py and kmeans_data.txt to download the resource files.
   You can also create the kmeans.py sample script and the kmeans_data.txt data file. Sample content in the files:
   
**kmeans.py**
   
```html
   """
A K-means clustering program using MLlib.

This example requires NumPy (http://www.numpy.org/).
"""
import sys

import numpy as np
from pyspark import SparkContext
from pyspark.mllib.clustering import KMeans


def parseVector(line):
    return np.array([float(x) for x in line.split(' ')])


if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: kmeans <file> <k>", file=sys.stderr)
        sys.exit(-1)
    sc = SparkContext(appName="KMeans")
    lines = sc.textFile(sys.argv[1])
    data = lines.map(parseVector)
    k = int(sys.argv[2])
    model = KMeans.train(data, k)
    print("Final centers: " + str(model.clusterCenters))
    print("Total Cost: " + str(model.computeCost(data)))
    sc.stop()
```

**kmeans_data.txt**
```html
0.0 0.0 0.0
0.1 0.1 0.1
0.2 0.2 0.2
9.0 9.0 9.0
9.1 9.1 9.1
9.2 9.2 9.2
```

2. Upload the ```pyspark_conda_env.tar.gz```, ```kmeans.py```, and ```kmeans_data.txt``` files to Object Storage
## Step 3: Develop and run a job
1. In the left-side navigation pane of the EMR Serverless Spark page, click **Data Development**.

2. On the Development tab, click **Create**.

3. In the Create dialog box, specify a job name in the Name field, choose **Batch Job > PySpark** from the Type drop-down list, and then click **OK**.

4. In the upper-right corner of the configuration tab of the job, select a queue from the Resource Queue drop-down list.

5. Configure the parameters and click **Run**. The following table describes the parameters.

| **Parameter** | **Description** |
| --- | --- |
| **Main Python Resources** | Select **OSS** from the drop-down list and enter the OSS path of the kmeans.py script. Example: oss://<yourBucketName>/kmeans.py. |
| **Execution Parameters** | Enter the OSS path of the ```kmeans_data.txt``` data file. Format: ```oss://<yourBucketName>/kmeans_data.txt 2```. |
| **Archive Resources** | Select **OSS** from the drop-down list and enter the OSS path of the ```pyspark_conda_env.tar.gz``` package. Format: ```oss://<yourBucketName>/pyspark_conda_env.tar.gz#condaenv```. |
| **Spark Configuration** | ```spark.pyspark.driver.python  ./condaenv/bin/python. spark.pyspark.python         ./condaenv/bin/python |

For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-third-party-libraries-of-python-in-a-pyspark-program)   

# Get 1000 CU·H Free – Try EMR Serverless Spark for 3 Months!🔥

Click the image to claim your free trial now! 

<a href="https://www.alibabacloud.com/free?_p_lc=1&accounttraceid=1d858b15fd1d4f139f1199f1f783aa15kask&keywords=EMR?utm_content=g_1000403124"><img src="https://img.alicdn.com/imgextra/i2/O1CN018w2DzS1d6EwTXCSen_!!6000000003686-0-tps-428-493.jpg"></a>

