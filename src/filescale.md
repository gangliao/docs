## FileScale

> Fast and Elastic Metadata Management for Distributed File Systems

Recent work has shown that distributed database systems are a promising solution for scaling metadata management in scalable file systems. This work has shown that systems that store metadata on a single machine, or over a shared-disk abstraction, struggle to scale performance to deployments including billions of files. In contrast, leveraging a scalable, shared-nothing, distributed system for metadata storage can achieve much higher levels of scalability, without giving up high availability guarantees. However, for low-scale deployments -- where metadata can fit in memory on a single machine -- these systems that store metadata in a distributed database typically perform an order of magnitude worse than systems that store metadata in memory on a single machine. This has limited the impact of these distributed database approaches, since they are only currently applicable to file systems of extreme scale.

**FileScale** is a disaggregated architecture that incorporates a distributed database system as part of a comprehensive approach to metadata management in distributed file systems. In contrast to previous approaches, the architecture described in the paper performs comparably to the single-machine architecture at small scale, while enabling linear scalability as the file system metadata increases.

FileScale's architectural design enables file system scalability with far less efficiency costs, so that it can be used from the early stages of an application up through the later stages as the application scales over time.  We packages up code and all its dependencies in the image file (i.e. AWS EC2 AMI and Docker) for users can quickly and reliably reproduce our storage system from one computing environment to another. FileScale includes around 38K LoC changed on HDFS 3.2.0. The code is freely available:

```shell
git clone https://github.com/DSLAM-UMD/FileScale
```
