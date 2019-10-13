---
layout: post
title:  "How to Set Up a Static Website on AWS using S3, CloudFront and Route 53"
excerpt: Hosting a static web site on Amazon S3 is secure, cheap and convenient.
date:   2019-10-11 09:35:15 -0700
categories: cloud
tags: aws
---

![aws]({{ site.url }}/assets/posts_images/flickr-teezeh-aws.jpg)
Photo by [Thomas Cloer](https://www.flickr.com/photos/teezeh/15670725648/)

## Background
Being able to host a static web site on Amazon S3 is secure, cheap and robust. I will take you through the steps in setting up a static, secure web site in Amazon AWS. One point I would like to emphasize is that `example.com` should redirect to `www.example.com` and `http` should redirect to `https`. Most articles point out the `http` to `https` redirection but some don't mention the redirection from `example.com` to `www.example.com` (and vice-versa).

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
    * `www.example.com`
        * html/css/js files are located here
    * `example.com`
        * redirection to www S3 bucket
3. Create one certificate
    * will contain both `example.com` and `www.example.com` domains
4. Create two CloudFront distributions
    * required for `https`
5. Configure Route 53 hosted zone

## Steps
### Purchase (or transfer domain) to Route 53

### Create two S3 buckets
S3 >> Properties >> Static website hosting  

Endpoint : `http://www.pulseoftheland.com.s3-website-us-west-1.amazonaws.com`
![S3 www Config]({{ site.url }}/assets/posts_images/s3-www.png){:width="500px"}

Redirect requests (pulseoftheland.com)  
![S3 Config]({{ site.url }}/assets/posts_images/s3-no-www.png){:width="500px"}

### Create one certificate
Certificate Manager >> Request a certificate  

Certificate settings (`www.pulseoftheland.com` and `pulseoftheland.com`)  
![Certificate Manager Config]({{ site.url }}/assets/posts_images/cm-potl.png){:width="500px"}

### Create two CloudFront distributions
CloudFront >> Create Distribution >> Web  

Distribution settings (www.pulseoftheland.com):  
![CloudFront Config]({{ site.url }}/assets/posts_images/cf-dist-www.png){:width="500px"}

Origin settings (`www.pulseoftheland.com`):  
![CloudFront Config]({{ site.url }}/assets/posts_images/cf-origin-www.png){:width="500px"}

Distribution settings (`pulseoftheland.com`):
![CloudFront Config]({{ site.url }}/assets/posts_images/cf-dist-no-www.png){:width="500px"}

Origin settings (pulseoftheland.com):  
![CloudFront Config]({{ site.url }}/assets/posts_images/cf-origin-no-www.png){:width="500px"}

### Configure Route 53
![CloudFront Config]({{ site.url }}/assets/posts_images/r53-www.png){:width="500px"}

![CloudFront Config]({{ site.url }}/assets/posts_images/r53-no-www.png){:width="500px"}

## Verification
Use curl from the command line to check that a `301` redirect happens from `example.com` to `www.example.com`:
```shell
curl -v www.example.com
```
Output:
```shell
* Rebuilt URL to: pulseoftheland.com/
*   Trying 99.84.224.13...
* TCP_NODELAY set
* Connected to pulseoftheland.com (99.84.224.13) port 80 (#0)
> GET / HTTP/1.1
> Host: pulseoftheland.com
> User-Agent: curl/7.58.0
> Accept: */*
>
< HTTP/1.1 301 Moved Permanently
< Content-Length: 0
< Connection: keep-alive
< Date: Sun, 13 Oct 2019 20:25:13 GMT
< Location: https://www.pulseoftheland.com/
```