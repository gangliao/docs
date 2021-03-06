# HDFS Architecture

HDFS is a distributed file system from Hadoop designed for storing very large files running on a cluster of commodity hardware. It is designed on principle of storage of less number of large files rather than the huge number of small files. HDFS has 2 types of nodes that work in the cluster. They are Namenode(s) and Datanodes in master-slave fashion.

<img src="https://raw.githubusercontent.com/DSL-UMD/docs/master/src/img/hdfs-arch.png" class="center" style="width: 80%;" />

<span class="caption">Figure 1: System Architecture of Hadoop HDFS.</span>

## Namenode

Namenode allocates files access to the clients. It maintains and manages the directory tree, relations among files, blocks and datanodes, and assigns tasks to datanodes. Namenode executes file system namespace operations like opening, closing, and renaming files and directories. When a client asks for a data access in HDFS, first of all, NameNode searches for the data in that datanodes, in which data is quickly available. And then provides access to that data to the client. HDFS Namenode itself makes data availability easy to the clients by providing the address of the datanode from where a user can directly read.


## Datanode

Datanodes manage storage of data. They serve read and write requests from the file system's clients. They also perform block creation, deletion, and replication upon instruction from the namenode. Once a block is written on a datanode, it replicates it to other datanode and process continues until the number of replicas mentioned is created. After the definite interval of time, all these datanodes sends heartbeat messages to the namenode. If the namenode stops receiving heartbeat messages from any of these datanodes, then it assumes it to be dead. After that, it checks for the data present in those nodes and then gives commands to the other datanode to create a replica of that data to other datanodes. Therefore data is always available.

## High Availability

Hadoop HDFS is a distributed file system. HDFS distributes data among the nodes in the Hadoop cluster by creating a replica of the file. Hadoop framework stores these replicas of files on the other machines present in the cluster. When a client wants to access his data, he can easily access that data from a number of machines present in the cluster. Data is easily available in the closest node in the cluster. At some unfavorable conditions like a failure of a node, the client can easily access their data from the other nodes.

## Checkpointng

[Checkpointing](https://blog.cloudera.com/blog/2014/03/a-guide-to-checkpointing-in-hadoop/) is an essential part of maintaining and persisting filesystem metadata in HDFS. It’s crucial for efficient NameNode recovery and restart, and is an important indicator of overall cluster health.
At a high level, the NameNode’s primary responsibility is storing the HDFS namespace. This means things like the directory tree, file permissions, and the mapping of files to block IDs. It’s important that this metadata (and all changes to it) are safely persisted to stable storage for fault tolerance. HDFS metadata represents the structure of HDFS directories and files in a tree in memory of Namenode. Persistence of HDFS metadata broadly breaks down into 2 categories of files:

- `FSImage` in Namenode is an "Image file" which contains the entire filesystem namespace and is stored as a file in Namenode's local file system. It also contains a serialized form of all the directories and children inodes in the filesystem. Each inode is an internal representation of a file or directory's metadata;

- `EditLogs` contains all the recent modifications made to the file system on the most recent FSImage. Namenode receives a create/update/delete request from the client. After that this request is first recorded to edits file. This way, if the NameNode crashes, it can restore its state by first loading the FSImage then replaying all the operations (also called edits or transactions) in the edit log to catch up to the most recent state of the namesystem.  

**Note**: FSImage is a file that represents a point-in-time snapshot of the filesystem’s metadata. However, while the fsimage file format is very efficient to read, it’s unsuitable for making small incremental updates like renaming a single file. Thus, rather than writing a new fsimage every time the namespace is modified, the NameNode instead records the modifying operation in the edit log for durability.

<img src="https://raw.githubusercontent.com/DSL-UMD/docs/master/src/img/checkpointing.png" class="center" style="width: 70%;" />

<span class="caption">Figure 2-2: Checkpointing creates a new fsimage from an old fsimage and edit log.</span>

 Checkpointing is the process of merging the content of the most recent FSImage with all edits applied (EditLogs) after that FSImage is merged in order to create a new FSImage. Checkpointing is triggered automatically by configuration policies or manually by HDFS administration commands.



