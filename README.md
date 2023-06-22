# Snowflake-Services

# Snowflake Database Architecture

![image](https://github.com/developer-Akhil/Snowflake-Services/assets/64408106/af24d875-2011-4246-8913-a940f26f89dd)

**Service Layer**: In Snowflake, the Service Layer is a critical component of the architecture that handles various functionalities such as query parsing, optimization, execution, transaction management, security, and metadata management. It acts as an intermediary between the user and the underlying storage and compute layers of Snowflake.  Logically, this can be assumed to hold the result cache – a cached copy of the results of every query executed.

**Compute Layer**:  Which actually does the heavy lifting.  This is where the actual SQL is executed across the nodes of a Virtual Data Warehouse.  This layer holds a cache of raw data queried, and is often referred to as Local Disk I/O although in reality this is implemented using SSD storage.  All data in the compute layer is temporary, and only held as long as the virtual warehouse is active.
Compute Layer is responsible for executing queries and processing data. It is the component that performs the actual computation and analysis of data stored in Snowflake's distributed storage layer.

**Storage Layer**:  Which provides long term storage of results.  This is often referred to as Remote Disk, and is currently implemented on either Amazon S3 or Microsoft Blob storage.

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


