---
author: steven.murawski@gmail.com
categories:
- 2012
- Systems Administration
- Windows Server
comments: true
date: 2012-10-01T00:00:00Z
tags: []
title: Server 2012–Continuously Available File Shares

---

I think SMB3 and the enhancements to the clustered file share roles are some of the most important new features in Server 2012.



&#160;



There are some great new features in file shares and the SMB protocol.&#160; Some of the terminology can be confusing, so I’m going to loosely define a few of them right up front.



**SMB3 – **Revised SMB protocol with improved performance, scaling, and support for technology like RSS and RDMA.&#160; It was introduced with Windows 8 and Server 2012. 



**Highly Available File Share –** A clustered file share.&#160; This can be made Continuously Available.



**Continuously Available File Share –** A file share with Transparent Failover enabled.&#160; This can be a Highly Available File Share or a Scale Out File Share. 



**Scale-Out File Share –** A file share optimized for application workloads like SQL Server and Hyper-V.&#160; Scale-Out File Shares are Continuously Available.



&#160;



## What Makes a File Share Continuously Available?




There are a couple of key features in this scenario.



1.  Transparent Failover 
2.  Witness Service 


### Transparent Failover




Transparent Failover is the capability of an SMB3 client to maintain a connection with data located on a remote (Server 2012) clustered file share despite the transition of the role between cluster nodes (planned or unplanned).&#160; This capability allows for more seamless access to documents and application data of all types.



### Witness Service




The Witness Service coordinates the access to a clustered Continuously Available File Share.&#160; When an SMB3 client connects to a Continuously Available File Share, it notifies the Witness Service on the cluster, which picks a different cluster node to be the witness for this SMB connection.&#160; This node then has the responsibility to help transition the SMB3 client to the new host for the role in the case of a failover, without requiring the client to wait for TCP timeouts.&#160; 



&#160;



## The Difference Between Continuously Available File Shares and Scale-Out File Shares




I touched on this earlier, but this is an important distinction that has definite performance impacts.&#160; All Scale-Out File Shares are Continuously Available, but not all Continuously Available File Shares are Scale-Out.



Scale-Out File Shares target certain workloads, like hosting Hyper-V virtual machine files or SQL Server Databases.&#160; The disk access for these file shares is optimized for files that are being held open and continually accessed.&#160; They leverage Cluster Shared Volumes to provide access to the same data across multiple cluster nodes.&#160; Additionally, Scale-Out File Shares do not have their own IP address associated with their client access point.&#160; The Scale-Out File Share registers all of the IPs (that the cluster allows client connections on) in DNS with the client access point name.&#160; The SMB3 client uses DNS round-robin to initially distribute load to the file servers.&#160; Scale-Out File Shares are limited to a maximum 8 nodes in a cluster.



Other Continuously Available File Shares operate as more traditional file shares, but with the added benefit of quicker (seamless) failover from cluster node to cluster node.&#160; These more traditional file shares have their own IP address associated to the client access point name.&#160; These shares can be used for any other file sharing purpose, with data/disk access mirroring more traditional modes.&#160; The disk for these file shares is cluster disk that is accessible only from the cluster node hosting it.



### Lesson Learned




 I’m emphasizing the difference in these two for a reason.&#160; In my initial deployments and testing, I didn’t really have a clear understanding of the difference.&#160; I thought that a Scale-Out File Share was the best all-around option, after all, every node was a potential endpoint so the performance had to be great.&#160; That didn’t happen in practice.



My VM’s that I had hosted on a Scale-Out File Share were performing great.&#160; The other workloads I targeted were not going so well.&#160; I had initially deployed a file share to be the target for SQL Server backups as a Scale-Out File Share.&#160; It turned out that the data access patterns of generating SQL backups was not really in line with the intended usage for the Scale-Out File Share and the performance was abysmal.&#160; Converting that to a Continuously Available Share improved the performance instantly and measurably (no change to the backend disk, still on the same servers – the only change was the type of file share and how the disk was presented to the cluster – no longer as a Cluster Shared Volume, but as a cluster disk).&#160;&#160; 



The flip side is also true.&#160; If I were to host my VM’s on a regular Continuously Available File Share, I would see performance degradation.&#160; The performance would still be superior to a 2008 R2 or other SMB file share, since SMB3 is a more efficient protocol and allows for better throughput, but the optimizations for disk access would be lost.



By going with the Continuously Available File Share and not the Scale-Out File Share for my backups, I did not lose any reliability.&#160;&#160; They are both highly available and have the Continuous Availability/Transparent Failover capability.



## How This Is Revolutionizing My Data Center…




Continuously Available File Shares are transforming my data center.&#160; I am able to host virtual machines, SQL databases, IIS application directories, and push more shared files onto file share clusters than ever before.&#160;&#160; By hosting my application workloads (VMs, SQL, etc..) on a file share, I reduce my dependence on any one particular storage method.&#160; I’m able to add a layer of indirection that allows my application servers to remain ignorant of the storage layer, which makes deploying new application and infrastructure servers much faster.



Advances in networking technologies (for example RDMA), allow for throughputs that are off the charts and disk speed is the limiting factor. (In some of my testing, I was getting 4GB, yes GIGABYTES, a second across Infiniband connections.&#160; And that was due to the file writes for the test being larger than the cache on the disk and requiring actual disk access for the test.&#160; Stuff that could be handled in the cache or in memory were even faster.)



In the year that I have had Windows Server 2012 in production (through it’s various previews, betas, and RCs), SMB3, Continuously Available File Shares, and Scale-Out File Shares have proven to be my FAVORITE thing.

