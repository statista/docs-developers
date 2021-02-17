---
RFC: 0002
Title: Content Distribution 4.0.
Start: 01/01/2021
PR: <blank>
---

# 0002 - Content Distribution 4.0

# Summary
This is a proposal to change the fundamental content distribution concept to be future-proof for the next five years. 

We do have to have a resilient and performant content distribution system that is easy to comprehend. Furthermore, the system should be accessible to other departments within Statista to enable data analytics and operations on the raw data without compromising compliance and performance. 

# Motivation

The current content distribution process has been invented to de-couple content and user databases. Data gets generated and serialized on-the-fly in the form it gets used in the frontend - the main reason for that has always been improving the frontend performance for our customers. Furthermore, we also improved reliability and monitoring in setting up stable queuing systems and logging. 

However, the existing process does have flaws, which lead to very long development cycles. 

One of the major concerns at the moment is, that combining the main content (Statistics, Studies, Topics etc.) with each other takes very long. The same applies to technical process itself, as all databases and technologies contain the complete data, yet the process is laid out to be very error-prone and write-operations are done for every single piece of data in every datacenter region. 

We should consider building one pool (data lake) for all contents (including CompanyDB, eCom etc.) to leverage accessibility for other teams outside PIT. By enabling other people to work on and with the data we would be able to shift responsibilities to the product owners and could leverage a data mindset across slices and departments. Additionally, we could easily generate the data and data formats that the databases and technologies need based on the data lake.

A nice side effect would be, that we wouldn't have to re-invent processes and database / caching technologies for different projects and a furthermore even have a chance to unify those to increase knowledge.

A future-proof reimplementation should:

* handle 100x contents in a performant and reliable way
* ease building relationships / linking contents with each other
* strengthen (data) responsibilities
* unify databases & caches
* ease building data-driven KPIs
* have clear interface definitions

# Guide Implementation

For the implementation of the Statista 4.0 content distribution process we rely on the concept of a data lake in order to store all data produced by either the content editors (ERI), data scientists (SMI), data analysts (R&A) as well as 3rd party raw content (e.g. CompanyDB).

<img src="https://docs.statista.tech/RFCs/0002-content-distribution-40-context.png" />

Taking the Statista data lake as the foundation we are continuously processing the data according to the needs of the recipient (e.g. frontend or search) and it's underlying technology - e.g. if the specific data item is needed in our search index the responsible developer or slice has to adjust the data process to include the data in question. Through building this foundation every slice / project can pick the data they want to have from the well-documented data lake and transform it to their needs. We do not need to store all data in every technology "just-in-case-of". This also shifts the responsibility to the according slice and in a well-architected scenario helps to fix performance problems and addresses data compliance easily. 

In order to set up this distribution process, we do have to implement ETL processes in order to process the data on a regular basis.
> **Open Question:** This should be as often as possible, but only as often as necessary. As a thumb-rule we should refresh all data at least every three hours but shouldn't refresh more than once in fifteen minutes.

This process introduces a few new concepts which we will describe here
### Event Stream

In our implementation the event stream can be described as another way of queuing with the ability to playback historic events. This way we can easily revert changes or re-apply changes to the queue. More functionality is available but not necessary for our case.

### Data Lake

Can be described as a data sink where all data is stored. This is useful for providing a central data repository, as a backup (versioning is easily possible) and in our case as the source for data transformations. We have been using S3 as a data sink for a long time, so we already have a lot of data foundation (e.g. webserver logs). 

### ETL Processes

A process to **E**xtract, **T**ransform and **L**oad the data from the data lake to the database or search index it uses - only the fields and data that is needed

In this context other processes often state the use of data warehouses to analyze and process the data in data lakes - in our case this isn't necessary as state-of-the-art data lakes get more functions like this as well (data lakehouses) and data warehouses are very expensive to use in production.  

# Reference Implementation
In a perfect scenario we do not have to change a lot, as content is processed from the old implementation to the new distribution process by only adding another consumer to the queuing system that does write the messages to the new event stream.

The backend systems can be switched one by one, simply by changing the consuming endpoint to the new version once it has been developed and deployed. In this way both systems can exist next to each other for quite a long time, although the final switch should be terminated - it can be done by only writing to the event bus, leaving the old queuing system untouched.

> **Open Question:** proposal: we will start with the main content and will switch to the new version for a certain content type, e.g. topic pages

<img src="https://docs.statista.tech/RFCs/0002-content-distribution-40-container.png" />

# Drawbacks
There are a couple of drawbacks involved, most of them regarding the ETL processes. The most obvious is that we will have to invest in a new paradigm, which leads to necessary knowledge-sharing and mentoring of developers. This also applies to the use of data lakes, which work rather different (column-orientated) from non-relational data storages (row-orientated). Additionally, AWS data processes often are serverless, which will lead to coding in less-known domains for developers. 

One of the biggest impacts is that there will be a lot of code duplication and cross-process documentation necessary as adding new data items might have to be done in all ETL processes - unfortunately this is something that has to be done either way.


# Alternatives

There are a couple of alternatives to this approach but all of them lack the greenfield momentum and would maybe take very long to be fully implemented. Additionally, a lot of extra development would have to be done to allow legacy switches for both systems to run smoothly next to each other. 

First approach would be to take the system that is in place at the moment and enable better batch-processing to speed up data integration. One of the major drawbacks here is that all teams have to stick to the main-process or duplicate processes as done at the moment (e.g. GCS). 

This leads us to the second alternative, which would be a microservice/API-centric way, in which every team would develop their own API and database to their needs. Especially from a dev ops perspective this is far from ideal, as all considerations and rules regarding SLA's (Service Level Agreements) have to be done for every new slice, backup strategies have to be put in place for all teams and knowledge & data-sharing to the out-of-slice-world would be even harder. Cross-Slice data combinations (e.g. topics enriched with e-com data) would still be difficult to archive. 

# Unresolved Questions

> **Open Question:** not sure if data processes should work like a deployment (generate data -> put into database -> test data -> switch servers / databases)

> **Open Question:** if we are updating all data with the search API processes for example, will the API be resilient enough for that? 
