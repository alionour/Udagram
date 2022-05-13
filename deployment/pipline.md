
# Pipeline Process

The pipeline is setup and connected with this GitHub repository in CircleCI to give us more confidence about our application before being deployed.
Pipline gives:
- **Speed**: Automatically performing all the steps of a pipeline is faster than doing it manually each time
- **Finding bugs**: By running tests each time we are trying to deploy, we are able to find bugs earlier.
## Continuous Integration
**Continuous integration** is a DevOps software development practice where developers regularly merge their code changes into a central repository, after which automated builds and tests are run. Continuous integration most often refers to the build or integration stage of the software release process and entails both an automation component (e.g. a CI or build service) and a cultural component (e.g. learning to integrate frequently). The key goals of continuous integration are to find and address bugs quicker, improve software quality, and reduce the time it takes to validate and release new software updates.

## Continuous Delivery
**Continuous Delivery** is the ability to get changes of all types—including new features, configuration changes, bug fixes and experiments—into production, or into the hands of users, safely and quickly in a sustainable way.
##### CircleCI version
This is simply indicating which version of the platform our pipeline should use and current version is 2.1. 
```yml
version: 2.1
```
##### CircleCI ORBS
**orbs** are a set of instructions created by CircleCi that allow us to configure the pipeline on which we will run our actions. These instructions will instruct the server to setup specific software on the server executing our pipeline. We used it setup node.js, install the AWS CLI, browser tools for the purpose of testing and elaticbeanstalk cli. 

```yml
orbs:
  aws-cli: circleci/aws-cli@3.1.1
  node: circleci/node@5.0.2
  browser-tools: circleci/browser-tools@1.3.0
  eb: circleci/aws-elastic-beanstalk@2.0.1
```
##### CircleCI JOBS
Jobs are groups of commands that we want to run.
we created a job called **build** that install node then checks out the code from master branch then setup aws-cli , chrome ,chromedriver and elasticbeanstalk with consideration that 
- **aws cli** is required to deploy the frontend application.
- **eb cli** is required to deploy the backend application.
- **chrome** is required to test the frontend application.

```yml
      # install node
      - node/install
      # checks the code at master
      - checkout
      # setup aws cli
      - aws-cli/setup
      # setup chrome and chromedriver
      # to run tests
      - browser-tools/install-chrome
      - browser-tools/install-chromedriver
      # setup elastic beanstalk
      # to deploy backend
      - eb/setup
```
Restore cache if exists by comparing package.json
```yml
- restore_cache: 
    keys: 
    - v1-dependencies-{{ checksum "package.json" }}
    - v1-dependencies-
```
Install the required packages required by both frontend and backend applications
```yml
      - run:
          name: Backend install
          command: |
            npm run backend:install
      - run:
          name: Frontend install
          command: |
            npm run frontend:install
```
Save chache of the packages installled
```yml
      - save_cache:
          paths:
            - "./udagram-api/node_modules"
            - "./udagram-frontend/node_modules"
          key: v1-dependencies- 
```
Build both frontend and backend applications
```yml
# build frontend
- run:
    name: Frontend build
    command: |
    npm run frontend:build
# run karma tests
- run:
    name: Frontend test
    command: |
    npm run frontend:test
```
Install angular cli to run tests
```yml
# install angular cli to run karma tests
- run:
    name: Install angularcli
    command: npm install -g @angular/cli@latest
```
Run frontend tests
```yml
      # run karma tests
      - run:
          name: Frontend test
          command: |
            npm run frontend:test

```
Frontend & Backend deploy considered as continuous delivery

```yml
       # deploy backend to elastic beanstalk
      - run:
          name: Backend Deployment 
          command: |
            npm run backend:deploy
      # deploy frontend to s3
      - run:
          name: Deployment
          command: |
            npm run frontend:deploy
```
#### CIRCLECI WORKSFLOWS
Workflows are instructions about the order of the jobs. They allow us to create complex flows and specify manual approvals. Workflows are not always present in a pipeline.
Runs build job if only changes have been made to master branch as specified by filter
```yml
workflows:
  build:
    jobs:
      - build:
          filters:
            branches:
              only: master
```