---
title: "Blog 3"
date: 2024-01-02
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP – Insights from AWS Architecture Blog
> **Original Article:** *Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP*  
> **Source:** AWS Architecture Blog

## Why I Chose This Article

While exploring large-scale storage solutions and database management on the cloud, ensuring High Availability (HA) for enterprise systems like Oracle Database has always been a significant challenge.

This article caught my attention because it solves a complex infrastructure problem not by using a single isolated service, but through a well-orchestrated combination of multiple AWS services to build an automated, self-healing architecture. I chose to review this post to consolidate my understanding and connect it deeply with the storage and infrastructure services I have been working with during my internship.

![Oracle Database High Availability Architecture with Amazon FSx for NetApp ONTAP](/images/blog3/archblog.png)

*Figure 1. Self-healing architecture for Oracle Database using Amazon FSx for NetApp ONTAP. (Source: AWS Architecture Blog)*

## First Impressions

Initially, I expected the article to be a standard walkthrough on how to install and configure an Oracle Database instance on Amazon EC2.

However, as I dove deeper, I realized it targets a core enterprise pain point: maintaining business continuity and establishing automated Disaster Recovery (DR) with zero manual intervention, while guaranteeing absolute data integrity through real-time synchronous replication.

## Core Architectural Components

The architecture leverages an optimized mix of flexible compute and premium enterprise storage solutions:

### 1. Amazon FSx for NetApp ONTAP (FSxN) Multi-AZ
- Acts as the centralized storage layer for all Oracle Database files.
- Connects to EC2 instances using the industry-standard iSCSI protocol.
- Features **Synchronous Replication** between two Availability Zones (AZ1 and AZ2), ensuring that data is always consistent and protected against full AZ failures.

### 2. EC2 Auto Scaling Group (Min=1, Max=1)
- Enforces that exactly one Oracle DB instance runs at all times. If the Active DB instance in AZ1 fails, the Auto Scaling Group automatically provisions a Replacement instance in AZ2.

### 3. AWS Backup & Automation Pipeline (EventBridge + Lambda + SSM)
- **AWS Backup** performs periodic backups and emits a completion event.
- **Amazon EventBridge** intercepts this event and triggers an **AWS Lambda** function.
- **AWS Lambda** updates the latest AMI ID inside the **AWS Systems Manager (SSM) Parameter Store**.
- The Auto Scaling Group's Launch Template dynamically references (`resolve:ssm`) this Parameter Store to ensure that the replacement server alwaysboots from the latest, fully patched system image.

## Key Learnings on RTO and RPO

The most valuable takeaway for me from this architecture is understanding how to properly optimize the two critical metrics in resilient system design:

- **RPO (Recovery Point Objective):** Thanks to the Multi-AZ Synchronous Replication provided by FSx for NetApp ONTAP, data writes are committed simultaneously in both zones, pushing the RPO down to near-zero (no data loss during a failover).
- **RTO (Recovery Time Objective):** The automated pipeline comprising EventBridge -> Lambda -> SSM coupled with the Auto Scaling Group minimizes downtime. Instead of requiring manual re-configuration, the replacement instance in AZ2 launches automatically, maps back to the existing storage fabric via iSCSI, and brings the database back online swiftly.

## Conclusion

The article *Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP* provides a highly practical and valuable perspective on cloud architecture best practices. It highlights that true high availability is not just about making routine backups, but rather about seamlessly integrating synchronous replication, infrastructure automation, and centralized configuration management.

## References

**Reference Link:** [Building Highly Available Oracle Databases with Amazon FSx for NetApp ONTAP](https://aws.amazon.com/vi/blogs/architecture/building-highly-available-oracle-databases-with-amazon-fsx-for-netapp-ontap/)