By Decai Xu, Yuanyi, Yuanyun, and Suying

A new compatibility layer enables code-free migration, serverless elasticity, and PostgreSQL reliability for your document database needs.

Following the recent release of DynamoDB compatibility, we are excited to announce another major innovation for PolarDB: native compatibility with the MongoDB data model.

This new feature allows you to migrate your MongoDB-based applications to PolarDB for PostgreSQL without changing a single line of code. Gain the enterprise-grade reliability and advanced features of PostgreSQL while maintaining the flexibility and familiar development patterns of a document database.

In this post, we'll guide you through:

● Provisioning a Serverless PolarDB instance with MongoDB compatibility.

● Connecting to it using standard MongoDB tools.

● Validating its performance and auto-scaling capabilities with a benchmark test.

## Key Benefits:
● Seamless Migration: Preserve your existing MongoDB application code and development tools. The compatibility layer handles the translation, making migration fast and frictionless.

● Effortless Scaling: Leverage the power of PolarDB Serverless to automatically scale compute resources up and down based on real-time workload demands, optimizing both performance and cost.

● Unified Architecture: Combine the flexibility of MongoDB's document model with the proven reliability, ACID compliance, and rich ecosystem of PostgreSQL in a single, powerful database.

## Technical Insights: How It Works
The MongoDB compatibility layer is an intelligent translation service built into PolarDB that:

● Translates the MongoDB Wire Protocol into corresponding PostgreSQL operations in real-time.

● Preserves Document Semantics by leveraging PostgreSQL's native JSONB capabilities, ensuring document-oriented data is stored and queried efficiently.

● Supports Common Operations, including most CRUD (Create, Read, Update, Delete) operations and familiar query patterns.

## Getting Started: A Step-by-Step Guide
### Step 1: Provision Your PolarDB Serverless Instance
First, provision a new PolarDB for a PostgreSQL serverless instance with the following recommended configurations.

Configuration Item	Recommended Value
Billing Type	Serverless
Database Engine	PostgreSQL 16
Minimum PCUs per Node	1
Maximum PCUs per Node	16
Minimum Read-only Nodes	1
Maximum Read-only Nodes	1
You can find these options on the Alibaba Cloud purchase page.
<img width="2346" height="1000" alt="image" src="https://github.com/user-attachments/assets/6c83b006-71e6-4ded-9b97-1e2100773485" />

<img width="2276" height="1020" alt="image" src="https://github.com/user-attachments/assets/a3a70636-1696-4d50-878d-b04a3e95c69b" />


Important Note: Preview Limitation on Scaling

The current preview version supports vertical scaling (scaling up and down) but not horizontal scaling (scaling out and in). For this reason, set Maximum Read-only Nodes to 1. Full horizontal scaling capabilities will be available in the general release.

### Step 2: Enable MongoDB Compatibility (Preview Access)
As this feature is currently in preview, access must be enabled for your account.

To activate the feature, contact your Alibaba Cloud account manager, technical support engineer, or open a support ticket to request access to the MongoDB compatibility preview.

### Step 3: Connect to Your Instance
Once your instance is running and the feature is enabled, follow these steps to connect:

For more details, please click on [this link](https://www.alibabacloud.com/blog/seamless-migration-run-mongodb-workloads-on-polardb-for-postgresql-serverless_602540?utm_content=g_1000407201) to read all.
