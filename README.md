# Technical Project
## Developer Support Engineer

This project is done in phases to get progressively harder. We do not require the full project to be completed - what we are looking for is your approach to solving problems and your learning process. As part of this project you will be set up with a Codefresh account, and you will have access to our Support team to ask questions. You are welcome to make use of our documentation, blog and support team as part of this process.


## Phase 1 - Setup

1. Wait for the invitation email to Codefresh to arrive, and then follow the invite link to join your interview account
2. Set up GitHub and DockerHub accounts to be used for this interview (or use ones you already have, either way is fine)
3. Using the files for this repository (that are attached to an email in a .zip file to you), set up a new private repository in GitHub with those files
4. Invite paul.oreilly@octopus.com and aleksander.aladov@octopus.com to that GitHub repository
5. Set up a dockerhub account, and attach the GitHub account and DockerHub account to your Codefresh account
    - [Git Provider Integration](https://codefresh.io/docs/docs/integrations/git-providers/)
    - [Docker Registry Integration](https://codefresh.io/docs/docs/integrations/docker-registries/)
6. Set up MiniKube (using Docker as the container engine).
7. Use the Helm based install process to set up a Codefresh runner inside the MiniKube cluster and attach that to your Codefresh account
     - [Codefresh runner install docs](https://codefresh.io/docs/docs/installation/runner/install-codefresh-runner/)
     - [Codefresh runner Helm chart](https://github.com/codefresh-io/venona/tree/release-1.0/charts/cf-runtime)
     - Customise your deployment to allow the runner to use more memory and CPU than the defaults, and keep a copy of your the values.yaml file you create to do so.


## Phase 2 - YAML Validation

1. Inside of Codefresh, set up a new project called "Interview"
2. Create a new pipeline called "Build"
3. In the view with the yaml, switch to "Use YAML from Repository" and use the codefresh.yaml in your repository for the pipeline.
6. Fix the YAML Validation Errors

## Phase 3 - Misconfigurations

1. Working Directory needs fixed
    - [working-directories](https://codefresh.io/docs/docs/codefresh-yaml/what-is-the-codefresh-yaml/#working-directories)
2. Update the Push to Docker Registry to use your DockerHub account (hint: less is more)
    - [Build Step](https://codefresh.io/steps/step/build)
    - [Build Docs](https://codefresh.io/docs/docs/codefresh-yaml/steps/build/)
4. Add Stages to the steps
    - [Pipeline Stages](https://codefresh.io/docs/docs/codefresh-yaml/stages/)
5. Add Git Trigger to trigger only on the `feature` branch
    - [Git Triggers](https://codefresh.io/docs/docs/configure-ci-cd-pipeline/triggers/git-triggers/)
6. Modify the trigger to only the Web Folder
    - [Modified files field](https://codefresh.io/docs/docs/configure-ci-cd-pipeline/triggers/git-triggers/#using-the-modified-files-field-to-constrain-triggers-to-specific-folderfiles)
7. Change Hardcoded items into variables
    - [Variables](https://codefresh.io/docs/docs/codefresh-yaml/variables/)
8. Add an annotation that the build was a success

## Phase 4 - Kubernetes

1. Create a "Deploy via Kuberentes" pipeline, and inside of this:
2. Create a dynamic Namespace based on branch and commit ID
3. Deploy the `kube-deployment.yml` from the root of the repo
4. Add a conditional step called "Successful review"
5. Use a pipeline hook step to take everything down again when the pipeline is complete

### Related Items

[Deploy Step](https://codefresh.io/steps/step/deploy)
[Deploy Docs](https://codefresh.io/docs/docs/codefresh-yaml/steps/deploy/)

## Phase 5 - Schedule the Next Interview

1. Email, slack or make contact for the next interview. Additional instructions on who to contact and contact methods will be in your email
2. Continue to work on the below steps

## Phase 6 - Fault Tolerance

In the "Deploy via Kuberentes" pipeline from Phase 4:
1. Add a step before taking down the deployment to Curl the Endpoint of the "web" service
2. Add fault tolerance to that step
3. Add fault tolerance to the unit test to not fail the build if that step fails

## Phase 7 - Helm (bonus)

1. Create a "Deploy via Helm" pipeline
2. Add a Helm Step to deploy the "wordsmith" chart
3. Change the custom values for web, db, words to be the image that was built and pushed
4. Deploy it to a namespace that has been dynamically created for testing
5. Destroy everything after the curling the endpoint

## Phase 8 - Documentation (bonus points!)

1. Write up the technical steps you went through for this test as if you were explaining the process to a customer via email, blog post, or similar.
2. Add this writeup to your code repo in a "blog" folder, which will hold your article in markup, and any other assets.
3. Update this blog folder with new information for the below steps, if you do them.

## Phase 9 - Hybrid GitOps Runtime Deployment (bonus points!)

1. Following the [GitOps Runtime Install Documentation](https://codefresh.io/docs/docs/installation/gitops/hybrid-gitops-helm-installation/) add a GitOps runtime to your MiniKube cluster

## Phase 10 - GitOps Deployment (bonus points!)

1. Using the Helm chart created in Phase 7 and the GitOps runtime just deployed, set up a new Helm chart based GitOps application to automatically deploy the example application into your MiniKube environment.

## Phase 11 - Update your CI process (bonus points!)

1. Update your Codefresh CI pipeline so that when a new build is made on the production branch, it automatically updates a deployment repository which Codefresh GitOps (Argo) uses to deploy your application
