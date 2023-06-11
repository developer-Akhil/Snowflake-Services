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
**Standard(default) and Economy**
**Standard Scaling Policy**
The Standard scaling policy is designed for workloads with consistent performance requirements and is suitable for most production environments. It provides dedicated compute resources to a virtual warehouse, ensuring consistent performance regardless of the overall system load.
**Under the Standard scaling policy:**
	The virtual warehouse is allocated a fixed number of compute resources, including CPU, memory, and storage.
	The allocated resources are reserved exclusively for the virtual warehouse, providing predictable and stable performance.
	The compute resources remain allocated and available to the virtual warehouse even when it's idle, ensuring immediate availability for query execution.
The Standard scaling policy is recommended when you have critical workloads or when you require predictable performance and consistent query response times.
**Economy Scaling Policy:**
The Economy scaling policy is a cost-saving option that dynamically shares compute resources among multiple virtual warehouses. It is suitable for non-production environments, such as development, testing, and ad-hoc analytics, where consistent performance may not be a top priority.
**Under the Economy scaling policy:**
	Compute resources are shared across multiple virtual warehouses within a Snowflake account.
	Compute resources are dynamically allocated and released based on the workload demands of each virtual warehouse.
	When a virtual warehouse is idle, its compute resources are automatically released and made available to other virtual warehouses.
	When workload demand increases, compute resources are dynamically allocated to virtual warehouses as needed.


