# Prerequistes

* [Create GitHub Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)
  * Note: This token ONLY needs ['read:packages' permission](https://docs.github.com/en/packages/learn-github-packages/about-github-packages#authenticating-to-github-packages), and should be deleted from GitHub account after docker image build.
  * This token is needed when pulling images from 'docker.pkg.github.com'.

# Usage

## Docker image build and pull.
Build images locally with `build_images.sh`, which pulls Docker images from Docker hub.  
Pull images from 'docker.pkg.github.com' with `pull_github_pkg.sh`.

## Docker image push.
To push to the ECR repository of default AWS account, run `push_images.sh`.  
To push to the ECR repository pass in a complete ECS url `push_images.sh ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${REPOSITORY}`. 



This commit adds a Codebuild CI to the public interface of ACCP's 
Github repo. The current plan is to merge this code into the main
`develop` branch first, then deploy the CI using the code defined
in `develop`.

This CI is a stripped down version of [AWS-LC's current CI 
dimension](https://github.com/awslabs/aws-lc/tree/main/tests/ci),
with tests and the docker image to fit ACCP's needs. 
The CI uses AWS CDK and the AWS CLI to deploy CI resources. After 
the CI is deployed, each pull request opened and updated will
kick off a Codebuild batch. The Codebuild batch is defined at
`tests/ci/cdk/cdk/codebuild/github_ci_linux_x86_omnibus.yaml`,
which runs under the dockers defined in `tests/ci/docker_images` 
and uses various test shell scripts defined in `tests/ci`.

Specifics on how to run and deploy the CI via the cdk script can 
be found at `tests/ci/cdk/README.md` and more specifics should 
be added to an internal runbook. 
Note: We still need to add a bot user to access this repo from 
ACCP's  internal AWS account before we can deploy the CI, once 
this gets merged (described in the README).