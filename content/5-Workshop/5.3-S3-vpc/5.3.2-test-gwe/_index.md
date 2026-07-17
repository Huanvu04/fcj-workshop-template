---
title : "Validate the frontend upload result"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

### Goal
Verify that the frontend artifacts exist in the S3 bucket and are ready to be integrated with CloudFront or the deployment pipeline.

### Check the file list
In the **Files and folders** tab, confirm that the bucket contains:
* `index.html`
* `favicon.svg`
* `icons.svg`
* `assets/index-*.js`
* `assets/index-*.css`

![Image](/images//workshop/s3-upload.png)

### File meaning
* `index.html`: entry point of the single-page application.
* `assets/index-*.js`: bundled JavaScript containing UI logic and API calls.
* `assets/index-*.css`: frontend styling.
* `favicon.svg` and `icons.svg`: static icon assets.

### Conclusion
The frontend was uploaded successfully. Next, we validate the authentication layer, GraphQL API, and Lambda backend used by the frontend.