---
layout: post
title:  "How to Set Up a Static Website on AWS using S3, CloudFront and Route 53"
date:   2019-10-11 09:35:15 -0700
categories: cloud
tags: [aws]
---
![aws]({{ site.url }}/assets/flickr-teezeh-aws.jpg)
Photo by [Thomas Cloer](https://www.flickr.com/photos/teezeh/15670725648/)

## Background
Being able to host a static web site on Amazon S3 is secure, cheap and convenient. I will take you through the steps in setting up a static, secure web site. Before doing so I would like to emphasize one assumption which is that example.com should redirect to www.example.com and http should redirect to https.

## Services
In order to set up a static website via AWS the following services will be used:
* [S3](https://aws.amazon.com/s3/)
> Object storage built to store and retrieve any amount of data from anywhere 

* [CloudFront](https://aws.amazon.com/cloudfront/)
> Fast, highly secure and programmable content delivery network (CDN)

* [Route 53](https://aws.amazon.com/route53/)
> A reliable and cost-effective way to route end users to Internet applications

* [Certificate Manager](https://aws.amazon.com/certificate-manager/)
> Easily provision, manage, and deploy public and private SSL/TLS certificates for use with AWS services and your internal connected resources

## Overview
The process for setting up a static website with SSL is:
1. Purchase (or transfer domain) to Route 53
2. Create two S3 buckets
    * www.example.com
    * example.com
3. Create one certificate
4. Create two CloudFront distributions
5. Configure Route 53

## Steps
### Purchase (or transfer domain) to Route 53

### Create two S3 buckets
S3 >> Properties >> Static website hosting  

Endpoint : http://www.pulseoftheland.com.s3-website-us-west-1.amazonaws.com (www.pulseoftheland.com)  
![S3 www Config]({{ site.url }}/assets/s3-www.png)

Redirect requests (pulseoftheland.com)  
![S3 Config]({{ site.url }}/assets/s3-no-www.png)

### Create one certificate
Certificate Manager >> Request a certificate  

Certificate settings (www.pulseoftheland.com and pulseoftheland.com)  
![Certificate Manager Config]({{ site.url }}/assets/cm-potl.png)

### Create two CloudFront distributions
CloudFront >> Create Distribution >> Web  

Distribution settings (www.pulseoftheland.com):  
![CloudFront Config]({{ site.url }}/assets/cf-dist-www.png)

Origin settings (www.pulseoftheland.com):  
![CloudFront Config]({{ site.url }}/assets/cf-origin-www.png)

Distribution settings (pulseoftheland.com):
![CloudFront Config]({{ site.url }}/assets/cf-dist-no-www.png)

Origin settings (pulseoftheland.com):  
![CloudFront Config]({{ site.url }}/assets/cf-origin-no-www.png)

### Configure Route 53


## Verification
curl -v www.example.com