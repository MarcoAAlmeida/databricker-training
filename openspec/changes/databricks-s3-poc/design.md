## Context

We have a Databricks workspace and an AWS account that have not yet been configured to communicate. The sam-analysis pipeline will read thousands of raw text files from S3, so confirming this integration works — and documenting the chosen auth approach — is a prerequisite before any pipeline work begins.

Current state: Databricks workspace exists, AWS account exists, no cross-cloud auth configured.

## Goals / Non-Goals

**Goals:**
- Configure credentials so a Databricks notebook can authenticate to AWS
- Read at least one SAM.gov raw text file from S3 using Spark-native APIs
- Document the auth setup and access pattern for future pipeline work

**Non-Goals:**
- Writing to S3
- Performance or throughput testing
- Building any extraction or transformation logic
- Setting up Delta Lake or a full medallion pipeline

## Decisions

### Auth approach: Databricks Secrets + Access Keys (for POC)

**Chosen:** Store AWS access key ID and secret in a Databricks secret scope, reference them in the notebook via `dbutils.secrets.get()`, and pass them to Spark via `spark.conf.set()`.

**Alternatives considered:**
- *IAM instance profile* — recommended for production; requires workspace admin to attach the profile to the cluster. More setup overhead for a POC.
- *Unity Catalog external location* — the modern recommended path; requires Unity Catalog to be fully configured in the workspace. Too much setup for a validation step.

**Migration note:** After the POC succeeds, the production pipeline should switch to an IAM instance profile or Unity Catalog external location. Access keys in secrets are a temporary shortcut.

### File access pattern: Spark-native (`spark.read.text`)

**Chosen:** Use `spark.read.text("s3a://bucket/path/")` after configuring Hadoop S3A credentials via `spark.conf.set`. This integrates naturally with future Delta Lake work and avoids adding `boto3` as a dependency.

**Alternatives considered:**
- *`dbutils.fs.ls` + Python `open()`* — works but bypasses Spark; won't scale to the full dataset.
- *boto3* — fine for scripting but adds a dependency and doesn't benefit from Spark parallelism.

### Notebook location: `sam-analysis/notebooks/`

The POC notebook belongs in the sam-analysis subproject since it validates infrastructure for that pipeline. Named `poc_s3_connectivity.py` (Databricks Python notebook format).

## Risks / Trade-offs

- **Access keys less secure than IAM profiles** → Acceptable for a short-lived POC; document the migration path explicitly so it doesn't linger.
- **S3 bucket permissions must allow the IAM user** → Validate bucket policy before running the notebook; missing permissions produce opaque Spark errors.
- **`s3a://` requires Hadoop AWS JARs** → Available on Databricks Runtime by default; no manual install needed, but the Runtime version should be noted.

## Open Questions

- Which S3 bucket and prefix holds the SAM.gov sample files? (Need bucket name to configure the notebook.)
- Is Unity Catalog already enabled in the workspace? If so, an external location may be faster to set up than expected.
