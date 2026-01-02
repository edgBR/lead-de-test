

# Lead Data Engineer – System Design Interview

<p align="center">
	<img src="b2logo-crop.png" alt="Company logo" width="1024" />
</p>

<!--
Replace the `src` above with either:
- a workspace-relative path to a local image (example: `/assets/logo.png`), or
- an absolute URL (example: https://example.com/logo.png).

If your Markdown renderer doesn't support raw HTML, use this alternate centered option:

<div align="center"> ![logo](https://example.com/logo.png){width="240px"} </div>

Or a simple left-aligned image with standard Markdown:

![logo](https://example.com/logo.png)
-->

This document outlines the **System Design** portion of the Lead Data Engineer interview exercise.

The purpose of this interview is to understand how you approach designing a modern data platform, how you evaluate trade-offs, and how clearly you communicate your decisions. Your design should emphasize fundamental concepts and architecture principles. You should propose a technology stack to support specific parts of your solution, if this stack does not match what we have, this is completely okay, as we are not expecting an exact match to our current tools. This stack can have any kind of component (self-hosted open source, managed open source in the cloud, PaaS resources, etc...)

Please note that the design you produce will serve as an input for the second part of the interview exercise, which will be scheduled on a different date.


## Duration

**1 week to submit - Estimated 90-120 mins to complete**

Traditionally, we have conducted this stage of the interview process online via a virtual meeting. However, this approach makes it difficult to connect the system design interview with the second part of the process—unless the candidate agrees to record the session and share it afterward so the panel can review it in preparation.

In addition, we recognize that many applicants have full-time jobs, family obligations, and other commitments, so scheduling a long live session can be challenging.

For these reasons, we will give you one week to submit your design proposal. This does not mean you are expected to use the full week. In fact, we recommend that you stay aligned with our estimated effort and scope your design accordingly.

## Scenario

You are designing a modern data platform that ingests data from multiple sources and serves curated datasets for **analytics and machine learning**.


## Data Sources

### 1) Streaming/NRT Sources

Imagine an scenario where there is not standard infrastructure components to centralize all the potential messaging events that a company generates. These events will come mainly from 3 sources

- Web applications (e.g., clickstream, user activity)
- AI agent / LLM call events (e.g., prompts, responses, metadata such as latency, tokens, model name)
- Backend applications which might need to manually submit events after certain actions have been issued.

Streaming characteristics may include:
- Schema evolution over time
- Late-arriving and duplicated records
- Payloads with high cardinality and potentially sensitive data (e.g., prompts)

The data volumes of the streaming sources are low, that means that we process at max around **5K events/s**.

---

### 2) Batch Sources

Imagine an scenario where the batch sytems needs to ingest data periodically from:

- Public APIs from financial instutes (i.e European Central Bank, OECD, World Bank etc...)
- OLTP systems on-premises and in the cloud of all kind ((PostreSQL, Azure SQL, SQL Server, Oracle, MySQL etc...)
- These batch data sources data volumes are medium. **All historical data will be around 1TB** (for a full load).
- The datasets differs a lot in size. **One OLTP system might contain 4GBs of data, and another ones could contain 300GBs**.


Batch ingestion characteristics may include:
- Snapshot or incremental extraction.
- Limited network connectivity.
- Schema drift.
- Backfills and reprocessing needs.
- Lack of natural timestamps in the target tables needed for extraction.
- You have multiple OLTP systems (PostreSQL, Azure SQL, SQL Server, Oracle, MySQL etc...)


---

## Storage and Modeling Requirements

### 1) Open Table Format

Data must be stored using a columnar data format, the storage format should support:

- Schema evolution.
- Efficient incremental reads/writes.
- ACID-like behavior and consistency guarantees.
- Time travel / versioning (if applicable).
- Partitioning and file management best practices.
- Hard deletes.

---

### 2) Layered Architecture
The platform should follow a layered approach (example: Raw/Bronze/Silver/Gold):

- **Raw:** raw ingestion, minimal transformation.
- **Bronze:** raw ingestion with schema correctness.
- **Silver:** conformed and validated data, modeled entities.
- **Gold:** curated datamarts optimized for consumption.

Or any other alternative that you consider. We think about this concepts just as conceptual models. Wether this layers are for all of the data, scoped by domains or any other subdivision this is completely up to you to decide. It might be useful for you tough, that usually **our data comes from different countries which can have different investment entities within a country**.

---

### 3) SCD Type 2 in Silver

In the Silver layer (or wherever you decide), entities must be modeled using **Slowly Changing Dimensions (SCD) Type 2** to preserve history.

Your approach should address:
- How records are versioned
- How change detection works
- How late-arriving updates are handled
- How deletes are represented (soft/hard deletes, expiry)

---

### 4) Gold Datamarts
Gold datasets must support:
- **Machine learning model training** and feature creation
- **Power BI analytics** (performance, refresh patterns, data model structure)

---

## What you need to do:

You will design an end-to-end architecture that satisfies the requirements above, and discuss:
- Streaming ingestion and processing design
- Batch extraction from on-prem databases
- Storage format + layout across layers
- Transformation strategy and orchestration
- Silver layer modeling (SCD Type 2)
- Gold datamarts for ML and BI
- Governance, security, and operational concerns
- Trade-offs (correctness vs latency vs cost vs complexity)

---

## Notes

- This is not a test of memorized tooling.
- You can propose any tools, patterns, or architectures you believe fit best.
- You may use diagrams (whiteboard, Miro, etc.).
- You may ask clarifying questions at any time.
- If you use AI assistance, please submit your conversation history.
- You need to create a private fork repository hosting your solution and share it with us. Remember that if you fork this publicly this will be visible to other applicants.

Thank you — we look forward to the discussion!
