# Use EMR Serverless Spark to connect to an external Hive Metastore

E-MapReduce (EMR) Serverless Spark allows you to connect to an external Hive Metastore. This way, you can access the data that is stored in the Hive Metastore with ease. This topic describes how to...

## Prerequisites

A workspace and an SQL compute are created. For more information, see [Create a workspace](link_to_workspace) and [Manage SQL sessions](link_to_sessions).

## Limits

- You can only preview the Hive Metastore service. To use the Hive Metastore service, join the DingTalk group (ID: **58570004119**) that provides technical support for EMR Serverless Spark custom...
- To use the Hive Metastore service, you must restart the existing compute service in your workspace.
- After you specify a Hive Metastore as a default catalog, your workflow tasks automatically depend on the Hive Metastore.

## Procedure

### Step 1: Prepare the Hive Metastore service

**Note:** In this example, the Hive Metastore that is deployed in EMR on ECS is used as an external service. If a Hive Metastore has been deployed in your virtual private cloud (VPC), skip this step.

1. On the EMR on ECS page, create a DataLake cluster that contains the Hive service and for which the **Metadata** parameter is set to **Built-in MySQL**. For more information, see [Create a cluster](link_to_cluster).
2. Log on to the master node of the DataLake cluster in SSH mode. For more information, see [Log on to a cluster](link_to_logon).
3. Run the following command to open the Hive CLI:
   ```bash
   hive

4. Run the following commands to create a table named dw_users that points to Object Storage Service (OSS) and write data to the table:

CREATE TABLE `dw_users`(
  `name` string)
LOCATION
  'oss://<yourBucket>/path/to/file';

INSERT INTO dw_users select 'Bob';
