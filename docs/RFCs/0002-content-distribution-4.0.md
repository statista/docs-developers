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
This section details why we are doing something, what use cases it supports, and what we expect the outcome to be.

The current content distribution process has been invented to de-couple content and user databases. Data gets generated and serialized on-the-fly in the form it gets used in the frontend - the main reason for that has always been improving the frontend performance for our customers. Furthermore, we also improved reliability and monitoring in setting up stable queuing systems and logging. 

However, the existing process does have flaws, which lead to very long development cycles. One of the major concerns at the moment is, that adding the main content data (Statistics, Studies, Topics etc.) to the content takes very long, as all databases and technologies contain the complete data, yet the process is laid out to be very error-prone and write-operations are done for every single piece of data in every datacenter region. 

We should also consider building one pool (aka data lake) for all contents (including CompanyDB, eCom etc.) to leverage accessibility for other teams outside PIT. By enabling other people to work on and with the data we would be able to shift responsibilities to the product owners and could leverage a data mindset across slices. Additionally we could we could easily generate the data and data formats that the databases and technologies need based on the data lake. A nice side effect would be, that we wouldn't have to re-invent processes and database technologies for different projects and a chance to unify those.

A future-proof reimplementation should:

* handle 100x contents in a performant and reliable way
* ease building relationships / linking contents with each other
* strengthen (data) responsibilities
* unify databases & caches
* ease building data-driven KPIs
* help to build a clearer Domain Driven Design
* have clear interface definitions
* be well documented

# Guide Implementation
This is a high level overview of the process. In this section, explain the proposal as if it was already used at Statista and you were teaching it to a new developer. That means you're probably including

* Introduction of new named concepts
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

# Drawbacks
You should explain why we _wouldn't_ do this. There is a cost associated with these changes, and while there wouldn't be an RFC if the benefits didn't outweigh the drawbacks, your readers might not know all of the tradeoffs being made.

# Alternatives
You need to answer why this design is the right design. To do so, you need to understand all of the possible alternatives. You should list alternate designs and ideas and why they weren't considered for the solution. This ensures due diligence has been done on the proposal.

# Unresolved Questions
You should gather any open questions and either list them in the document (folks often use markdown quoting) or add them to this section at the end of the document. If you want to mark unresolved options inline

> **Open Question:** this is an open question

Is the common format used. The quote and bold combination (plus its indent) makes it easy to spot when reading through the document.
