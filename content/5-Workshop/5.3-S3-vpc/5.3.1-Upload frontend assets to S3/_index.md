---
title : "Upload frontend assets to S3"
date : 2024-01-01 
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

### Goal
Upload the TaskManager frontend build output to Amazon S3 to prepare the web interface for delivery.

### Steps
1. Build the frontend locally or through CI/CD.
2. Open the Amazon S3 console in the `ap-southeast-1` Region.
3. Select the frontend bucket named `taskmanager-frontend-dev-*`.
4. Choose **Upload** and add the build output files.
5. Confirm that the upload succeeds.

### Result
The bucket contains 5 frontend files:
* `index.html`
* JavaScript bundle in the `assets/` folder
* CSS bundle in the `assets/` folder
* `favicon.svg`
* `icons.svg`

![Image](images/workshop/s3-upload.png)

The screenshot shows **Succeeded: 5 files, 611.4 KB (100%)**, confirming that the frontend artifacts were uploaded to S3 successfully.

### Security note
When this bucket is connected to CloudFront, it should not be publicly readable. Use CloudFront Origin Access Control or Origin Access Identity so only CloudFront can read objects from the bucket.