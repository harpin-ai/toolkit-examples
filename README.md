![image](images/harpinAI-logo-medium.png)

* [Harpin AI Overview](#harpin-ai-overview)
  * [The Business Problem](#the-business-problem)
  * [The Technical Challenge](#the-technical-challenge)
  * [The harpin AI Solution](#the-harpin-ai-solution)
* [Repository Overview](#repository-overview)
  * [Docker Image](#docker-image)
  * [AWS Sagemaker Algorithm](#aws-sagemaker-algorithm)
* [Contact Us](#contact-us)

# Harpin AI Overview
Quality entity data is the bedrock of effective business operations. Harpin AI maximizes the value of your data (and the value of your investment too). Our team of experts and ML/AI technologies pre-process your data in real time, enhancing the quality, authenticity, and integrity of your data. Harpin AI will transform inaccurate, erroneous data into a cost-saving, revenue-driving asset that feeds into and powers your sales, marketing, call centers, compliance and operational systems.

## The Business Problem
Organizations have data, and dirty data at that, spread across many disparate systems today.  The reasons for this are plentiful but include selection of purpose-built technologies such as customer data platforms (CDPs), customer relation management (CRM) tools, customer loyalty systems, transactional systems for point of sale (POS) or payment, and so on, duplicative systems obtained through mergers and acquisitions, and legacy systems that were built over time to solve a specific business need.  Even if your company has created data warehouses and data lakes to bring all this data into a single technology, it's still a challenge to get an accurate representation of each customer as there is often no reliable way to link the data across all of these data sets.  You are then unable to find your most loyal customers, identify customers that are at risk of churn, or even get an idea of how many distinct customers you have.  And good luck using all the new ML and AI technologies out there as you will struggle to achieve the desired outcomes if you are training your models with messy data!  This is where harpin AI can help by pre-processing your data through our data quality and identity resolution technologies to provide high quality data organized into identity profiles for use in all of your downstream systems.

## The Technical Challenge
The goal of our identity resolution solution is to group together individual records seamlessly across disparate data sources into unique profiles. The desired outcome is to produce a single profile for each person combining the attributes from all of the pertinent records across data sources. There are two main technical hurdles that must be cleared to achieve this end goal. First, the scale of the problem prohibits a brute force solution relying on exhaustive pairwise comparison. For instance, a record system with one billion entries produces 10^18 pairs, which would be infeasible to process even on the fastest supercomputer. Second, data integrity issues eliminate simple solutions, which are not robust to data sources using different schema, conflicting information across records, or missing values in key fields.

## The harpin AI Solution
Our identity solution is able to overcome these and other challenges by leveraging automated data repair using LLMs with state-of-the-art supervised and unsupervised machine learning models. The core algorithm is a sequential clustering process, where the full set of records is first sorted into small disjoint groups that share some similarities. Within each of these groups, clusters of records are formed using hierarchical clustering. This clustering relies on fuzzy attribute matching in conjunction with dedicated machine learning models to identify the relevant customer profiles. Finally, edge pruning is used to clean up and condense the resulting identity graph. The result is a solution able to scale to billions of records despite data quality issues with industry-leading performance maximizing the aggregation of records while minimizing the number of incorrect merges.

# Repository Overview
While the main harpin AI toolkit is primarily offered as a software as a service (SaaS) solution, we wanted to provide more accessible ways for software engineers, data scientists, data engineers, and others who are tasked with solving today's difficult data problems with a way to easily take the harpin AI toolkit for a test drive.  This repository contains materials that will help you evaluate the harpin AI toolkit on your data, in your environment, for maximum security.  That way you can spend your time playing around with our toolkit instead of filling out forms and answering questions from your compliance team.  The following sections highlight the different ways you can run the harpin AI toolkit, with links to step-by-step instructions to get you up and running quickly.

## Docker Image
Download our harpin AI Toolkit Docker image and try out our identity resolution algorithm on your data.  Run the Docker image anywhere you have access to the data and a Docker runtime, in your cloud or on your own workstation.  The harpin AI shell will walk you through the setup and configuration process and have the resolution process running in Docker in no time at all.

Step by step instructions for downloading and running the harpin AI Toolkit Docker image are available [here](docker/README.md).

## AWS Sagemaker Algorithm
*COMING SOON!!!*

For you data scientists out there using AWS, we are working on a Sagemaker algorithm that will allow you to run the harpin AI toolkit with a single line of Python code in a Jupyter notebook.

# Contact Us
Do you have a question or problem with one of these solutions?  Is there something missing that would help your team or help you evaluate this product?  Do you like what you see and want to learn more about our product or get a demo?
* [Start a discussion on Github](https://github.com/harpin-ai/toolkit-examples/discussions)
* [Request a demo](https://harpin.ai/demo/)
* [Send us an email](mailto:engineering@harpin.ai)
