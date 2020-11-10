---
layout: post
title:  "Journey Through the Cloud PT 3"
date:   2020-11-07
image: /images/posts/awspipeline.png
tags: [Series, Christopher L Medina, AWS, OAI, S3, Lambda@Edge, CodePipeline, CodeBuild, CodeDeploy, CodeCommit, CI/CD]
---

I’m sure you’ve heard the terms thrown around enough, but what does CI/CD really mean? What roles are each responsible for? Maybe we can help clear some of that up in today’s blog post. Today we’re going to be discussing Continuous Integration (CI) and Continuous Delivery (CD) at high levels. We’ll discuss their native counterparts in AWS, and why we decided to use them for our production workflow. Additionally, we’ll discuss our wishlist and what we plan on doing in the future in regards to our established pipeline.

<!--more-->

## CI/CD Primer

Much like the phrase DevOps, CI/CD is more a philosophy than a tech stack. These lines get more blurred each year with well designed, efficient product suites becoming synonymous with the practice. At its core Continuous Integration and Continuous Delivery are practices leveraged by developers to ensure a healthy and managed life cycle of code development. 

The role of CI is to produce artifacts, or outputs of your code (binary files, jars, etc.) for deployment. During this process workflow tests are run to make sure that the artifact for the given code is safe to deploy. This is key for the overall CI/CD process. 

When it comes time to introduce CD, code changes are continuously deployed. Throughout this process deployments are triggered manually. Continuous Deployments (not to be confused with the Continuous Delivery method) occurs when the entire process of moving code from source to destination is completely automated. Soup to nuts as they say. 

## AWS CI/CD Services

Within the fleet of AWS Developer Tools managed services there are a few that stand out. Here will talk about some of the services that are relevant to the purpose of this post.

<b>AWS CodeCommit</b>

AWS CodeCommit is AWS’ fully-managed source control service. It’s a git-based repository service akin to that of GitHub, etc. There once was a big benefit here, back before GitHub allowed unlimited free private repositories. Still if you want to be exclusively in the AWS ecosystem it’s a pretty full featured rich git repository service.

<b>AWS CodeBuild</b>

AWS CodeBuild is AWS’ fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy. CodeBuild is a full featured, easy to configure CI service. 

<b>AWS Code Deploy</b> 

AWS CodeDeploy is a fully managed deployment service that automates software deployments to a variety of different compute services such as EC2, Fargate, Lambda, and even on-premise servers. 

<b>AWS CodePipeline</b>

AWS CodePipeline is AWS’s fully managed continuous delivery service that helps us automate our release pipelines for fast and reliable application and infrastructure updates. CodePipeline can be used to automate build, test, and deploy phases of our release process every time there is a code change.

Much like most same vendor product sweets working with these services in tandem I’ve found to be quite pleasant. All of these services integrate very well together, which could be a decision point in leveraging services like AWS CodeCommit over GitHub, etc.

## Master The Cloud

As we’ve stated previously we’ve decided to split our services into different frameworks. Each of these frameworks will be managed via different workflows. For the purpose of this site we decided to keep our workflow as simple as possible. Our goals were to set up a workflow that enabled us to push commits to a branch and have that kickstart our Jekyll build. Lastly, we want that output from the Jekyll build to be stored in a source that our CloudFront distribution is pointing to. I suggest you read the previous blogs in this series to get a clear understanding of our desired endstate (if you haven’t already read them). In order to accomplish all of this, we took some short cuts. Our pipeline consists of only AWSCodepipeline and AWS Codebuild. This workflow is for a simple blog aware static site, we’re not running any test along the way. We write the copy, have it proofed, test it locally on our dev boxes, and if it looks good enough we push from a short lived branch to the master branch. From there AWS CodePipeline kick starts and triggers AWS CodeBuild where our site is built. As part of the final stages in the build process it runs a simple S3 Sync on to our designated bucket. 

In a more enterprise or ‘real world’ scenario there would need to be much more precautionary steps. Approval process, different environments, etc. Our team is quite small, and while our current solution doesn’t scale well it will work for the time being.

As our team grows we do plan on making this a bit more streamlined. Particularly when we’re in the position to hire a full time editor. We’ll probably have another environment that we will deploy into first. We’ll use CodePipeline for a manual approver process once the editor okayes everything. Additionally, this other location will probably be an S3 bucket with ACL policies restricting get request by IPs. We’ll be sure to update you along the process as we change it.

Be sure to join us next time when I walk through the IaC portion of this initiative.

As always, thanks for reading!

> <cite>- FIN -</cite>
> <cite>Christopher L Medina</cite>
> <cite>Solutions Architect - Masterthe.Cloud</cite>