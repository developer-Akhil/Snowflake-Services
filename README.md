**Contents**
1. How many types of accounts in snowflake?
2. Why we need snowflake when do we have redshift and azure synapse?
3. Snowflake Database Architecture
4. Understanding End-to-End Encryption in Snowflake
5. Order of execution of a Query
6. Can we modify warehouse-size on the fly?
7. What are Maximized and Auto-scale mode in Snowflake?
8. What are Auto-suspension and Auto-resumption?
9. Secure Data Sharing
10. Scaling Policy
11. Micro-partitions
12. Clustering Key
13. Re-Clustering Key
14. Automatic Clustering
15. Snowflake – Cache
16. Q How to clear a Snowflake cache?
17. System-Defined Roles
    a. ORGADMIN (aka Organization Administrator)
    b. ACCOUNTADMIN (aka Account Administrator)
    c. SECURITYADMIN (aka Security Administrator)
    d. USERADMIN (aka User and Role Administrator)
    e. SYSADMIN (aka System Administrator)
    f. PUBLIC
18. User and Role
19. Privilege
20. Snowflake Integration
21. External Table
22. SnowPipe
23. Change Data capture (CDC)
    Standard Streams	39
        a. Append-only Streams	39
        b. Insert-only Streams	39
24. Retrieve up to the last 100 queries run in the current session
25. What Problem Qualify Solved?
26. CAST and TRY_CAST
27. Information Schema and Public Schema
28. Snowflake – Parament/Transient/Temporary Tables
29. Delete vs Truncate
30. Stored Procedure
31. Time Travel
32. Fail Safe
33. Retention Period
34. Zero Copy Clone
35. Copy table vs Clone table
36. Undrop a Table, Database or Schema
37. Regular Schema vs Managed Schema
38. Tasks
39. Snowflake – View
40. How do you stop a running query in SnowSQL
41. Masking Policy (Dynamic Data Masking)
42. Snowflake Row-level Security
43. What key considerations should be addressed when migrating an on-premise Oracle database to Snowflake?
44. How are costs incurred?
45. Snowflake Credit Usage too high compared to query runtime
46. Snowflake query credit calculation
47. If a query is taking longer to execute compared to its previous run, what actions should we consider?
48. I have a table that contains a gender column that has three values: male, female, and null. How many partitions will create a gender column in snowflake?
49. Bloom Filter in Snowflake


# How many types of accounts in snowflake?
Standard, Enterprise, Business Critical, and Virtual Private Snowflake (VPS).

# Snowflake-Services
**Why we need snowflake when do we have redshift and azure synapse?**
The choice between Snowflake, Amazon Redshift, and Azure Synapse Analytics (formerly known as SQL Data Warehouse) depends on your specific requirements and the existing technology stack of your organization. Each of these data warehousing solutions has its strengths and may be more suitable for different use cases:

**Snowflake:**
**Multi-Cloud Support:** Snowflake is known for its native multi-cloud support, which means you can deploy it on multiple cloud platforms (AWS, Azure, and Google Cloud). This can be valuable if you want flexibility and avoid vendor lock-in.

**Zero Maintenance:** Snowflake is a fully managed service, meaning you don't have to worry about infrastructure management, scaling, or performance tuning.

**Data Sharing:** Snowflake provides robust data sharing capabilities, allowing you to securely share data with other Snowflake accounts without copying or moving it.

**Data Sharing Economy:** Snowflake's pricing model is based on actual usage, which can be cost-effective for organizations with varying workloads.

# Snowflake Database Architecture

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/af24d875-2011-4246-8913-a940f26f89dd)

**Service Layer**: In Snowflake, the Service Layer is a critical component of the architecture that handles various functionalities such as query parsing, optimization, execution, transaction management, security, and metadata management. It acts as an intermediary between the user and the underlying storage and compute layers of Snowflake.  Logically, this can be assumed to hold the result cache – a cached copy of the results of every query executed.

**Compute Layer**:  Which actually does the heavy lifting.  This is where the actual SQL is executed across the nodes of a Virtual Data Warehouse.  This layer holds a cache of raw data queried, and is often referred to as Local Disk I/O although in reality this is implemented using SSD storage.  All data in the compute layer is temporary, and only held as long as the virtual warehouse is active.
Compute Layer is responsible for executing queries and processing data. It is the component that performs the actual computation and analysis of data stored in Snowflake's distributed storage layer.

