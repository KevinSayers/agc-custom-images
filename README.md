# Intro

This repo contains information on using a custom container image with the Amazon Genomics CLI. 

## Custom Nextflow version

### Build Image
The AGC repo contains a Dockerfile for each of the workflow engines. The [Nextflow](https://github.com/aws/amazon-genomics-cli/tree/main/packages/engines/nextflow) image for example can be used to build with a specified version of Nextflow. 

Specify the Nextflow version using `--build-arg` as below. 
```
docker build -t nextflow --build-arg NEXTFLOW_VERSION=22.10.4 .
```    

### Upload to ECR
* Create an ECR repository
* Run the following commands, which are also listed under the `View push commands` in the ECR repository when viewed in the AWS Console. 
```
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com

docker tag nextflow:latest <ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/nextflow:latest

docker push <ACCOUNT ID>.dkr.ecr.us-east-1.amazonaws.com/nextflow:latest
```

### Set the necessary env vars
These values are obtained from the above ECR repository URL. 
```
export ECR_NEXTFLOW_ACCOUNT_ID=<some-value>
export ECR_NEXTFLOW_REGION=<some-value>
export ECR_NEXTFLOW_TAG=<some-value>
export ECR_NEXTFLOW_REPOSITORY=<some-value>
```

### Redeploy the context
It is necessary to run `agc context deploy -c <your context>` after the custom image has been pushed and the env vars have been set. 

## Snakemake Singularity install
This uses a modified Dockerfile contained in this repo, and modifies how AGC runs in this container. 

### Changes 

* pinned version of tabulate versions in `setup.py` installed currently are incompatible
* Modified `aws_batch.py` to add `priviliged` container options.
* Modified `Dockerfile` to copy `setup.py` and `aws_batch.py`

### Setup
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

### Testing
```
cd agc-test
agc context deploy -c singularity
agc workflow run test -c singularity
```