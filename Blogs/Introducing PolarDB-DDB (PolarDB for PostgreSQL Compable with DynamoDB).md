## PolarDB-DDB: A High-Performance, Cost-Effective Cloud-Native Alternative Compatible with DynamoDB
We are excited to announce the release of PolarDB's DynamoDB Compatibility feature (PolarDB-DDB) as part of the latest version of PolarDB for PostgreSQL. This new capability is highly compatible with the Amazon DynamoDB API and includes a comprehensive migration solution. With PolarDB-DDB, you can transition your existing business systems without modifying your code by connecting through native DynamoDB drivers (such as the Python or Go SDK).

PolarDB-DDB boasts enterprise-level capabilities, including instant elastic scaling, massive storage, and high availability, fully meeting the demands for high performance and reliability of modern applications. Furthermore, PolarDB-DDB provides a cost-effective solution that significantly outperforms DynamoDB, thereby empowering businesses to build efficient, stable, and economical data infrastructures perfect for large-scale NoSQL data processing and real-time access applications.

## Why Choose PolarDB-DDB?
While developers love DynamoDB for its flexible schemaless data model, its costs can escalate quickly with growing data volume and workloads.

PolarDB-DDB is our answer: a solution designed specifically to be compatible with the DynamoDB API. It retains the flexibility of NoSQL data models while offering enhanced data processing capabilities, more comprehensive cloud-native features, and superior cost-effectiveness.

At its core, PolarDB-DDB is powered by a mature PostgreSQL engine, leveraging efficient JSONB storage, flexible indexing, complete ACID transaction capability, and robust SQL analysis. In addition, backed by a vibrant open-source community, PolarDB-DDB is able to continuously provide ongoing feature enhancements, plugin extensions, and security updates, ensuring long-term stable evolution.

## Key Advantages of PolarDB-DDB
**1. Convenience and Scalability:**

Highly compatible with the DynamoDB API, requiring minimal code changes while offering a schemaless, elastic, and easily scalable experience.
Supports complex queries, batch processing, and ETL using SQL, allowing seamless integration with BI tools and enterprise systems, while maintaining API simplicity.

**2. Elasticity and Automated Maintenance:**

Provides instant elastic scaling (serverless mode); computing resources can dynamically adjust to business needs.
Uses a multi-reader, single-writer architecture with built-in automatic load balancing to support high-concurrency scenarios.
Separates compute and storage, automatically expanding storage based on demand, accommodating data growth from terabytes to petabytes.

**3. Fully Managed Service:**

There’s no need for hardware management, master-slave replication, or backup recovery, significantly reducing operational burdens.

**4. Ecosystem Integration and Innovation:**

Deeply integrates into the PostgreSQL ecosystem, compatible with mainstream BI tools, data pipelines, programming languages, and heterogeneous database connections, while allowing SQL and NoSQL usage together, aiding efficient data governance, business expansion, and multi-cloud deployment.

**5. High Cost-Effectiveness:**

Under equivalent specifications or performance conditions, PolarDB-DDB can achieve over 30% cost savings compared to DynamoDB through improved performance and resource utilization. This cost advantage is especially pronounced in scenarios involving heavy writes and high concurrent queries.
In summary, PolarDB-DDB combines the flexibility of NoSQL and the powerful analytical capabilities of SQL, greatly simplifying data architecture, enabling businesses to benefit from high cost-Effectiveness, ease of maintenance, and sustainable volutive without compromise.

## Architecture Overview: How to Achieve Compatibility and High Performance?
<img width="1642" height="1014" alt="image" src="https://github.com/user-attachments/assets/00bd0efb-adc5-4dd1-8173-b1eefd440c7b" />


**Database Proxy Layer**
PolarDB-DDB uses a dedicated proxy layer to efficiently convert DynamoDB API requests and translate them to the PostgreSQL protocol. Logic for protocol parsing, authentication, and conversion is centralized at the proxy layer. Communication between the proxy and clients uses the HTTP protocol of DynamoDB, while standard PostgreSQL protocol is used between the proxy and database compute nodes. This bridging and decoupled architecture effectively avoids embedding DynamoDB's specific HTTP interfaces and authentication mechanisms into the PostgreSQL kernel, thus preserving the simplicity and stability of the PostgreSQL architecture. Additionally, this design reuses PolarDB’s connection pool, load balancing, and read-write separation capabilities, mitigating maintenance burdens arising from protocol coupling.

Here's the workflow for a complete DynamoDB request:

The client initiates an HTTP request via the DynamoDB endpoint.
The database proxy authenticates the request using the key in the request header.
Once authenticated, the protocol converter interprets the requests and translates them into JSON-targeted PostgreSQL extension protocol messages.
Then, the router intelligently sends requests to optimal compute nodes based on request type (read/write) and current node load conditions.
When responding, the database proxy converts PostgreSQL's response data back to a DynamoDB-compatible HTTP format, ensuring a consistent API experience for the client.
This decoupled architecture preserves the stability of the PostgreSQL core while reusing PolarDB’s mature connection pool, load balancing, and read-write separation capabilities, supporting hybrid queries (SQL & NoSQL) that enhance system flexibility.

Click here to [read all](https://www.alibabacloud.com/blog/introducing-polardb-ddb-polardb-for-postgresql-compable-with-dynamodb_602530).