**Storage Layer**:  Which provides long term storage of results.  This is often referred to as Remote Disk, and is currently implemented on either Amazon S3 or Microsoft Blob storage.

# Understanding End-to-End Encryption in Snowflake
End-to-end encryption in Snowflake is a feature that provides enhanced security for data stored and processed within the Snowflake cloud data platform. It ensures that data remains encrypted throughout its entire lifecycle, from when it is initially ingested into Snowflake until it is accessed and processed by authorized users or applications. Here's an overview of how end-to-end encryption works in Snowflake:

**Encryption at Rest (Encryption at rest is designed to prevent the attacker from accessing the unencrypted data by ensuring the data is encrypted when on disk.):**

Snowflake encrypts data at rest using strong encryption algorithms, such as Advanced Encryption Standard (AES) with 256-bit keys. This encryption applies to all data stored in Snowflake, including tables, metadata, and backups. Each piece of data is encrypted individually.

**Snowflake-managed Keys**
All Snowflake customer data is encrypted by default using the latest security standards and best practices. Snowflake uses strong AES 256-bit encryption with a hierarchical key model rooted in a hardware security module.
Keys are automatically rotated on a regular basis by the Snowflake service, and data can be automatically re-encrypted (“rekeyed”) on a regular basis. Data encryption and key management is entirely transparent and requires no configuration or management.

**Encryption in Transit:**

Snowflake uses Transport Layer Security (TLS) to encrypt data in transit between the client and the Snowflake service. This ensures that data transferred over the network is secure and protected from eavesdropping (secretly listen to a conversation.).

**Data Key Management:**

One of the key components of Snowflake's end-to-end encryption is its use of hierarchical key management. Snowflake manages a hierarchy of encryption keys, including:
Root Key: The top-level key that encrypts and decrypts data keys.
Data Key: A unique key generated for each micro-partition of data in Snowflake. These data keys are used to encrypt the actual data within micro-partitions.
Metadata Key: Used to encrypt metadata associated with data, such as column names and data types.

**Client-Side Encryption:** 

Snowflake allows clients to perform client-side encryption before sending data to Snowflake. Clients can encrypt data using their own encryption keys (called customer-managed keys or CMKs). This provides an additional layer of security, as the data is already encrypted before it reaches Snowflake's infrastructure.
**Role-Based Access Control (RBAC):**

Snowflake's RBAC system ensures that only authorized users and roles can access and decrypt specific data. Users and roles are granted access to specific objects in Snowflake, and these permissions determine who can read, write, or modify data.

**Zero-Copy Clone Encryption:**

Snowflake allows for the creation of zero-copy clones of data, which also inherit the same encryption settings as the source data. This ensures that cloned data maintains the same level of security as the original data.

**Secure Data Sharing:**

Snowflake enables secure data sharing between organizations by allowing data providers to share encrypted data with data consumers. Data sharing is controlled, and data remains encrypted during the sharing process.

**Audit and Compliance:**

Snowflake provides extensive audit and compliance features to track and monitor data access and changes, ensuring that data is protected and compliant with regulatory requirements.
In summary, end-to-end encryption in Snowflake encompasses encryption at rest, encryption in transit, client-side encryption, hierarchical key management, and role-based access control to provide a comprehensive security framework for your data. It helps protect sensitive information and ensures that data remains confidential and secure throughout its lifecycle within the Snowflake platform.

**Can we modify warehouse-size on the fly?**

Yes, you can modify the size of a warehouse (i.e. Vertical Scaling) even if it is in a running state, provided the new change will only be applicable for newly queued queries and all existing queries will still use old config. You can modify warehouse size from the Context menu and also modify the min & max cluster under warehouse>config.

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/c9916622-7a79-4b4e-9571-8dedc289bd61)

**Under the Economy scaling policy:**
1.	Compute resources are shared across multiple virtual warehouses within a Snowflake account.
2.	Compute resources are dynamically allocated and released based on the workload demands of each virtual warehouse.
3.	When a virtual warehouse is idle, its compute resources are automatically released and made available to other virtual warehouses.
4.	When workload demand increases, compute resources are dynamically allocated to virtual warehouses as needed.

