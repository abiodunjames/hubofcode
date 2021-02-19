---
title: Choosing the Right Cloud Provider – Containers
date: 2021-02-15 07:15:00 Z
categories:
- SoftwareEngineering
tags:
- containers
- cloud
canonical_url: https://iamondemand.com/blog/which-cloud-provider-is-right-for-you-an-iod-series-part-2/
author: samuel
comments: true
---

Prior to container technology, making applications run on different environments was one of the greatest struggles for a developer. “It runs on my machine” was a common frustration you heard from engineers—including me, many times.

Like numerous engineers, I’ve built a working code on my machine that refused to then run in other environments due to the disparity of environments, that is, different configurations and dependencies.

> Editorial note:  This post was initially published on Iamondemand’s blog. You can check out the [original here](https://iamondemand.com/blog/which-cloud-provider-is-right-for-you-an-iod-series-part-2/), at their site.

[Docker](https://www.docker.com/resources/what-container) defines containers as “a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.”

By bundling an entire runtime environment, including an application plus all its configurations, binary files, dependencies, and libraries needed to run it into a single image, container technology eliminates the “but it runs on my local machine” problem and allows you to adopt the “build once, deploy anywhere” principle.

Containers provide a consistent deployment environment that development teams can use across software development and delivery pipelines.

The demand today for tolerant, reliable, and scalable software has given rise to thousands of containers running in a typical production environment, which brings a new problem: management. Container orchestration tools like [Kubernetes](https://iamondemand.com/blog/5-facts-you-need-know-about-k8s-til-now/) solve this issue.

Kubernetes lets you run and manage containers and deploy stateful and stateless applications with multiple cloud providers. With Kubernetes, you can run a single application across a fleet of machines or different computing environments. Furthermore, you can either deploy your applications to a Kubernetes cluster in the cloud or on-premises. Of course, the major challenge with cloud deployments is selecting the right provider out of the many out there that offer managed Kubernetes as a service.

Before making a decision, it’s important you examine each service provider to identify which one best suits your organization’s needs. In this post, I’ll discuss the top Kubernetes as a Service offerings, their similarities, key differences, and how to choose the right one.

# How to Choose the Right Kubernetes Provider

Most cloud providers have their own set of Kubernetes hosting environments for managing containers. Although Kubernetes offerings vary wildly among providers, here are some factors to consider as you shop for the right platform to manage your container workloads:

## Data Security

Before selecting a Kubernetes service provider, you should understand each provider’s security governance process, measures, and mechanisms for preserving your data and application. An ideal Kubernetes service provider should align with strict security compliance and industry best practices. It should also implement some sort of confidentiality for sensitive data and provide administrative rights for restricting, monitoring, blocking, and approving users’ access to data.

## Performance and Reliability

Reliability is an important requirement of any application running in the cloud. You should ascertain the reliability of cloud service providers by comparing their performance against their service-level agreements for the last 6 to 12 months. Cloud service providers usually publish this information, and even if they don’t, you can always request it.

You shouldn’t expect 100% perfection because every provider will experience downtime at some point. A good example was the[ recent Google incident](https://techcrunch.com/2020/12/14/gmail-youtube-google-docs-and-other-services-go-down-simultaneously-in-multiple-countries/). What matters the most is how often downtimes happen and how a provider handles them when they do occur. An ideal Kubernetes service provider should have documented, proven, and established processes for handling unplanned and planned downtime.

## Standards and Certifications

Before going forward with a Kubernetes provider, you need to ensure that the provider adheres to the best industry practices and recognized standards. It’d be helpful if you also understood how the provider plans to continuously adhere to these standards. An ideal Kubernetes provider should have good knowledge management, effective data management, service status visibility, and structured processes.

Now that you’re familiar with the criteria to consider while searching for the right Kubernetes Managed Service Provider, the next step is to identify the market’s biggest players.

# Top Options for Kubernetes as a Service

There are many cloud services for running container workloads using Kubernetes, but let’s closely examine the Kubernetes solution from the Big 3 players: Amazon’s Elastic Kubernetes Service (EKS), Google Kubernetes Engine (GKE), and Microsoft’s Azure Kubernetes Service (AKS).

## Amazon Elastic Kubernetes Service

[Amazon Elastic Kubernetes Service (EKS)](https://aws.amazon.com/eks/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&eks-blogs.sort-by=item.additionalFields.createdDate&eks-blogs.sort-order=desc) is AWS’s managed container offering that allows developers to start, run, and scale Kubernetes apps on-premises or in the AWS cloud. EKS also allows you to plan, schedule, and execute your container workloads across compute services and features available on AWS, such as Spot Instances, Amazon Fargate, and EC2.

With Amazon EKS, you can easily manage your applications and Kubernetes clusters across hybrid environments too, plus you can use the Kubernetes jobs to run parallel or sequential batch workloads on your EKS cluster. Common use cases for Amazon EKS include hybrid development, batch processing, machine learning, and web applications. The fee charged by AWS is $0.10/hour for each EKS cluster created.

## Strengths

* Highly configurable and customizable

* Efficiently provisions and scales your Kubernetes application

## Limitations

* Complexity involved with addition and customization of node pools

* No automatic node updates

## Google Kubernetes Engine (GKE)

[Google Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine) is Google’s managed offering that allows you to deploy, manage, and scale containerized applications. The GKE environment comprises multiple Compute Engine instances grouped together to form clusters that are powered by Kubernetes. By running a GKE cluster, you get to benefit from Google’s advanced cluster management features, including automatic upgrades and scaling, logging and monitoring, node auto-repair, load-balancing, and more.

With Google Kubernetes Engine, you can create clusters based on your budget and the availability requirements of your workload. The two available clusters include: regional and zonal (multi-zonal and single-zonal). GKE has a financially backed Service Level Agreement that guarantees 99.5% availability and 99.95% availability for[ zonal clusters](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters)and [regional clusters](https://cloud.google.com/kubernetes-engine/docs/concepts/types-of-clusters), respectively. Just like Amazon, Google charges $0.10/hour for every GKE cluster that you create, although there is no charge for Anthos GKE Clusters.

## Strengths

* Easy Cluster creation and management

* Automatic cluster patching and upgrades

## Limitations

* Difficulty in customizing server configurations

* No adequate documentation for supportive information

## Microsoft Azure Kubernetes Service (AKS)

[Azure Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/) is a highly secure, available, and fully managed Kubernetes service that allows you to deploy and manage containerized apps more easily. AKS offers an integrated CI/CD experience, serverless Kubernetes, enterprise-grade governance, and security. Your DevOps team can leverage AKS to build, deliver, and scale applications fast.

AKS has a financially backed Service Level Agreement that guarantees 99.95% availability for Kubernetes clusters using Azure Availability Zone and 99.9% availability for Kubernetes clusters that don’t. Unlike other providers, Azure doesn’t charge for Kubernetes cluster management. However, you pay for the resources you use, like networking, storage resources, and virtual machine instances.

## Strengths

* Faster end-to-end development experience via Azure DevOps, Kubernetes tools, Visual Studio Code, and Azure Monitor

* Advanced access and identity management using Azure Active Directory

## Limitations

* No automatic node updates

* Inability to update availability zone settings after a cluster is created

## Comparison Criteria

| Comparison Criteria | EKS                      | GKE                                                      | AKS                                                          |
| ------------------- | ------------------------ | -------------------------------------------------------- | ------------------------------------------------------------ |
| Kubernetes Release  | 1.15, 1.16, 1.17, 1.18   | 1.16, 1.17, 1.18                                         | 1.15, 1.16, 1.17, 1.18, 1.19                                 |
| Cluster management  | $0.10/hour               | $0.10/hour                                               | Free                                                         |
| SLA                 | 99.9%                    | 99.95% for regional deployments and 99.5% for zonal ones | 99.95% for clusters that use Azure Availability Zone and 99.9% for clusters that don’t |
| Year Started        | 2018                     | 2014                                                     | 2018                                                         |
| Serverless compute  | Fargate                  | Cloud Run                                                | Azure Container Instances                                    |
| Bare Metal Nodes    | Yes                      | No                                                       | No                                                           |
| Resource monitoring | Third-party (Prometheus) | Stackdriver                                              | Azure Monitor                                                |

Google, Amazon, and Microsoft Azure all offer Managed Kubernetes solutions, but you’re at liberty to select the provider whose offerings meet your organization’s needs and requirements. If you don’t want to use EKS, AKS, and GKE, the three cloud providers do have other alternatives that you can use to run container workloads.

Alternative to Kubernetes for Running Container Workloads

There are two ways to run container workloads on the cloud: using managed Kubernetes services like AKS, GKE, EKS or by [leveraging the serverless container services](https://iamondemand.com/blog/what-why-how-run-serverless-kubernetes-pods-using-amazon-eks-and-aws-fargate/) provided by the cloud providers. If you don’t plan to run on the cloud, you can also adopt a self-hosted solution like OpenShift to run container workloads.

By implementing serverless container services, you can manage the life cycle and availability of your container workloads. Examples of serverless container services include Google Cloud Run, AWS Fargate, and Azure Container Services.

## AWS Fargate

[AWS Fargate](https://aws.amazon.com/fargate/?whats-new-cards.sort-by=item.additionalFields.postDateTime&whats-new-cards.sort-order=desc&fargate-blogs.sort-by=item.additionalFields.createdDate&fargate-blogs.sort-order=desc) is a serverless compute engine that lets you run containers without the need to provision, configure, or scale clusters of your virtual machines. With Fargate, you don’t have to choose server types, decide when to optimize cluster packing, or scale your cluster. Development teams can thus focus on building and operating applications while Fargate manages all the infrastructure and scaling needed to run the app with high availability.

## Google Cloud Run

[Cloud Run](https://cloud.google.com/run) is a serverless compute platform that allows you to build and deploy highly scalable applications without the need for Kubernetes-based or VM deployments. With Cloud Run, you can build applications with your favorite tools/dependencies and in your favorite language and deploy the apps in seconds.

Cloud Run offers several benefits, including auto-scaling, no up-front provisioning, and zero server management. It’s ideal for use cases like mobile backends, web applications, stateless HTTP applications, streaming, batch data processing, etc.

## Azure Container Instances

[Azure Container Instances (ACI)](https://docs.microsoft.com/en-us/azure/container-instances/) is a serverless compute platform that enables you to easily run containers without having to handle the management of the virtual servers. ACI lets you deploy containers in the cloud with remarkable speed and simplicity. It also enables you to secure applications with hypervisor isolation.

By running your application in [Azure Container Instances](https://iamondemand.com/blog/running-kubernetes-windows-containers-on-the-azure-cloud/), you can focus on developing applications without worrying about managing the infrastructure that runs the app. ACI is suitable for [data-processing jobs, building event-driven applications, and elastic bursting](https://azure.microsoft.com/en-us/services/container-instances/#documentation).

# Wrapping Up

Containers solve the problem associated with the multiple, different environments that development teams must deal with today, letting them build and deploy the same application for development, testing, and production environments. And Kubernetes allows you to orchestrate these containers.

Although there are several cloud service providers for your container workload orchestration, you need to compare what each provider brings to the table. By understanding the strengths and limitations of the various managed Kubernetes services, you’ll be well positioned to select the right provider that perfectly suits your organization’s needs.