# Research Notes — Data Engineering Applied to AI

## 1. Research Overview

This document consolidates research notes on the role of data engineering in AI and machine learning systems.

The project explores how data is ingested, transformed, validated, stored, and made available to downstream analytical and AI workflows. It also examines workflow orchestration and the operational practices required to build reliable, maintainable data pipelines.

The research combines educational materials, technical tutorials, and official documentation. Concepts are documented progressively, with distinctions between source-supported findings, technical interpretations, and topics that still require verification.

**Current stage:** Research and conceptual analysis.

**Implementation status:** No end-to-end pipeline has been implemented or tested as part of this research stage.

## 2. Research Objectives

The main objectives are to:

- Understand the fundamental concepts of data engineering and data pipelines.
- Examine ingestion and transformation as stages of a data workflow.
- Understand workflow orchestration, task dependencies, scheduling, and failure handling.
- Investigate how data quality and pipeline reliability affect AI and machine learning workflows.
- Compare relevant tools and architectural approaches without assuming that one solution fits every scenario.
- Identify concepts that require verification against current official documentation.
- Establish a foundation for a later practical experiment with reproducible steps and documented results.

## 3. Research Scope

### 3.1 Topics Included

The research is organized around the following topics:

1. Data engineering fundamentals.
2. Data ingestion and transformation.
3. Data storage and management.
4. Workflow orchestration.
5. Data quality and pipeline reliability.
6. Data engineering in AI and machine learning workflows.
7. Practical applications and potential experiments.

### 3.2 Scope Boundaries

This is a progressive learning project rather than a production-ready platform.

The current research does not claim to provide:

- A complete production architecture.
- Benchmarks comparing pipeline performance.
- Verified cloud deployment configurations.
- Financial cost comparisons between orchestration tools.
- An implemented machine learning pipeline.
- Empirical evidence of performance or reliability improvements.

These topics may be explored later if they become relevant to the practical experiment.

## 4. Research Methodology

The research process follows these steps:

1. **Source selection:** collect educational materials, tutorials, and official technical documentation.
2. **Initial synthesis:** organize concepts and questions using NotebookLM.
3. **Source validation:** compare synthesized claims against the selected materials.
4. **Technical verification:** consult official documentation for version-sensitive behavior and implementation details.
5. **Documentation:** record findings, limitations, open questions, and source references.
6. **Practical application:** design and implement a small experiment after the conceptual research is sufficiently developed.

### 4.1 Evidence Classification

Research findings should be interpreted using the following categories:

- **Source-supported:** directly supported by an identified source.
- **Verified technical detail:** checked against relevant official documentation.
- **Conceptual example:** illustrates an architecture or workflow but has not necessarily been implemented.
- **Requires further research:** plausible or relevant, but not sufficiently supported by the available sources.
- **Implemented and tested:** reserved for functionality that has actually been built and tested, with the procedure and results documented.

A claim should not be considered verified merely because it sounds technically plausible.

## 5. Key Concepts

This section will consolidate the foundational concepts identified throughout the research.

### 5.1 Data Pipelines

A data pipeline is a sequence of operations that moves data through one or more processing stages. Depending on its purpose, it may include ingestion, validation, transformation, storage, and delivery to downstream consumers.

Pipelines may be implemented as scripts, scheduled jobs, orchestrated workflows, streaming applications, or combinations of these approaches.

The appropriate architecture depends on factors such as data volume, execution frequency, reliability requirements, complexity, and operational constraints.

**Research status:** Preliminary conceptual framing; further source-based development is pending.

### 5.2 ETL and ELT

ETL stands for Extract, Transform, Load: data is extracted from a source, transformed, and then loaded into a target system.

ELT stands for Extract, Load, Transform: data is extracted and loaded before transformations are performed within the target environment or a connected processing system.

The distinction concerns where transformation occurs in the workflow. Neither approach is universally preferable; the choice depends on the data platform, processing requirements, governance, and intended use.

**Research status:** Preliminary conceptual framing; implementation details and tool-specific examples remain to be researched.

### 5.3 Data Quality and Reliability

Data quality concerns whether data is suitable for its intended use. Relevant dimensions may include completeness, validity, consistency, uniqueness, and timeliness.

Pipeline reliability also depends on execution behavior, error handling, observability, and the ability to recover safely from failures.

These concepts are particularly relevant when downstream consumers include machine learning models or AI applications.

**Research status:** Initial framing; specific validation techniques and tools require further research.

## 6. Workflow Orchestration

### 6.1 Definition and Purpose

