---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# AWS Architecture Blog | What I learned from S&P Global’s Disaster Recovery strategy

> **Original article:** *S&P Global’s innovative disaster recovery strategy using Amazon FSx for NetApp ONTAP snapshots*  
> **Authors:** AWS Architecture Blog  
> **Published:** AWS Official Blog

## Why I wrote this post

While learning more about AWS, I wanted to understand Disaster Recovery (DR) through a real production system rather than treating it as a theoretical concept.

I found an interesting AWS Architecture Blog describing how S&P Global Market Intelligence designed a Disaster Recovery solution for the Capital IQ platform by using **Amazon FSx for NetApp ONTAP**.

Before reading this article, I mainly thought Disaster Recovery meant creating backups and restoring data when failures occurred. After reading it, I realized that a complete DR strategy involves architecture design, replication, failover planning, and minimizing downtime.

![S&P Global Disaster Recovery Architecture](/images/blog1/sp-global-dr-architecture.png)

*Figure 1. Disaster Recovery architecture of S&P Global using Amazon FSx for NetApp ONTAP (Source: AWS Architecture Blog).*

## Article overview

The article explains how **S&P Global Market Intelligence** implemented a Disaster Recovery solution for the Capital IQ platform.

The primary objective is to keep business services available even if the primary AWS Region experiences an outage.

One of the most impressive aspects is that the platform can switch to a **read-only** Disaster Recovery environment in **less than 15 minutes**. When necessary, it can later be promoted into a **read-write** environment to fully restore operations.

---

## Architecture

The solution uses a **multi-Region architecture**.

- **US-East-1** is the Primary Region.
- **US-West-2** is the Disaster Recovery Region.
- SQL Server runs on Amazon EC2 instances with Windows Server Failover Cluster.
- Each Region contains an Amazon FSx for NetApp ONTAP file system.
- Data is replicated from the Primary Region to the DR Region using **SnapMirror**.
- **FlexClone** creates a cloned volume from the latest replicated snapshot, allowing quick access to data in the DR Region.

---

## Key components

### Amazon FSx for NetApp ONTAP

Amazon FSx for NetApp ONTAP is a managed file storage service that provides enterprise storage features such as snapshots, replication, cloning, and high availability.

### Snapshots

Snapshots capture the state of data at a specific point in time. They provide a reliable recovery point without interrupting the running application.

### SnapMirror

SnapMirror continuously replicates data from the Primary Region to the Disaster Recovery Region. The article explains that replication is configured to keep Recovery Point Objective (RPO) very low.

### FlexClone

FlexClone is the feature that interested me the most.

Instead of rebuilding storage from scratch, FlexClone creates an almost instant writable copy from an existing snapshot. This significantly reduces recovery time and allows users to access data much sooner.

---

## Recovery process

The Disaster Recovery strategy contains two major stages.

1. **Rapid read-only failover**

   - Create a FlexClone volume from the latest replicated snapshot.
   - Quickly provide users with read-only access to critical business data.

2. **Read-write recovery**

   - Stop write operations in the Primary Region.
   - Perform a final SnapMirror synchronization.
   - Break the replication relationship.
   - Promote the cloned storage to become the active production environment.

This approach allows users to continue accessing important information while engineers complete the full recovery procedure.

---

## What I learned

As a student learning AWS, this article helped me realize that cloud architecture is much more than deploying applications.

When designing a system, I should always consider questions such as:

- What happens if the system fails?
- How much data could be lost?
- Can users still access critical information?
- How quickly should the service recover?
- Has the Disaster Recovery process been tested?

I also learned that Disaster Recovery combines architecture, storage, replication, operations, and business continuity rather than relying only on backups.

---

## Conclusion

Although I still need to learn more about services such as Amazon FSx for NetApp ONTAP, SnapMirror, and FlexClone, this article provided an excellent real-world example of Disaster Recovery architecture on AWS.

It showed how cloud services can be combined to build highly available systems that minimize downtime while protecting business-critical data.

---

## Reference

**Original article:** [S&P Global’s innovative disaster recovery strategy using Amazon FSx for NetApp ONTAP snapshots](https://aws.amazon.com/blogs/architecture/sp-globals-innovative-disaster-recovery-strategy-using-amazon-fsx-for-netapp-ontap-snapshots/)