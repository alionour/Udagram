# Udagram

This full stack application have complete Pipline **[CI,CD]**.
The udagram application is a fairly simple application that includes all the major components of a Full-Stack web application.


![Architecture](./deployment/architecture.png)
## Getting Started

1. Clone this repo locally into the location of your choice.
1. Move the content of the udagram folder at the root of the repository as this will become the main content of the project.
1. Open a terminal and navigate to the root of the repo
1. follow the instructions in the installation step

The project can run but is missing some information to connect to the database and storage service. These will be setup during the course of the project

### Dependencies

```
- Node v14.15.1 (LTS) or more recent. While older versions can work it is advisable to keep node to latest LTS version

- npm 6.14.8 (LTS) or more recent, Yarn can work but was not tested for this project

- AWS CLI v2, v1 can work but was not tested for this project

- A RDS database running Postgres.

- A S3 bucket for hosting uploaded pictures.

```

### Installation

Provision the necessary AWS services needed for running the application:

1. In AWS, provision a publicly available RDS database running Postgres.
2. In AWS, provision a s3 bucket for hosting the uploaded files.
3. Export the ENV variables needed or use a package like [dotnev](https://www.npmjs.com/package/dotenv)/.
```.env
POSTGRES_USERNAME=USERNAME
POSTGRES_PASSWORD=PASSWORD
POSTGRES_DB=postgres
PORT=3000
POSTGRES_PORT=5432
POSTGRES_HOST=HOST
AWS_REGION=REGION
AWS_PROFILE=DEFAULT
AWS_BUCKET=S3Bucket
URL=EB.ENV.URL
AWS_ACCESS_KEY_ID=ID
AWS_SECRET_ACCESS_KEY=SECRTb2PhXaXSOou
JWT_SECRET=SECRET
```
1. From the root of the repo, navigate udagram-api folder `cd udagram-api` to install the node_modules `npm install`. After installation is done start the api in dev mode with `npm run dev`.
2. Without closing the terminal in step 1, navigate to the udagram-frontend `cd udagram-frontend` to intall the node_modules `npm install`. After installation is done start the api in dev mode with `npm run start`.

## Testing

This project contains two different test suite: unit tests and End-To-End tests(e2e). Follow these steps to run the tests.

1. `cd udagram-frontend`
2. `npm run test`
3. `npm run e2e`

There are no Unit test on the back-end

### Unit Tests:

Unit tests are using the Jasmine Framework.

### End to End Tests:

The e2e tests are using Protractor and Jasmine.

## Built With

- [Angular](https://angular.io/) - Single Page Application Framework
- [Node](https://nodejs.org) - Javascript Runtime
- [Express](https://expressjs.com/) - Javascript API Framework
## Application
[App ./](http://alionour.udagram.s3-website-us-east-1.amazonaws.com/)
## License

[License](LICENSE.txt)
