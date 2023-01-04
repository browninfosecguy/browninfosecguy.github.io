---
layout: post
title: "DevSecOps Chronicles Part 1"
summary_large_image: /images/devsecops101/DevSecOpsVenn.png
description: "This post is about DevSecOps 101."
date: 2023-01-03
feature_image: /images/devsecops101/DevSecOpsVenn.png
tags: [DevSecOps]
hashtags: CyberSecurity,DevSecOps
---
This is my first blog post after a very long hiatus. I'm hoping to create a blog post series on DevSecOps. This is my first blog in that series which explores some basic concepts around DevSecOps.
<!--more-->
# DevSecOps
According to [Department of Defense](https://dodcio.defense.gov/)
*DevSecOps is an organizational software engineering culture and practice that aims at unifying
software development (Dev), security (Sec) and operations (Ops). The main characteristic of
DevSecOps is to automate, monitor, and apply security at all phases of the software lifecycle:
plan, develop, build, test, release, deliver, deploy, operate, and monitor.*
{% include diagram.html imageurl="/images/devsecops101/DevSecOps.png" caption="Source: DoD Enterprise DevSecOps Fundamentals" %}

Before we can discuss DevSecOps it's important to understand what is DevOps? According to [NIST](https://csrc.nist.gov/glossary/term/development_operations) DevOps is defined as:

*A set of practices for automating the processes between software development and information technology operations teams so that they can build, test, and release software faster and more reliably. The goal is to shorten the systems development life cycle and improve reliability while delivering features, fixes, and updates frequently in close alignment with business objectives.*

DevOps has tremendously accelerated the Development and Delivery process. As we can see from the figure below there is an evolution in the application development process. Microservices have replaced monolithic architecture. Containers have made it really easy for developers to bundle all the dependencies and make the application portable. A Containerized application just requires a container run time engine to run the application, there is no additional overhead to install dependencies.
{% include diagram.html imageurl="/images/devsecops101/DevOps.png" caption="Source: DoD Enterprise DevSecOps Fundamentals" %}

Kubernetes and Containers have fundamentally changed the way applications are developed and delivered today. The software development process no longer happens in silos; it has been replaced with Continuous Integration and Continues Delivery (CI/CD). Any modern application looks similar to Figure 1. As we can observe, application hosting and development no longer happens on a server rack sitting in a local Data Centre rather it happens on the Cloud. Cloud Service providers like AWS, GCP and Azure provide robust, scalable and always available environments.
{% include diagram.html imageurl="/images/devsecops101/modernInfra.png" caption="Figure 1: Modern Application Workload" %}
 
GitOps has brought the same concepts from the DevOps world to Infrastructure. The underlying infrastructure to run the application is now fully codified and follows version control. The code repository serves as the single source of truth for the infrastructure, any variation from desired state is quickly rectified and brought back to the desired state as shown in Figure 2.
 {% include diagram.html imageurl="/images/devsecops101/GitOps.png" caption="Figure 2: Infrastructure as Code" %}
 
If we take a step back and analyze this architecture from a security perspective we can break down security requirements for each layer which comes with its own challenges. One example would be since everything is code, we no longer can just focus on Application code for security vulnerabilities but also have to focus on Infrastructure code for potential security flaws. While cloud has its advantages, suddenly everyone can spin up their new instance and start doing stuff which can lead to an insecure environment, so we need to have more robust RBAC (Role Based Access Control) policies for cloud.
 
**The key takeaways for security professionals are:**
 
1. Everything is Code now, you no longer click buttons to set up something.
2. Stop thinking Security with a traditional mindset.
3. Learn how the cloud works and the security implications.
4. Everything is API driven so learn how API's work.
5. Kubernetes and Containers run the world so learn them.
6. Learn how CI/CD pipelines are created and work.
7. YAML is the language to navigate this world so learn YAML.
8. Implement security at all stages of the DevOps cycle.
 
We have only scratched the surface in this post and tried to setup the stage for next installments of this series. I would be diving deeper into all of these topics, first presenting basic concepts and then looking at them from a security perspective. Following are some topics one must understand to implement a good DevSecOps program.

1. *Kubernetes*
2. *Containers*
3. *Git*
4. *OWASP Top 10*
5. *Static and Dynamic Code analysis*
6. *Secure code review*
7. *CI/CD Pipeline*
8. *GitHub Actions*
9. *GitOps*
10. *YAML*

{% include thanks.html %}