Workflow orchestration coordinates the execution of tasks that form a larger workflow. It can define execution order, dependencies, schedules, task states, failure handling, and monitoring.

Without an orchestration layer, independent scripts and traditional scheduling mechanisms can become difficult to coordinate as workflows grow. Dependencies may remain implicit, failures can be harder to trace, and retries and execution monitoring may require additional manual work.

An orchestrator is not mandatory for every data workflow. A small, straightforward process may be adequately managed with a well-structured script and a simple scheduler.

The decision to adopt an orchestration platform should be based on actual operational needs rather than complexity for its own sake.

### 6.2 Pipeline, Task, and DAG

These concepts describe different levels of a workflow:

- **Pipeline:** the overall process that delivers a result, potentially spanning multiple systems and stages.
- **Task:** an individual unit of work, such as downloading a file, validating a dataset, or triggering a transformation.
- **DAG:** a Directed Acyclic Graph. In orchestration, it represents tasks and their dependency relationships without directed cycles.

A DAG is a way to represent workflow structure; it is not synonymous with the entire data platform or necessarily with every operation in a broader end-to-end pipeline.

### 6.3 Dependencies and Execution

Tasks can depend on the successful completion or state of other tasks. Declaring these dependencies allows the orchestration system to determine which tasks are eligible to run.

Scheduling determines when a workflow should be considered for execution. Depending on the tool and configuration, workflows may be triggered by time-based schedules, external events, data availability, or manual requests.

Retries can help recover from transient failures. They should be configured with awareness of the operation being retried: repeating a task is not automatically safe if it creates duplicate records, repeats a financial transaction, or leaves partially written data.

Failure handling and monitoring help operators understand whether a workflow completed, failed, or requires intervention.

### 6.4 Apache Airflow

Apache Airflow is an open-source platform for authoring, scheduling, and monitoring workflows programmatically.

In Airflow, DAG definitions describe the workflow structure, while tasks represent individual operations. The platform coordinates task execution and records execution state.

Airflow is primarily an orchestration system, not a general-purpose distributed data-processing engine. Heavy transformations may be delegated to specialized systems such as Spark, cloud data warehouses, or other processing services. Airflow can coordinate these operations without needing to perform all the underlying computation itself.

The exact architecture depends on the Airflow version, executor, and deployment model. A description of one local setup should not be treated as a universal architecture.

### 6.5 Airflow Components and Configuration

The learning materials introduce several important components and concepts:

- **Scheduler:** determines which tasks and workflow runs are eligible for scheduling.
- **Web interface:** provides a way to inspect workflows, task states, and execution information.
- **Metadata database:** stores orchestration metadata, including information about workflow runs and task instances.
- **Executor and execution infrastructure:** determine how tasks are executed and how execution capacity is managed.
- **Operators and tasks:** define units of work and may encapsulate common operations or integrations.
- **Connections and configuration:** provide settings needed to interact with external systems, including connection details and authentication where applicable.

The exact component names, process layout, and execution model vary by version and deployment. For example, some configurations use separate worker processes, while others execute tasks locally.

SQLite may be useful in development or testing contexts, but its suitability should not be generalized to production deployments. Database selection depends on the supported deployment architecture and operational requirements.

Operators can simplify interactions with external systems, but they do not universally eliminate the need to configure permissions, connections, or authentication.

### 6.6 Authoring Workflows and Version-Specific Details

Airflow supports more than one approach to defining tasks.

The traditional approach includes operators such as `PythonOperator` and `BashOperator`. The TaskFlow API provides a Python-decorator-based approach, including `@dag` and `@task`, for defining workflows and tasks.

These approaches should not be described as universally interchangeable in every situation, nor should classic operators be assumed to be obsolete.

Other details require version-aware verification:

- Database initialization and migration commands.
- Scheduling arguments and supported scheduling mechanisms.
- Executor behavior and deployment requirements.
- Changes to component architecture between major versions.

The official documentation for the selected version should be used when writing implementation examples. Older tutorials remain useful for understanding concepts, but their commands and configurations may not match a current installation.

### 6.7 Conceptual Example: Batch Data Workflow

A conceptual batch pipeline might contain the following stages:

1. Retrieve a source file, such as a CSV dataset.
2. Validate basic file and schema expectations.
3. Store the original data in a designated location.
4. Trigger a transformation in a suitable processing environment.
5. Validate the transformed dataset.
6. Publish the output for downstream analytical or AI workflows.
7. Record execution status and notify an operator if intervention is required.

An orchestrator could coordinate the dependencies and execution of these stages.

This example is illustrative only. It does not represent an implemented pipeline, a validated cloud architecture, or a claim that a specific combination of tools has been tested.

