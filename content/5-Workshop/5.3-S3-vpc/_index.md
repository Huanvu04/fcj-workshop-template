---
title : "Deploy the frontend with Amazon S3"
date : 2024-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

### Overview
The TaskManager frontend is a static web application. After the build step, HTML, JavaScript, CSS, and SVG files are uploaded to the `taskmanager-frontend-dev-*` Amazon S3 bucket.

In a production architecture, the S3 bucket should stay private and be served through CloudFront. In this workshop, we focus on validating that the frontend artifacts were uploaded successfully to S3.

![Image](/images/workshop/s3-upload.png)

### Content
* Upload frontend assets to S3
* Validate the frontend upload result