## 1. AWS Setup

- [ ] 1.1 Create an S3 bucket (or identify the existing one) and upload at least one sample SAM.gov raw text file
- [ ] 1.2 Create an IAM user with read-only access to the bucket and generate access key ID + secret

## 2. Databricks Workspace Configuration

- [ ] 2.1 Install and authenticate the Databricks CLI against the workspace
- [ ] 2.2 Create a Databricks secret scope (e.g., `aws-poc`)
- [ ] 2.3 Store the AWS access key ID in the secret scope (`aws-poc/access-key-id`)
- [ ] 2.4 Store the AWS secret access key in the secret scope (`aws-poc/secret-access-key`)

## 3. POC Notebook

- [ ] 3.1 Create `sam-analysis/notebooks/poc_s3_connectivity.py`
- [ ] 3.2 Add a setup cell that reads credentials from the secret scope and configures `spark.conf` with the S3A Hadoop settings
- [ ] 3.3 Add a validation cell that lists files at the S3 path using `dbutils.fs.ls("s3a://...")` and prints the count
- [ ] 3.4 Add a read cell that loads the first file with `spark.read.text(...)` and prints the line count and first 5 lines
- [ ] 3.5 Add a summary cell capturing Databricks Runtime version, S3 path, file count, and result

## 4. Verification

- [ ] 4.1 Run the notebook end-to-end on a cluster and confirm no errors
- [ ] 4.2 Confirm the credential error path: temporarily break the secret name and verify the error message is clear

## 5. Documentation

- [ ] 5.1 Create `sam-analysis/docs/s3-connectivity.md` documenting the auth setup steps, the access pattern used, and the POC result
- [ ] 5.2 Register `s3-connectivity.md` in `sam-analysis/docs/mkdocs.yml` nav
