## Why

Before building the sam-analysis pipeline, we need to confirm that a Databricks notebook can authenticate to AWS and read raw text files from S3. Without this validated, any pipeline work risks being built on unconfirmed assumptions about auth and access.

## What Changes

- A new Databricks notebook in `sam-analysis/notebooks/` that mounts (or directly accesses) an S3 bucket and reads a sample file
- Documentation of the chosen auth approach and any workspace configuration required
- A smoke-test result captured in `sam-analysis/docs/` confirming the integration works

## Capabilities

### New Capabilities

- `databricks-s3-connectivity`: Read access from a Databricks notebook to an S3 bucket — auth configuration, file access pattern, and basic validation

### Modified Capabilities

*(none)*

## Impact

- Databricks workspace: requires IAM or access key configuration
- AWS: requires an S3 bucket with at least one sample SAM.gov text file
- `sam-analysis/notebooks/`: new notebook added
- `sam-analysis/docs/`: new page documenting the POC result
- `requirements.txt`: no changes expected (notebook deps managed by Databricks cluster)
