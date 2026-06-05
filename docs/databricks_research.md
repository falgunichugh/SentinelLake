# Databricks Research Notes

## What is Databricks?
Databricks is a cloud-based Data Intelligence Platform built on Apache Spark that enables organizations to process, analyze, and manage large-scale data. It provides a unified environment for data engineering, data science, machine learning, and business intelligence.
Databricks follows a Lakehouse Architecture, combining the scalability and flexibility of a Data Lake with the performance and governance capabilities of a Data Warehouse.

Key capabilities include:
- Data ingestion and ETL/ELT pipeline development
- Large-scale data processing using Apache Spark
- Data storage and management through Delta Lake
- Machine Learning and AI model development
- Data governance and access control through Unity Catalog
- Collaborative notebook-based development

For SentinelLake, Databricks serves as the primary platform from which audit logs, system tables, workflow events, and data access activities can be monitored and analyzed.

## What are Databricks Audit Logs?
Databricks Audit Logs are records of activities performed within a Databricks workspace. They provide visibility into user actions, administrative changes, and platform events for monitoring, security, governance, and compliance purposes.
Audit logs can capture events such as:
- User authentication and login activities
- Permission and access control changes
- Cluster creation, modification, and deletion
- Job and workflow execution events
- Notebook and workspace activities
- Data access and query-related events (depending on configuration)

For SentinelLake, audit logs can serve as a primary source of security telemetry. They can be analyzed to identify suspicious user behavior, unauthorized access attempts, unusual activity patterns, and other security risks.

## What are Databricks System Tables?
Databricks System Tables are predefined tables maintained by Databricks that store metadata and operational information about a workspace, account, and platform activities.
These tables provide a centralized and queryable source of information that can be accessed using SQL, making it easier to monitor, analyze, and govern Databricks environments.

System Tables can contain information related to:
- Audit events
- User activity
- Job and workflow executions
- Cluster usage
- Billing and cost data
- Data access patterns
- Unity Catalog operations

Unlike traditional log files, System Tables allow users to query operational data directly using SQL and integrate it into monitoring and reporting solutions.
System Tables can serve as a primary data source for SentinelLake. They can be used to:
- Monitor user access activities
- Track workflow and job executions
- Detect failed or delayed pipelines
- Analyze access patterns
- Generate risk scores
- Build dashboards and reports
Because System Tables are queryable using SQL, they provide a structured and scalable way to collect monitoring data within Databricks environments.

User accesses sensitive table
        ↓
Databricks records event
        ↓
System Table stores event
        ↓
SentinelLake queries System Table
        ↓
Risk Engine evaluates activity
        ↓
Dashboard/Alert generated

system.access.audit contains audit logs. For SentinelLake, this table can be used to detect suspicious user behavior, privilege changes, and unusual activity patterns.
system.lakeflow.jobs and system.lakeflow.job_run_timeline can track job execution. 

### Potential Data Sources for SentinelLake MVP

| SentinelLake Feature | Databricks Data Source |
|---------------------|------------------------|
| User Activity Monitoring | system.access.audit |
| Suspicious Access Detection | system.access.audit |
| Pipeline Monitoring | system.lakeflow.jobs |
| Job Failure Detection | system.lakeflow.job_run_timeline |
| Risk Scoring | Audit + Job Data |
| Dashboard Metrics | System Tables |

## What is Unity Catalog?
Unity Catalog is Databricks' centralized data governance solution that manages access control, security, auditing, and metadata across data assets.
It provides a unified way to govern data, AI assets, and analytics resources within Databricks environments.

Key capabilities include:
- Centralized access control
- Data discovery and metadata management
- Data lineage tracking
- Auditing and monitoring
- Fine-grained permissions
- Data classification and governance
Unity Catalog helps organizations ensure that only authorized users can access specific data assets while maintaining visibility into how data is being used across the platform.
### Relevance to SentinelLake
Unity Catalog can help SentinelLake identify:
- Which datasets are considered sensitive
- Who has permission to access datasets
- Who actually accessed datasets
- Changes in permissions and privileges
- Data lineage and impact analysis
This information can be used to enhance risk scoring and detect unauthorized or suspicious access patterns.
Unity Catalog provides the governance context that allows SentinelLake to determine whether a data access event is normal, authorized, and low-risk or potentially suspicious and high-risk.

## What Events Can SentinelLake Monitor?
SentinelLake can monitor both security-related and operational events occurring within a Databricks environment.
### Security Events
- Unauthorized access attempts
- Access to sensitive datasets
- Access outside normal business hours
- Privilege or permission changes
- New user or device activity
- Excessive data exports
- Unusual query patterns
- Repeated failed login attempts
- Administrative configuration changes

### Operational Events
- Job failures
- Pipeline execution failures
- Delayed workflows
- Long-running jobs
- Data freshness issues
- Schema changes
- Missing data loads
- Data quality anomalies

### Governance Events
- Changes in access permissions
- Changes in data ownership
- Creation of new catalogs, schemas, or tables
- Changes to Unity Catalog policies
- Audit log events indicating policy violations


## Questions

### Where does Databricks store audit events?
Databricks stores audit-related information in System Tables, particularly:
- system.access.audit

This table contains records of user and workspace activities such as:
- Logins and authentication events
- Permission changes
- Administrative actions
- Data access activities
- Workspace operations
Audit events can be queried using SQL, making them a valuable data source for monitoring and security analytics.

### Relevance to SentinelLake
SentinelLake can use audit events to:
- Track user activity
- Detect suspicious access patterns
- Monitor privilege changes
- Generate risk scores

### Can pipeline failures be monitored?
Yes.
Databricks provides information about jobs, workflows, and pipeline executions through System Tables such as:

- system.lakeflow.jobs
- system.lakeflow.job_run_timeline

These tables can provide information about:
- Job status
- Execution duration
- Start and end times
- Failure events
- Retry attempts

### Relevance to SentinelLake
SentinelLake can use this information to:
- Detect failed jobs
- Identify delayed pipelines
- Monitor long-running workflows
- Generate operational risk alerts

### Can user access be tracked?
Yes.
User access activities can be tracked through:
- system.access.audit
- Unity Catalog

These sources provide visibility into:
- Who accessed data
- What data was accessed
- When access occurred
- Permission and role information

### Relevance to SentinelLake
SentinelLake can use this information to:
- Detect unauthorized access attempts
- Monitor sensitive dataset access
- Identify after-hours activity
- Detect anomalous user behavior
- Generate security risk scores

## Key Findings
SentinelLake does not need direct access to business data.
Instead, it can leverage Databricks metadata and monitoring sources, including:
- system.access.audit
- system.lakeflow.jobs
- system.lakeflow.job_run_timeline
- Unity Catalog


These sources provide sufficient information to monitor user activity, data access patterns, permission changes, and pipeline execution risks without requiring access to sensitive client data.
