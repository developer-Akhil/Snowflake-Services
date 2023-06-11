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



