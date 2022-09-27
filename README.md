# OpenInsights
A universal framework to configure Insights and Best Practices


## What is OpenInsight
OpenInsight is a standard describing data formats and elements (known as "resources") and an application programming interface (API) for exchanging analysis of best practices. 
The Protocol was created by Itay Braun, a databades expert, after spending way too much time on helping organizations to fix the same configuration, schema and performance problem. 

Note: The vision of OpenInsight is wide and relevant to many modern technologies. However, Metis Data, the company behind OpenInsight, focuses on databases. Therefore the examples are only about databases, mainly postgres and MySQL.  

##The Problem OpenInsight is trying to solve
Databases best practices are compliated. It is hard to know how to configure the server and each database, use the right table schema and build the best indexes. The developers / DevOps / SRE can read the documentation, StackOverflow posts, buy some books and try to process all the information into a clear action plan. 
OpenInsight aims to provide an end-to-end solution for: 
- Definition of best practices
- Workflow engine to evaluate the current status and generate insights
- A server to store the insights
- Tools to run the workflows and view the results

## Our Vision 
Building a platform for managing best practices, to help developers and DevOps to use proactively stop problems before they hit production. 
We do that using predefined workflow to collect the relevant information, process it and provide explainable action plans.   

## Observability - Next phase


## Our Guidelines for building OpenInsights
1. Before we can analyze the current status we must agree on the **Facts**. Therefore the first step is collecting the raw data to build the Facts. 
2. The analysis layer (Insights) assume all the Facts are accurate and were calculated correctly. Data validation is not part of the Insights Layer. 
3. An OpenInsight Fact should implement high explainability: how the values were calculated, queries and explanations. (a) 
4. Insights are opinionated. A natural evolution of the protocol is having many packages of insights, created by different groups, with different definitions of what is good or bad and what data is needed to determine it. 


(a) The [OpenMetadata](https://open-metadata.org/) might be useful here.

## The Building Blocks
### Observations
The OpenInsight protocol starts with defining **Observations**. An Observation is a fact, not good, not bad, just a fact about the observed system. 
Examples of observations: 
* The Postgres server called pg1 is configured with shared_buffers = 128MB.
* The last backup of the DB db-name-1 was 3 days ago.
* The CPU of the Postgres server called pg1 was on avg 78.2% between Sep-14-2022 15:00:00 and Sep-14-2022 16:00:00.
* The query SELECT * FROM tbl_orders WHERE order_date > '2022-09-14 15:09:12' read 1.5GB of data from the hard drives.

Since the OpenInsight relay on the Observations and sees them as unquestionable truth, the Observation should contain the metadata of how it was calculated, to improve explainability and avoid false positive alerts. 

An **Insight** in applying set of **Rules** on the **Observations** to evaluate the existing facts into an analysis of good or bad. By default the OpenInsight protocol suggest 5 levels of severity to help the users to quickly understand how bad is the current status.
* INFO - The insight reviewed the observations but couldn't find any problem
* LOW
* MEDIUM
* HIGH
* CRITICAL

### Automated Investigation
The **Automated Investigation** is part of the **Insight**. It follows the principal of good explainability, sharing with the user the internal flow which lead to the shown severity. It shows which observations were used and how the data was evaluated to the selected severity. That should help user to increase confidence in the insights or manually changes the **Rules**. 

### Remediation Plan
A **Remediation Plan** is another part of an **Insight**. It is a clear call to action, usually followed by a script to implement the plan. 

### Knowledgebase Item 
The Insight should be short and clear. Sometimes, explaining about a concepts used in the Automated Investigation or the Remediation Plan can be long. Therefore an insight might contain a link to the knowledgeable for further reading. 

### Insights Package
An **Insights Package** is an opinionated list of insights, grouped around a specific subject. 
For ex. The Insight Package "Postgres Configuration Best Practices" contains insights about the values of the configuration property "shared_buffers", with rules that the actual value is 25% of the Machine's RAM. It also contains a link to a Knowledgeable Item that explains how we got to this number and the impact of misconfiguration. 
The Insight Package contains specification of all needed observations and the scripts used to bring the data. 



