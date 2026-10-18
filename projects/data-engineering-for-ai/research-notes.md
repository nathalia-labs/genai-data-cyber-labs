# Research Notes

## 1. Research Overview

This document records the concepts, comparisons, insights, and open questions identified during the study of data engineering applied to AI systems.

The notes are based on the sources cataloged in [`sources.md`](sources.md). Claims and conclusions should be linked to the relevant source IDs whenever possible.

## 2. Research Scope

The research focuses on the following topics:

- Data ingestion and transformation
- Data storage and management
- Workflow orchestration
- Data quality, testing, and reliability
- Experiment management and monitoring
- Data pipelines supporting machine learning systems

## 3. Key Concepts

This section will document important concepts, their definitions, practical applications, and relevance to data engineering for AI.

### 3.1 Data Ingestion

**Definition:** To be completed after reviewing the selected sources.

**Key points:**
- To be investigated.

**Sources:** To be added.

### 3.2 Data Transformation

**Definition:** To be completed after reviewing the selected sources.

**Key points:**
- To be investigated.

**Sources:** To be added.

### 3.3 Data Storage and Management

**Definition:** To be completed after reviewing the selected sources.

**Key points:**
- To be investigated.

**Sources:** To be added.

### 3.4 Workflow Orchestration

**Definition:** To be completed after reviewing the selected sources.

**Key points:**
- To be investigated.

**Sources:** To be added.

### 3.5 Data Quality and Testing

**Definition:** To be completed after reviewing the selected sources.

**Key points:**
- To be investigated.

**Sources:** To be added.

### 3.4 Workflow Orchestration

**Definition**

Workflow orchestration is the coordination of tasks, dependencies, execution order, scheduling, and failure handling within a workflow. In data engineering, orchestration helps ensure that pipeline steps run under the appropriate conditions and that their execution can be monitored.

**Key Concepts**

- **Pipeline:** A sequence of processing steps that moves data from source systems toward a destination or intended outcome.
- **Task:** An individual unit of work within a workflow, such as extracting data, running a transformation, or validating a dataset.
- **DAG (Directed Acyclic Graph):** A representation of tasks and their dependencies. The graph is directed and acyclic, allowing the orchestrator to determine valid execution order.
- **Dependencies:** Rules that determine when a task can run based on the state or completion of upstream tasks.
- **Scheduling:** Defines when a workflow should run. Depending on the orchestration platform and configuration, execution may be time-based, manually triggered, or event-driven.
- **Retries and failure handling:** Configured policies determine whether failed tasks should be retried and how execution failures are reported.
- **Monitoring and logging:** Execution states and logs help operators investigate failures and understand workflow behavior.

**Apache Airflow**

Apache Airflow is an open-source platform for defining, scheduling, and monitoring workflows programmatically. Workflows are represented as DAGs, with tasks connected through declared dependencies.

Airflow is primarily responsible for orchestration. Data-intensive processing can be delegated to specialized systems, such as Spark or cloud data processing services, rather than being performed entirely inside the orchestration process.

Its architecture includes a scheduler, DAG processing, a web interface, and a metadata database. The execution architecture depends on the selected executor and deployment configuration; separate workers are not required in every setup.

**Conceptual Pipeline Example**

A batch data pipeline could contain the following tasks:

1. Extract a dataset from an API or public data source.
2. Validate the structure and required fields.
3. Store the raw data in a designated storage layer.
4. Transform the data and load it into analytical tables.
5. Run quality checks and publish the results or trigger a downstream process.

The orchestrator manages task dependencies and execution. The actual extraction, storage, and transformation are performed by the tools or services configured for each task.

This is a conceptual example, not an implemented experiment.

**Relevance to AI/ML Systems**

Data pipelines support machine learning by preparing datasets, validating inputs, and coordinating repeatable workflow steps. Orchestration can also coordinate model training, evaluation, and downstream actions.

Workflow orchestration and experiment tracking serve different purposes. An orchestrator manages task execution and dependencies, while experiment-tracking tools record information such as parameters, metrics, and artifacts.

More advanced topics, including feature stores and data drift monitoring, require additional research before drawing implementation conclusions.

**Good Practices and Limitations**

- Keep tasks focused and define dependencies explicitly.
- Configure retries and timeouts according to the failure modes of each task.
- Validate data before downstream processing.
- Avoid using the orchestrator itself as a substitute for a distributed data processing engine.
- Choose orchestration tools based on project requirements, operational complexity, and team capabilities.
- Verify version-specific examples against official documentation.

**Open Questions**

- Which data quality checks should run before a dataset is used for model training?
- How should retries be configured for tasks that are not safe to execute more than once?
- Which monitoring signals help distinguish pipeline failures from changes in incoming data?
- When is a dedicated orchestrator justified over a simpler scheduled script?

**Sources**

- S001 — Development Infrastructure & Tooling (FSDL 2022)
- S002 — Troubleshooting & Testing (FSDL 2022)
- S003 — Data Management (FSDL 2022)
- S005 — Apache Airflow Tutorial
- S009 — Engenharia de Dados: Orquestração de Pipelines com Apache Airflow
- S013 — Apache Airflow Official Documentation: Core Concepts

**Research Status:** Initial synthesis completed; individual claims and implementation details remain subject to source review and verification.

## 4. Data Engineering for AI/ML

This section will explore how data engineering practices support machine learning workflows, from data preparation to experiment management and monitoring.

**Key questions:**
- What distinguishes a data pipeline for analytics from one supporting machine learning?
- How do data quality and data validation affect ML outcomes?
- How can orchestration and monitoring improve pipeline reliability?

**Sources:** To be added.

## 5. Source Comparisons

Comparisons will be documented after reviewing relevant sources.

| Topic | Source 1 | Source 2 | Main findings |
|---|---|---|---|
| Workflow orchestration | To be added | To be added | Pending research |
| Data management | To be added | To be added | Pending research |
| Testing and reliability | To be added | To be added | Pending research |
| Experiment management and monitoring | To be added | To be added | Pending research |

## 6. Key Insights

Insights will be added as sources are reviewed and their claims are checked.

- No conclusions recorded yet.

## 7. Open Questions

- Which orchestration practices are most useful for reliable data pipelines?
- How should data quality be validated before data is used to train or evaluate ML models?
- Which monitoring practices help detect failures in data pipelines and ML systems?
- Which concepts are tool-independent, and which depend on specific implementations?

## 8. Practical Applications

Potential practical applications will be documented here. Each application should distinguish between proposed ideas and experiments that have actually been implemented.

- No experiments documented yet.

## 9. References

References use the source IDs defined in [`sources.md`](sources.md).

- Sources reviewed: None recorded yet.
- Sources pending review: See `sources.md`.

## 10. Research Methodology

- Record concepts in your own words.
- Link important claims to the corresponding source IDs.
- Distinguish source statements from personal interpretations.
- Verify implementation details against official documentation when available.
- Record disagreements, limitations, and unanswered questions.
- Update the notes as the research progresses.
