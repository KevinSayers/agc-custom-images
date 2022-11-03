# Changes 

* pinned version of tabulate versions in `setup.py` installed currently are incompatible
* Modified `aws_batch.py` to add `priviliged` container options.
* Modified `Dockerfile` to copy `setup.py` and `aws_batch.py`

## Setup
* Build image and push to ECR 
* Export the following
```
export ECR_SNAKEMAKE_ACCOUNT_ID=<some-value>
export ECR_SNAKEMAKE_REGION=<some-value>
export ECR_SNAKEMAKE_TAG=<some-value>
export ECR_SNAKEMAKE_REPOSITORY=<some-value>
```
* Deploy context. 
* 

## Testing
```
cd agc-test
agc context deploy -c singularity
agc workflow run test -c singularity
```