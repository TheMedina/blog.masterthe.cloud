---
layout: post
title:  "Journey Through the Cloud PT 4"
date:   2020-11-10
image: /images/posts/terraform.png
tags: [Series, Christopher L Medina, IAC, Terraform, CloudFormation, ARM, CDK]
---

Today we’re going to be covering one of the most fundamental skill sets when it comes to cloud computing, Infrastructure as Code (IaC). Regardless of which public cloud provider you’re using I would strongly recommend that you have a good understanding of what IaC is and how to use it. In today’s article we’ll be discussing the various IaC tool sets and what some pros and cons are. Additionally, we’ll explain why we chose to go with our IaC of choice for blog.masterthe.cloud.

<!--more-->

## Infrastructure as Code Primer

Infrastructure as Code, otherwise referred to as IaC, is a set of simplistic, verbose and descriptive languages / tool sets used to provision requested resources from public cloud providers (AWS, Azure, GCP, etc.). They are very much declarative in nature and are devoid of the logic typically found in more traditional programming languages. This in turn makes them easier for both developers and system engineers / system administrators to learn. 

Because IaC code bases consist of simple flat files, they can be managed in traditional code repositories and be treated with the same development practices already in place within your organization. Things like peer reviewing, branch merging, and continuous integration, can all be done with IaC.

While there are a lot of benefits of IaC I’d like to emphasize the two most impactful benefits I’ve experienced, speed and repeatability. Regardless of which public cloud provider you’re using it will prove faster to provision requested resources in bulk via IaC then through the providers given web console. On a similar note, your ability for consistent and reliable infrastructure provisioning via IaC is unparalleled when compared to manual deployment.

There are several different tools and languages available when it comes to IaC most of which are vendor specific. Below we’ll go over the more popular ‘cloud native’ ones and what their strengths and weaknesses are.

## AWS CloudFormation

One of the first IaC languages / toolsets is CloudFormation. As its name implies CloudFormation is specific to AWS. With CloudFormation you get AWS’ best effort to support new AWS features and implementations when they’re released. It’s easy to learn and as you’d expect it plays well within the AWS ecosystem. It supports JSON and YAML syntax and is very easy to write and learn; albeit a bit more verbose in nature than its competitors. 

A lot of newer services in AWS abstract CloudFormation away while still attempting to help with best practices in IaC. For example, AWS Amplify CLI distills your work into a series of CloudFormation stacks for AWS to handle. For this reason It would be beneficial to learn how to read CloudFormation if you’re going to invest heavily in AWS as your public cloud provider of choice.

## Hashicorp Terraform 

Terraform is quickly becoming the de facto IaC golden standard. From a business perspective Hashicorp has done wonders to empower their business partners to grow with them, as well as creating things the industry values such as certifications for proven competencies. Technically speaking, Terraform is also very easy to learn and use. There are some additional concepts to familiarize yourself with such as “state files,” “providers, ”and “remote back ends”, but writing Terraform is similar in nature to the resource block declaration as all of the other IaC methods we’re discussing.

The biggest difference between Terraform and the rest of the competition is that Terraform is the only cloud native IaC toolset that is cloud agnostic. Not only does it work for all the most popular public cloud providers, but they have a ‘provider’ construct that allows you to work with popular IaaS platforms and even some private cloud providers. This makes Terraform a very attractive solution in the world of IaC. 

## Azure Resource Manager

Azure’s platform native IaC comes in the form of Azure Resource Manager. ARM works similar to AWS CloudFormation; however, lacks a few of the bells and whistles of CloudFormation. For example, not only are you limited to JSON for your syntax when writing ARM templates; however, there is no drift detection with ARM. 

Much like with AWS CloudFormation, this method creates a very opinionated IaC pipeline and you will lock yourself in to Azure. 

## AWS Cloud Development Kit

The newest kid on the block in the world of IaC is AWS Cloud Development Kit. This toolset takes the most innovative approach to IaC by creating an additional layer of abstraction in IaC. The goal of AWS CDK was to allow for development teams to easily onboard into AWS. The CDK itself supports a wide array of high level object oriented programming languages and allows you to define your code in a way that is familiar to you. Then CDK will synthesize your code into CloudFormation at runtime. 

AWS CDK has a lot of potential but is in its infancy at the moment. I feel in due time CDK or similar tool sets will replace more traditional IaC. 

## The Other Guys

Historically speaking there are non-native IaC tool sets out there. The same companies who have been specializing in configuration management and desired state configuration have been attempting to provide IaC service as well. This of course is in reference to Ansible, Chef, Puppet, etc. In my experience the IaC portion of those stacks are lacking and not entirely comprehensive. As a note, typically in my conversations with clients or other engineers IaC means more specifically ‘Cloud Native IaC’.

## Our Choice

I have had the pleasure of working with most of the IaC tool sets mentioned above. For this site we’ve decided to use Terraform. Our decision had to do with the fact that I was the main developer responsible for a lot of our pipeline and I needed to upskill on Terraform for their HCL certification. In the future I personally intend on really leaning into AWS CDK and I look forward to producing content on developing CDK stacks for you to consume.

As always, thanks for reading!

> <cite>- FIN -</cite>
> <cite>Christopher L Medina</cite>
> <cite>Solutions Architect - Masterthe.Cloud</cite>