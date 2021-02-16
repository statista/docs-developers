---
RFC: 0002
Title: Content Distribution 4.0.
Start: 01/01/2021
PR: <blank>
---

# 0002 - Content Distribution 4.0

# Summary
This is a proposal to change the fundamental content distribution concept to be future-proof for the next five years. We do have to have a resilient and performant content distribution system that is easy to comprehend. Furthermore, the system should be accessible to other departments within Statista to enable data analytics and operations on the raw data without compromising compliance and performance. 

# Motivation

The current content distribution process has been invented to de-couple content and user databases. Data gets generated and serialized on-the-fly in the form it gets used in the frontend - the main reason for that has always been improving the frontend performance for our customers. Furthermore, we also improved reliability and monitoring in setting up stable queuing systems and logging. 

However, the existing process does have flaws, which lead to very long development cycles. One of the major concerns at the moment is, that adding the main content data (Statistics, Studies, Topics etc.) to the content takes very long, as all databases and technologies contain the complete data, yet the process is laid out to be very error-prone and write-operations are done for every single piece of data in every datacenter region. We should consider building one pool (aka data lake) for all contents (including CompanyDB, eCom etc.) to leverage accessibility for other teams outside PIT. By enabling other people to work on and with the data we would be able to shift responsibilities to the product owners and could leverage a data mindset across slices and departments. Additionally, we could easily generate the data and data formats that the databases and technologies need based on the data lake. A nice side effect would be, that we wouldn't have to re-invent processes and database / caching technologies for different projects and a furthermore even have a chance to unify those to increase knowledge.

A future-proof reimplementation should:

* handle 100x contents in a performant and reliable way
* ease building relationships / linking contents with each other
* strengthen (data) responsibilities
* unify databases & caches
* ease building data-driven KPIs
* have clear interface definitions

# Guide Implementation

For the implementation of the Statista 4.0 content distribution process we rely on the concept of a data lake in order to store all data produced by either the content editors (ERI), data scientists (SMI), data analysts (R&A) as well as 3rd party raw content (e.g. CompanyDB). Taking the Statista data lake as the foundation we are continuously processing the data according to the needs of the recipient (e.g. frontend or search) and the underlying technology - e.g. if data is needed in search the responsible developer has to adjust the data process to include the data in question. Through building this foundation every slice / project can pick the data they want to have from the well-documented data lake and transform it to their needs. We do not need to store all data in every technology "just-in-case-of". This also shifts the responsibility to the according slice and in a well-architected scenario helps to fix performance problems and addresses data compliance easily. 

In order to setup this distribution process, we do have to set up ETL processes in order to process the data on a regular basis.
> **Open Question:** This should be as often as possible, but only as often as necessary. As a thumb-rule we should refresh all data at least every three hours but shouldn't refresh more than once in fifteen minutes.


* Introduction of new named concepts
  
- Data Lake
- ETL Processes / Glue
- Event Stream

* Explain things in terms of examples
* Explain the impact and how you want developers to approach the feature and its goals
* If applicable, provide sample errors / warnings / or migration guidance
* If applicable, describe the differences required teaching this to new developers vs existing developers

# Reference Implementation
This section provides space for deep technical details as required in the RFC. You need to hit on the following points in your reference implementation:

* It's interaction with other features / infrastructure
* It is reasonably clear how this would be implemented
* Corner cases are addressed through example

You can (and should) expand on the examples you used in the Guide Implementation and provide additional context.

In a perfect scenario we do not have to change a lot as content is processed from the old implementation to the new distribution process by only adding another consumer to the queuing system that does write the messages to the event stream. The backend systems can be switched one by one, simply changing the consuming endpoint to the new version once it was developed and deployed. In this way both systems can exist next to each other for quite a long time, although the final switch should be terminated - it can be done by only writing to the event bus, leaving the old queuing system untouched.


# Drawbacks
You should explain why we _wouldn't_ do this. There is a cost associated with these changes, and while there wouldn't be an RFC if the benefits didn't outweigh the drawbacks, your readers might not know all of the tradeoffs being made.

- ETL Processes 
    - learnings for developers
    - code in unknown domain
    - code duplication in different "views"
- Cloud Provider
    - further lock-in through ETL Processes
    - 

halten die APIs das durch?


# Alternatives
You need to answer why this design is the right design. To do so, you need to understand all of the possible alternatives. You should list alternate designs and ideas and why they weren't considered for the solution. This ensures due diligence has been done on the proposal.

- wie bisher
- "API Variante" - einbauen von Legacy Switches in alle Prozesse


# Unresolved Questions
You should gather any open questions and either list them in the document (folks often use markdown quoting) or add them to this section at the end of the document. If you want to mark unresolved options inline

> **Open Question:** this is an open question

Is the common format used. The quote and bold combination (plus its indent) makes it easy to spot when reading through the document.
