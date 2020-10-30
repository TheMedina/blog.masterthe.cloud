---
layout: post
title:  "Journey Through the Cloud PT 1"
date:   2020-10-30
image: /images/posts/mtc.png
tags: [Series, Christopher L Medina, Static Site Generator, Hugo, Jekyll, GatsbyJS]
---

In this blog series we’ll walk through why and how we chose our technology stack for our rebranding initiative here at masterthe.cloud. We’ll cover some intermediate Terraform, Static Site Generators (SSG), and some core AWS services. Additionally, there will be a video on our youtube channel towards the end combining all of the blog posts. This first part will cover Static Site Generators, what they are, and why chose the one we did.

<!--more-->

## A Brief History

Most sites during the initial budding years of the Internet were static sites. They had static content delivered in the form of HTML files and not much else. This was affectionately known as Web 1.0. Around the late 90’s and early 2000’s there was a fundamental change in the way users wanted to consume the internet. This in turn led to a change in how we developed websites. 

Web 2.0 was defined by the aggressive embrace of user generated content, emphasis on UI improvements (JS DOM), and improved interoperability (API’s), etc. Web apps became increasingly complicated and abstracted leveraging databases and server side scripting. Infrastructure required dedicated arrays of servers to handle the complex nature of the user loads and the scalability requirements. This is the internet we know today, but it has come at a cost.

The overhead needed to host an average WordPress blog even on a container is far too much. Traditional three tier application architecture may be a bit of an overkill depending on what your needs are. There is a runtime cost to handle the processing needed to render a standard site made in PHP backed by a DB.  The scalability and management are also increasingly more complex, because you can’t just manage your code base. How can we sidestep unnecessarily complex infrastructure and resource overhead?

## What are Static Site Generators

Enter Static Site Generators (SSGs). These are typically packages that will process templated layout files and convert them into a static site after a build process. They will maintain organizational structure as dictated by your configuration and are extremely performant due to the lack of overhead. Generally a SSG will produce flat files in the form of HTML and CSS. Dom related JavaScript also fits this model just fine.

This all sounds great. So why doesn’t everyone use a SSG? The short answer is that it depends on your needs. If your requirements are to create and host an information event page, ads, or even simple blogs, then you probably should be using a SSG. If you’re looking to manage an ecommerce site or custom web applications, I don’t believe we’re there yet. You could still create or use a custom solution leveraging Serverless principals or one of the more ‘web appy’ SSGs; however, there isn’t a SSG solution for that level of complexity as of yet.

## Different Static Site Generators

There are many different types of SSGs. The concept has been around for quite a while and is only gaining in popularity. There are a wide array of different SSGs that support different templating styles and languages. Much like most projects it will always come down to identifying your needs and finding the best tool for the job. 

For us the decision came down to the more popular three SSGs.

### Hugo

Hugo is written in Go and has quickly become one of the most popular SSGs available. It’s fairly simple to use and is known for its incredible build speed. This could come in handy when we factor in growth as a consideration. In addition it has a very active development community and documentation. 

### Jekyll

Jekyll was the first SSG. It’s written in Ruby and has one of the simplest learning curves we’ve seen. It also has a very large community and is tightly integrated with GitHub.

### Gatsby

Gatsby is phenomenal at what it does. However, that is not without its drawbacks. It has a very steep learning curve and you need to be familiar with GraphQL, React, and other technologies. It’s really good at helping you reach custom solutions that require a true web app approach. 

In the end we choose to split our services. For blog.masterthe.cloud we decided to go with Jekyll. It was easiest enough for us to learn and manage. The build time is noticeably slower than our Hugo test; however, it’s not a major inconvenience. Also this won’t really become an issue with our project scale of growth. For our main masterthe.cloud service that we're still working on, we decided to test the waters with Gatsby. We’ll be sure to document our journey and share it with you as we progress.