**What are Micro-partitions?**

All data in Snowflake tables is automatically divided into micro-partitions, which are contiguous units of storage. Each micro-partition contains between 50 MB and 500 MB of uncompressed data (note that the actual size in Snowflake is smaller because data is always stored compressed). Groups of rows in tables are mapped into individual micro-partitions, organized in a columnar fashion. This size and structure allows for extremely granular pruning of very large tables, which can be comprised of millions, or even hundreds of millions, of micro-partitions.
Benefits of Micro-partitioning
The benefits of Snowflake’s approach to partitioning table data include:
1.	In contrast to traditional static partitioning, Snowflake micro-partitions are derived automatically; they don’t need to be explicitly defined up-front or maintained by users.
2.	As the name suggests, micro-partitions are small in size (50 to 500 MB, before compression), which enables extremely efficient DML and fine-grained pruning for faster queries.
3.	Micro-partitions can overlap in their range of values, which, combined with their uniformly small size, helps prevent skew.
4.	Columns are stored independently within micro-partitions, often referred to as columnar storage. This enables efficient scanning of individual columns; only the columns referenced by a query are scanned.
5.	Columns are also compressed individually within micro-partitions. Snowflake automatically determines the most efficient compression algorithm for the columns in each micro-partition.

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/a8e2a698-24e9-4c10-a2e5-9f56a6e15b95)

**What are Maximized and Auto-scale mode in Snowflake?**

In Snowflake, you can provision warehouses in 2 different modes. Either it can be on maximized mode or Auto-Scale mode.

**Auto-Scale mode**: When you provide different values for Minimum & Maximum clusters like below, you are opting for auto-scale mode. In this mode, Snowflake starts and stops clusters as needed to dynamically manage the load on the warehouse. As the query load or concurrent user increases or decreases, the warehouse scale-out/in, respectively.

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/eb356ee6-1bef-4c23-bb47-c98196f06532)

**Maximized mode**: When you provide the same value(Should be >1) for Minimum & Maximum cluster like below, you are opting for maximized mode. In this mode, when the warehouse is started, Snowflake starts all the clusters so that maximum resources are available while the warehouse is running. This mode is suitable when you know your workload has concurrent users, and you are going to need a provisioned server to support them.

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/8ef68ff8-e6bd-495c-bc08-895862cc2a4c)

**What are Auto-suspension and Auto-resumption?**

A warehouse can be set to automatically resume or suspend, based on activity:
	By default, auto-suspend is enabled. Snowflake automatically suspends the warehouse if it is inactive for the specified period of time.
	By default, auto-resume is enabled. Snowflake automatically resumes the warehouse when any statement that requires a warehouse is submitted, and the warehouse is the current warehouse for the session.
	Auto-suspend and auto-resume apply only to the entire warehouse and not to the individual clusters in the warehouse.

**What is Scaling Policy in Snowflake?**

In Snowflake, we have predefined 2 types of scaling policy.

**1. Standard(default) and Economy**

**2. Standard Scaling Policy**

The Standard scaling policy is designed for workloads with consistent performance requirements and is suitable for most production environments. It provides dedicated compute resources to a virtual warehouse, ensuring consistent performance regardless of the overall system load.

**Under the Standard scaling policy:**

**1.**	The virtual warehouse is allocated a fixed number of compute resources, including CPU, memory, and storage.

**2.**	The allocated resources are reserved exclusively for the virtual warehouse, providing predictable and stable performance.

**3.**	The compute resources remain allocated and available to the virtual warehouse even when it's idle, ensuring immediate availability for query execution.

The Standard scaling policy is recommended when you have critical workloads or when you require predictable performance and consistent query response times.

**Economy Scaling Policy:**

The Economy scaling policy is a cost-saving option that dynamically shares compute resources among multiple virtual warehouses. It is suitable for non-production environments, such as development, testing, and ad-hoc analytics, where consistent performance may not be a top priority.
**Under the Economy scaling policy:**

**1.**	Compute resources are shared across multiple virtual warehouses within a Snowflake account.

**2.**	Compute resources are dynamically allocated and released based on the workload demands of each virtual warehouse.

**3.**	When a virtual warehouse is idle, its compute resources are automatically released and made available to other virtual warehouses.

