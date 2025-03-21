Third-party Python libraries are often used to enhance the data processing and analysis capabilities of interactive PySpark jobs that run in notebooks. This topic describes how to install third-party Python libraries in a notebook.

## Background information
When you develop interactive PySpark jobs, you can use third-party Python libraries to enable more flexible and easier data processing and analysis. The following table describes the methods to use third-party Python libraries in a notebook. You can select a method based on your business requirements.

| **Method** | **Scenario** |
| --- | --- |
| Method 1: Run the pip command to install Python libraries | You want to process variables that are not related to Spark in a notebook, such as the return values calculated by Spark or custom variables. > **Important** You must reinstall the libraries after you restart a notebook session. |
| Method 2: Create a runtime environment to define a custom Python environment | You want to use third-party Python libraries to process data in PySpark jobs, and you want the third-party libraries to be preinstalled each time a notebook session is started. |
| Method 3: Add Spark configurations to create a custom Python environment | You want to use third-party Python libraries to process data in PySpark jobs. For example, you use third-party Python libraries to implement Spark distributed computing. |
## Prerequisites
- A workspace is created. For more information, see Create a workspace.

- A notebook session is created. For more information, see Manage notebook sessions.

- A notebook is developed. For more information, see Develop a notebook.

## Procedure
### Method 1: Run the pip command to install Python libraries
1. Go to the configuration tab of a notebook.
  a. Log on to the E-MapReduce (EMR) console.

  b. In the left-side navigation pane, choose EMR Serverless > Spark.

  c. On the Spark page, find the desired workspace and click the name of the workspace.

  d. In the left-side navigation pane of the EMR Serverless Spark page, click Data Development.

  e. Double-click the notebook that you developed.
  
2. In a Python cell of the notebook, enter the following command to install the scikit-learn library and click the image icon.

```html
pip install scikit-learn
```

3. Add a new Python cell, enter the following command in the cell, and then click the image icon.

```html
# Import datasets from the scikit-learn library. 
from sklearn import datasets

# Load the built-in dataset, such as the Iris dataset. 
iris = datasets.load_iris()
X = iris.data # The feature data.
y = iris.tar get # The tag.

# Divide datasets. 
from sklearn.model_selection import train_test_split

# Divide the datasets into training sets and test sets. 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Use the support vector machine (SVM) model for training. 
from sklearn.svm import SVC

# Create a classifier instance. 
clf = SVC(kernel='linear') # Use a linear kernel. 

# Train the model. 
clf.fit(X_train, y_train)

# Use the trained model to make predictions. 
y_pred = clf.predict(X_test)

# Evaluate the model performance. 
from sklearn.metrics import classification_report, accuracy_score

print(classification_report(y_test, y_pred))
print("Accuracy:", accuracy_score(y_test, y_pred))
```

The following figure shows the results.

![image](https://github.com/user-attachments/assets/5a58c9dc-92a9-4931-96c0-9baf832e2dab)

### Method 2: Create a runtime environment to define a custom Python environment
#### Step 1: Create a runtime environment
1. Go to the Runtime Environments page.

a. Log on to the EMR console.

b. In the left-side navigation pane, choose **EMR Serverless > Spark**.

c. On the **Spark** page, find the desired workspace and click the name of the workspace.

d. In the left-side navigation pane of the **EMR Serverless Spark** page, click **Runtime Environments**.

2. Click **Create Runtime Environment**.

3. On the **Create Runtime Environment** page, configure the Name parameter. Then, click **Add Library** in the Libraries section.

For more information, see Manage runtime environments.

4. In the **Create Library** dialog box, set the **Source Type** parameter to **PyPI**, configure the PyPI Package parameter, and then click **OK**.

Specify the library name and version in the required format for the **PyPI Package** parameter. If you do not specify a version, the library of the latest version is installed. Example: ```scikit-learn```.

5. Click **Create**.

The system starts to initialize the runtime environment after you click Create.

### Step 2: Use the runtime environment
> **Note** You must stop a session before you modify the session.

1. Go to the Notebook Sessions tab.

  a. In the left-side navigation pane of the **EMR Serverless Spark** page, choose **Operation Center > Sessions**.

  b. Click the **Notebook Sessions** tab.

  2. Find the desired session and click **Edit** in the Actions column.

  3. Select the created runtime environment from the **Runtime Environment** drop-down list and click **Save Changes**.

  4. In the upper-right corner of the page, click **Start**.

### Step 3: Use the Scikit-learn library to classify data
1. Go to the configuration tab of the desired notebook.

   a. In the left-side navigation pane of the **EMR Serverless Spark** page, click **Data Development**.

   b. Double-click the notebook that you developed.

2. Add a new Python cell, enter the following command in the cell, and then click the image icon.
```html
# Import datasets from the scikit-learn library. 
from sklearn import datasets

# Load the built-in dataset, such as the Iris dataset. 
iris = datasets.load_iris()
X = iris.data # The feature data.
y = iris.tar get # The tag.

# Divide datasets. 
from sklearn.model_selection import train_test_split

# Divide the datasets into training sets and test sets. 
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Use the SVM model for training. 
from sklearn.svm import SVC

# Create a classifier instance. 
clf = SVC(kernel='linear') # Use a linear kernel. 

# Train the model. 
clf.fit(X_train, y_train)

# Use the trained model to make predictions. 
y_pred = clf.predict(X_test)

# Evaluate the model performance. 
from sklearn.metrics import classification_report, accuracy_score

print(classification_report(y_test, y_pred))
print("Accuracy:", accuracy_score(y_test, y_pred))
```
For detailed steps and complete instructions, please refer to the full article on Alibaba Cloud’s official documentation:  
👉 [Read the full guide here](https://www.alibabacloud.com/help/emr/emr-serverless-spark/use-cases/use-third-party-libraries-of-python-in-notebook?utm_content=g_1000402731)   