### 6.8 Orchestration in AI and Machine Learning

AI and machine learning workflows often involve several operational stages, including data preparation, validation, training, evaluation, and delivery of model artifacts.

An orchestrator can coordinate these stages and their dependencies. It may also trigger data-quality checks or downstream tasks when required conditions are met.

However, orchestration is only one part of an ML system.

It is useful to distinguish:

- **Workflow orchestration:** coordinates task execution and dependencies.
- **Experiment tracking:** records experiment parameters, metrics, and related outputs.
- **Data validation:** checks whether data meets specified expectations.
- **Feature management:** organizes and serves features for machine learning use cases.
- **Model serving and monitoring:** concern the deployment and behavior of models after training.

Tools such as MLflow, Weights & Biases, and TensorBoard are associated with experiment tracking and related model-development workflows. Their exact capabilities differ, and they do not replace the general role of an orchestrator in coordinating a multi-step pipeline.

Feature stores, data drift monitoring, and advanced ML deployment workflows are relevant extensions of this topic, but they require dedicated research before detailed technical claims are made.

### 6.9 Practices and Trade-offs

The research suggests several principles for designing maintainable workflows:

- Avoid introducing an orchestration platform when a simpler solution adequately meets the requirements.
- Keep tasks focused and make dependencies explicit.
- Configure retries and timeouts according to the behavior of each task.
- Consider idempotency and partial failures before enabling automatic retries.
- Validate data at meaningful pipeline boundaries.
- Keep heavy computation in appropriate processing systems rather than treating the orchestrator as the processing engine.
- Make execution state and failures observable.
- Select tools according to the workflow's complexity, deployment constraints, team needs, and maintenance costs.
- Verify version-sensitive behavior against official documentation.

Prefect, Mage, and Kestra are independent workflow orchestration tools, not Airflow plugins. Their suitability should be evaluated against concrete requirements; claims that one is always simpler or cheaper require supporting evidence.

### 6.10 Limitations of the Current Research

The selected materials do not provide sufficient evidence for definitive conclusions about:

- Detailed production configurations for managed cloud services such as AWS MWAA or Google Cloud Composer.
- Advanced production executor configurations such as Kubernetes-based execution.
- Exact financial cost comparisons among orchestration platforms.
- Universal performance comparisons between orchestration tools.
- A production-ready reference architecture applicable to all environments.

These topics remain outside the current validated scope.

### 6.11 Open Questions

Further research should address:

1. How should data-quality checks be incorporated into a pipeline?
2. How can retries be designed to avoid duplicate or inconsistent outputs?
3. Which monitoring signals help distinguish execution failures from changes in data quality?
4. When is a dedicated orchestrator justified over a simpler scheduled process?
5. Which Airflow version and execution model are appropriate for a small reproducible experiment?
6. How should orchestration be integrated with experiment tracking without duplicating responsibilities?

### 6.12 Research Status

The initial synthesis of workflow orchestration has been reviewed against the selected learning materials, and selected version-sensitive details have been identified for verification.

The next step is to continue researching data ingestion and transformation before designing the practical experiment.

No orchestration workflow has been implemented or execution-tested as part of this project at this stage.

## 7. Data Ingestion and Transformation

**Status: Pending research.**

This section will be developed after reviewing the relevant sources.

Planned topics include:

- Batch and streaming ingestion.
- File-based, API-based, and database-based data sources.
- Schema handling and data type consistency.
- Missing values, duplicates, and invalid records.
- Transformation logic and reproducibility.
- Data validation and error handling.
- How ingestion and transformation decisions affect downstream AI and machine learning workflows.

No specific implementation approach has been selected or validated yet.

## 8. Data Storage and Management

**Status: Pending research.**

This section will examine how data is stored and managed throughout a pipeline.

Planned topics include:

- Raw, intermediate, and processed data.
- Data warehouses, data lakes, and related storage approaches.
- File formats and schema considerations.
- Data organization, accessibility, and reproducibility.
- Storage choices for analytical and AI workloads.

Tool comparisons and architectural recommendations will be added only after the relevant sources have been reviewed.

## 9. Data Quality and Pipeline Reliability

**Status: Pending research.**

This section will investigate how to identify and handle data problems throughout a workflow.

Planned topics include:

- Schema and type validation.
- Completeness, uniqueness, and validity checks.
- Error handling and recovery.
- Idempotency and safe retries.
- Logging, monitoring, and alerting.
- The distinction between task execution success and data correctness.

The practical application of these concepts will be documented after the corresponding research and experiments.

## 10. Source Comparisons