**4.**	When workload demand increases, compute resources are dynamically allocated to virtual warehouses as needed.

**Q What is a Clustering Key in Snowflake?**

When data is loaded into a Snowflake table, it is automatically distributed across multiple nodes in the Snowflake cluster. By specifying a Cluster Key, Snowflake can group together rows with similar values in the Cluster Key columns, and store them together on the same node. This can drastically reduce the amount of data that needs to be scanned when running a query that filters on the Cluster Key columns.
In Snowflake, a clustering key is a column or set of columns used to cluster data in a table. The clustering key determines how data is physically stored in the table, and can significantly impact query performance.
When data is inserted into a table with a clustering key, Snowflake will physically group together data that shares the same value(s) in the clustering key columns. This can improve query performance by reducing the amount of data that needs to be scanned to answer a query.
Snowflake supports both single-column and multi-column clustering keys. When defining a table, you can specify the clustering key using the CLUSTER BY clause. For example, the following SQL statement creates a table with a clustering key on the "date" column:

**CREATE TABLE my_table (
    id INTEGER,
    date DATE,
    value FLOAT
)
CLUSTER BY (date);**

Once a table has been created with a clustering key, you can use the SHOW CLUSTERING KEY command to view information about the clustering key for that table.
It's important to choose a good clustering key for your tables in Snowflake, as this can have a significant impact on query performance. Factors to consider when choosing a clustering key include the distribution of data within the key columns, the types of queries you will be running, and the frequency and type of data updates.

**Here's how Snowflake calculates the clustering key:**

**Sorting Algorithm**: Snowflake uses a variant of the Z-order sorting algorithm to calculate the clustering key. The Z-order algorithm transforms the values of the clustering key columns into a single-dimensional value, which can then be used for efficient data sorting and grouping.

**Data Ordering**: Snowflake applies the Z-order algorithm to the values of the clustering key columns and determines the order in which data should be stored within each Micro-partition. This ordering is based on the combined values of the clustering key columns.

**Micro-partition Assignment**: Once the data is ordered based on the clustering key, Snowflake assigns the ordered data to Micro-partitions. Each Micro-partition contains a subset of data that shares the same or similar clustering key values.

**Efficiency and Performance**: By storing data with similar clustering key values together within Micro-partitions, Snowflake achieves data locality and reduces the need for data scans during query execution. Queries that leverage the clustering key can skip irrelevant Micro-partitions, resulting in faster query performance and reduced resource consumption.

**Q What is a Re-Clustering Key in Snowflake?**

Re-clustering in Snowflake refers to the process of reorganizing the data within a table based on its clustering key. When data is initially loaded into a table, it may not be perfectly clustered based on the key, or over time, data may become less optimally clustered due to updates or deletes. Re-clustering can be done to optimize the table's data layout and improve query performance.
Re-clustering in Snowflake is accomplished using the CLUSTER command. This command can be used to cluster an unclustered table or to re-cluster a table that already has a clustering key. The syntax for the CLUSTER command is as follows:

**ALTER TABLE table_name CLUSTER BY (clustering_key);**

When the CLUSTER command is executed, Snowflake reads the data in the table and reorganizes it based on the specified clustering key. This process can take some time and may use a significant amount of compute resources, especially for large tables.
Re-clustering can be done manually by running the CLUSTER command, or it can be automated using Snowflake's auto-reclustering feature. Auto-reclustering is a Snowflake feature that automatically re-clusters tables based on usage patterns and data changes. With auto-reclustering, Snowflake periodically monitors tables and re-clusters them as needed to maintain optimal query performance.


**What is Cache in Snowflake and thier types?**

Snowflake has a unique feature of caching. It provides fast and quick result with less data scan based on this caching. It even helps the customer to reduce their billing as well.
There are basically three types of caching in Snowflake.

**Metadata Caching**

**Query Result Caching**

**Data Caching**

By default, cache is enabled for all snowflake session. But user can disable it based on their needs. However, user can disable only Query Result caching but there is no way to disable Metadata Caching as well as Data Caching.
Metadata Caching
Metadata stores at Cloud Service Layer hence caching is also at same layer. These metadata caching is always enabled for everyone.

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/8fe7a69e-c16e-4598-9de3-10c934c792fb)

