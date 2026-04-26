## ADDED Requirements

### Requirement: Notebook authenticates to AWS and reads from S3
The notebook SHALL configure Spark with AWS credentials retrieved from a Databricks secret scope and successfully read at least one raw text file from the designated S3 bucket using `spark.read.text`.

#### Scenario: Successful read with valid credentials
- **WHEN** valid AWS credentials are stored in the Databricks secret scope
- **THEN** the notebook reads the target S3 path without error and prints the file count and line count of the first file

#### Scenario: Missing or invalid credentials
- **WHEN** the secret scope is missing or credentials are invalid
- **THEN** the notebook fails with a clear error message identifying the credential problem (not a generic Spark exception)

### Requirement: POC result is documented
The notebook SHALL produce a summary cell (or a separate markdown output) capturing the Databricks Runtime version used, the S3 path accessed, and the result (file count, sample line count).

#### Scenario: Result captured in docs
- **WHEN** the POC notebook runs successfully
- **THEN** findings are recorded in `sam-analysis/docs/` as a new page describing the auth setup, access pattern, and observed result

### Requirement: Auth approach is documented for future use
The design SHALL document the chosen auth method, its limitations, and the migration path to a production-grade approach (IAM instance profile or Unity Catalog external location).

#### Scenario: Design notes reviewed before pipeline work begins
- **WHEN** a developer starts the sam-analysis pipeline
- **THEN** `design.md` for this change provides sufficient context to replicate or upgrade the auth setup without re-investigation
