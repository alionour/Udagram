
# Pipeline Process

The pipeline is setup and connected with this GitHub repository in CircleCI.

## Order of commands

1. The pipeline uses orbs to install Node, the AWS cli ,browser tools and the EB cli .
```yml
orbs:
  aws-cli: circleci/aws-cli@3.1.1
  node: circleci/node@5.0.2
  browser-tools: circleci/browser-tools@1.3.0
  eb: circleci/aws-elastic-beanstalk@2.0.1
```

2. install node then checks out the code from master branch then setup aws-cli , chrome ,chromedriver and elasticbeanstalk
```yml
      - node/install
      - checkout
      - aws-cli/setup
      - browser-tools/install-chrome
      - browser-tools/install-chromedriver
      - eb/setup
```
3. FrontEnd & BackEnd install
4. FrontEnd & BackEnd build
5. Frontend test
6. FrontEnd & BackEnd deploy

```yml
 - restore_cache: 
          keys: 
            - v1-dependencies-{{ checksum "package.json" }}
            - v1-dependencies-
      - run:
          command: |
            google-chrome --version
            chromedriver --version
          name: Check install
      - run:
          name: Backend install
          command: |
            npm run backend:install
      - run:
          name: Frontend install
          command: |
            npm run frontend:install
      - save_cache:
          paths:
            - "./udagram-api/node_modules"
            - "./udagram-frontend/node_modules"
          key: v1-dependencies- 
      - run:
          name: Frontend build
          command: |
            npm run frontend:build
      - run:
          name: Install angularcli
          command: npm install -g @angular/cli@latest
      - run:
          name: Frontend test
          command: |
            npm run frontend:test
      - run:
          name: Backend build
          command: |
            npm run backend:build

      - run:
          name: Deployment
          command: |
            npm run frontend:deploy
```