It basically contains the following details −
1. Row Count in a table.
2. MIN/MAX value of a column
3. Number of DISTINCT values in a column
4. Number of NULL values in a column
5. Details of different table versions
5. References of physical files
   
This information is basically used by SQL optimizer to execute faster and quicker. There could be a few queries those can be answered completely by metadata itself. For such kind of queries, no virtual warehouse is required but Cloud service charges may be applicable.
Such queries are like −

1. All SHOW commands
2. MIN, MAX but limited to only Integer/Number/Date data types of columns.
3. COUNT

**Query Result Caching**

Query Results are stored and managed by Cloud Service Layer. It is very useful if the same query run multiple times, but condition is underlying data or base tables are not changed between time duration when query has to run multiple times. This caching has unique feature that is available for other users within the same account.
For example, if user1 runs a query first time, the result gets stored in caching. When user2 also tries to run same query (by assuming that base tables and data are not changed), it fetches the result from Query Result caching.

**2nd Time Execution** (Reading directly from Query Results)

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/1ddb3744-c096-47ef-a334-b01da505b4e0)

Result cached are available for 24hours. But, counter of 24hours get reset each time when the same query re-run. For example, if a query ran at 10AM, its caching will be available until 10AM next day. If the same query re-run at 2PM on same day, now the caching will be available until 2PM next day.
There are some criteria to fulfil to use query result cache –
1. Exact same SQL query should be re-run.
2. There should not be any random function in the SQL.
3. User must have right permissions to use it.
4. Query result should be enabled while running the query. By default, it's enabled until set otherwise.
Some cases for Query result caching are −
1. Queries those required massive amount of computing like Aggregate function and semi structured data analysis.
2. Queries those run very frequently.
3. Queries those are complex.
4. Refactor the output of another query like "USE TABLE function RESULT_SCAN (<query_id>)"
   
Login into Snowflake and go to Worksheets. Resume the warehouse by running following query −

**ALTER WAREHOUSE COMPUTE_WH Resume;**

Now, run following queries sequentially –

**USE SCHEMA SNOWFLAKE_SAMPLE_DATA.TPCH_SF100;**

SELECT l_returnflag, l_linestatus,

SUM(l_quantity) AS sum_qty,

SUM(l_extendedprice) AS sum_base_price,

SUM(l_extendedprice * (l_discount)) AS sum_disc_price,

SUM(l_extendedprice * (l_discount) * (1+l_tax)) AS sum_charge,

AVG(l_quantity) AS avg_qty,

AVG(l_extendedprice) AS avg_price,

AVG(l_discount) AS avg_disc,

COUNT(*) AS count_order

FROM lineitem

WHERE l_shipdate <= dateadd(day, 90, to_date('1998-12-01'))

GROUP BY l_returnflag, l_linestatus

ORDER BY l_returnflag, l_linestatus;

Click the Query Id. It will display the link of query Id. Then click on link as shown in previous example (Metadata-Caching). Check the Query profile, it will be displayed as shown below –
 
It shows 80.5% data is scanned so no cache was involved. Suspend the warehouse by running following query −

**ALTER WAREHOUSE COMPUTE_WH Suspend;**

Run the same query again as we previously did −

**USE SCHEMA SNOWFLAKE_SAMPLE_DATA.TPCH_SF100;**

SELECT l_returnflag, l_linestatus,

SUM(l_quantity) AS sum_qty,

SUM(l_extendedprice) AS sum_base_price,

SUM(l_extendedprice * (l_discount)) AS sum_disc_price,

SUM(l_extendedprice * (l_discount) * (1+l_tax)) AS sum_charge,

AVG(l_quantity) AS avg_qty,

AVG(l_extendedprice) AS avg_price,

AVG(l_discount) AS avg_disc,

COUNT(*) AS count_order

FROM lineitem

WHERE l_shipdate <= dateadd(day, 90, to_date('1998-12-01'))

GROUP BY l_returnflag, l_linestatus

ORDER BY l_returnflag, l_linestatus;

Click the Query Id. It will display the link of query Id. Then click on link as shown in previous example (Metadata-Caching). Check the Query profile, it will be displayed as shown below −
 
It shows query result reuse. It means that without warehouse query it ran successfully and entire result set has been taken from Query Result Caching.
