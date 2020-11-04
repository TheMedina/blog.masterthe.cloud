---
layout: post
title:  "Journey Through the Cloud PT 2"
date:   2020-11-03
image: /images/posts/pipeline.png
tags: [Series, Christopher L Medina, CloudFront, AWS, OAI, S3, Lambda@Edge]
---

In part two of our Journey Through the Cloud we're going to talk a little bit more about our overall desired endstate of our architecture. This will focus more on our end user expereince and best pracitces in regards to security, and not so much on the CI/CD process of our Jekyll site. If you're a fan of CloudFront and or Origin Access Identity this will be the blog for you!

<!--more-->

Best practices in AWS would dictate that no object in our S3 buckets should be publicly viewable. This is why if you create a policy that allows your objects to be public (i.e. static web content) there is a big red triangle that is almost indicative of an error message. So how do we work around this? 

{% include image_caption.html imageurl="/images/posts/public.png" title="Public Buckets" %}

Enter Origin Access Identity (OAI). Using AWS CloudFront we can create and associate a special user (the OAI) and restrict our S3 buckets to only allow that OAI user to serve the content. Content is no longer served via an S3 http endpoint, rather it’s retrieved by a CloudFront Distribution and delivered on origin request via a REST call. This is a very simple process and is effectively only two steps:

1. Create the OAI CloudFront user and associate it with your distribution.
2. Configure your S3 bucket permissions so CloudFront can use the OAI to access files in your bucket and serve them to your users. 

You can view the link <a href = "https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html" target = "_blank">here</a> to for a thorough walkthrough from the AWS Docs. 

Completing the above steps will make it so your site is adhering to best practices as it relates to static content hosting, and buckets. Additionally, you’ll gain the added benefits of all the bells and whistles of a CloudFront Distribution!

Great, so we’re done right? My content is secured, performant, and cached at several edge locations. What else do we need? It really depends on what it is you're hosting. Since the focus of this series is to talk about our rebranding efforts, we’ll talk about an issue we ran into that is quite common when leveraging Static Site Generators, routing and permalinking. The out of the box the methodologies that a lot of the popular SSGs use won’t function properly in our leveraged model. S3 REST endpoints do not support redirection to a default index page. This means that if a hyperlink directs a user to a directory that is created during our build process with the expectation that the index of that directory will be called, we’ll have a failure. This isn’t typically an issue with web servers or S3 buckets with web hosting enabled; however, once again, we’re using S3 REST endpoints in our model. Fortunately this is a pretty trivial fix. 

We can use Lambda@Edge to inspect user requests then re-write the request to a default object that exists (probably index.html) for request URIs ending in ‘/’. The following JavaScript is a simple lambda function you can use for this purpose:

```javascript
'use strict';
exports.handler = (event, context, callback) => {
    
    // Extract the request from the CloudFront event that is sent to Lambda@Edge 
    var request = event.Records[0].cf.request;

    // Extract the URI from the request
    var olduri = request.uri;

    // Match any '/' that occurs at the end of a URI. Replace it with a default index
    var newuri = olduri.replace(/\/$/, '\/index.html');
    
    // Log the URI as received by CloudFront and the new URI to be used to fetch from origin
    console.log("Old URI: " + olduri);
    console.log("New URI: " + newuri);
    
    // Replace the received URI with the URI that includes the index page
    request.uri = newuri;
    
    // Return to CloudFront
    return callback(null, request);

};
```

Follow this phenomenal article on AWS blogs to know more details about how to get this up and running.

Join us next time where we'll go into more detail about the CI/CD pipeline we created in AWS to enable the Jekyll builds and allow us to update our Jekyll blog in production with a single code commit into a git repo. 

As always, thanks for reading!

> <cite>- FIN -</cite>
> <cite>Christopher L Medina</cite>
> <cite>Solutions Architect - Masterthe.Cloud</cite>