The selected materials cover complementary perspectives:

- **Educational lectures:** introduce conceptual foundations and engineering trade-offs.
- **Practical tutorials:** demonstrate implementation patterns that may be specific to a tool version or environment.
- **Official documentation:** supports verification of current behavior, supported configuration, and version-specific details.
- **Alternative orchestration projects:** provide context for comparing different approaches, but do not establish a universal ranking.

A detailed tool comparison will be added when the research has enough evidence to compare capabilities, operational complexity, limitations, and relevant use cases consistently.

## 11. Key Insights

The following insights summarize the current research stage:

1. Orchestration coordinates tasks and dependencies; it does not necessarily perform the underlying data processing itself.
2. A pipeline, a task, and a DAG describe related but different aspects of a workflow.
3. Retries and scheduling improve automation only when task behavior and failure conditions are considered carefully.
4. An orchestration platform introduces operational overhead and should be chosen based on actual requirements.
5. Data engineering is important to AI workflows because the quality, consistency, and availability of data affect downstream processing and model development.
6. Experiment tracking and workflow orchestration serve complementary purposes.
7. Technical examples must be checked against the version of the tool being used.
8. A conceptual design should not be described as implemented or tested until there is evidence from an actual experiment.

These are research-stage conclusions and may be refined as additional sources are reviewed.

## 12. Practical Applications and Future Experiments

The research may lead to a small, reproducible data pipeline that demonstrates selected concepts.

A future experiment could include:

- A clearly identified input dataset.
- A documented ingestion step.
- Basic transformations and data-quality checks.
- A structured output.
- Explicit task dependencies.
- Execution logs and documented failure behavior.
- Tests that verify the expected output.

The final choice of tools and implementation scope will depend on the findings from the remaining research.

Any implementation will be documented separately under `practical-experiments/`, including its configuration, execution instructions, tests, limitations, and observed results.

## 13. References

The source inventory is maintained in [`sources.md`](./sources.md). The following source IDs identify materials relevant to the current workflow orchestration synthesis.

- **S001:** Full Stack Deep Learning educational material on development infrastructure and tooling. Exact title and relevance should be confirmed against the source inventory.
- **S002:** Full Stack Deep Learning educational material on troubleshooting and testing. Exact title and relevance should be confirmed against the source inventory.
- **S003:** Full Stack Deep Learning — Lecture 04: Data Management (2022).  
  https://fullstackdeeplearning.com/course/2022/lecture-4-data-management/
- **S005:** Apache Airflow tutorial on setting up and orchestrating a data pipeline. See `sources.md` for the selected video URL and exact title.
- **S006–S008:** Data Engineering Zoomcamp materials from 2023, 2024, and 2025. See `sources.md` for the individual URLs and titles.
- **S009:** Alura — Engenharia de Dados: Orquestração de Pipelines com Apache Airflow. See `sources.md` for the course URL.
- **S010:** Full Stack Deep Learning — Lab 04: Experiment Management (2022).  
  https://www.youtube.com/watch?v=NEGDJuINE9E
- **S011:** Full Stack Deep Learning — Lab 05: Troubleshooting & Testing (2022).  
  https://www.youtube.com/watch?v=D65SlCSoS-0
- **S012:** Full Stack Deep Learning — Lab 08: Monitoring (2022).  
  https://www.youtube.com/watch?v=-mKzxSC0r7w
- **S013:** Apache Airflow — Core Concepts, official documentation.  
  https://airflow.apache.org/docs/apache-airflow/stable/core-concepts/index.html

**Reference note:** Source IDs and titles should remain consistent with `sources.md`. The list above identifies materials associated with the current synthesis; it does not imply that every source supports every statement in this document.

## 14. Research Log

| Area | Status | Next action |
|---|---|---|
| Project scope and methodology | Initial draft | Refine as research progresses |
| Key data engineering concepts | Preliminary | Validate and expand with sources |
| Workflow orchestration | Initial synthesis reviewed | Continue version-specific verification as needed |
| Data ingestion and transformation | Pending | Review sources and document findings |
| Data storage and management | Pending | Review sources and document findings |
| Data quality and reliability | Pending | Research validation and recovery practices |
| Practical experiment | Not started | Define after conceptual research |

## 15. Overall Project Status

The project is currently in the research and conceptual analysis phase.

The workflow orchestration section has an initial source-reviewed synthesis, including explicit limitations and version-sensitive questions. The remaining topics will be developed incrementally.

The next planned research area is **Data Ingestion and Transformation**.

Implementation, testing, and performance evaluation will be reported only after they have actually been carried out.
