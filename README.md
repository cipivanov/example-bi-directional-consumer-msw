# Example Mock Service Worker Consumer

[![Build](https://github.com/pactflow/example-consumer-msw/actions/workflows/build.yml/badge.svg)](https://github.com/pactflow/example-consumer-msw/actions/workflows/build.yml)

[![Can I deploy Status](https://testdemo.pactflow.io/pacticipants/pactflow-example-consumer-msw/branches/master/latest-version/can-i-deploy/to-environment/production/badge)](https://testdemo.pactflow.io/pacticipants/pactflow-example-consumer-msw/branches/master/latest-version/can-i-deploy/to-environment/production/badge)


This is an example of a React consumer using mock-service-worker to demonstrate the bi-directional contract testing capability of [Pactflow](https://pactflow.io).

It implements a "Product" website. You can see the [Provider](https://github.com/pactflow/example-pactflow-example-provider-dredd) counterpart (see below for other compatible example providers).

It is using a public tenant on Pactflow, which you can access [here](https://test.pactflow.io) using the credentials `dXfltyFMgNOFZAxr8io9wJ37iUpY42M`/`O5AIZWxelWbLvqMd8PkAVycBJh2Psyg1`.

Unlike with Pact, the provider does not need to replay any tests to ensure it remains compatible with its consumers. This is achieved with a call to "can-i-deploy", which performs a cross-contract validation, ensuring any consumer consumes a valid subset of the OAS for the provider.

![Consumer Test](docs/consumer-scope.png "Consumer Test")

When you run the CI pipeline (see below for doing this), the pipeline should perform the following activities (simplified):

![Consumer Pipeline](docs/consumer-pipeline.png "Consumer Pipeline")

### Pre-requisites

**Software**:

- Tools listed at: https://docs.pactflow.io/docs/workshops/ci-cd/set-up-ci/prerequisites/
- A pactflow.io account with an valid [API token](https://docs.pactflow.io/docs/getting-started/#configuring-your-api-token)

#### Environment variables

To be able to run some of the commands locally, you will need to export the following environment variables into your shell:

- `PACT_BROKER_TOKEN`: a valid [API token](https://docs.pactflow.io/docs/getting-started/#configuring-your-api-token) for Pactflow
- `PACT_BROKER_BASE_URL`: a fully qualified domain name with protocol to your pact broker e.g. https://testdemo.pactflow.io
- `PACT_PROVIDER=pactflow-example-provider-dredd`: this changes the default provider to the Dredd based provider (https://github.com/pactflow/example-provider-dredd)
- `PACT_PROVIDER=pactflow-example-provider-postman`: ... Postman (https://github.com/pactflow/example-provider-postman)
- `PACT_PROVIDER=pactflow-example-provider-restassured`: ... Rest Assured (https://github.com/pactflow/example-provider-restassured)

### Usage

#### Use case with Mock Service Worker

Reuse your API mocks generated with MSW and generate pacts from specified interactions using [pact-msw-adapter](https://www.npmjs.com/package/@pactflow/pact-msw-adapter)

- `make clean` - ensure previous pacts are cleared
- `make test` - run msw test locally
- `make fake_ci` - emulate the CI process locally

The test file can be seen here `./src/api.msw.spec.js`

## Key points

- [`src/mocks/handlers.js`](src/mocks/handlers.js) describes msw request handlers to use.
- [`src/mocks/server.js`](src/mocks/server.js) sets up the "server" to use the same mocking logic in Node.
- [`src/setupTests.js`](src/setupTests.js) enables mocking for unit tests via and allows us to record which msw matched interactions we wish to serialise into a pact.
