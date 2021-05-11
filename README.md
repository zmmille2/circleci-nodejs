# Circle CI Kata

Circle CI is a continuos integration and delivery tools that be used on any cloud.

## Challenges

## 1 - Setup

Prerequisites

- Forked the repo (njm3754/kata-bootstrap)
- Cloned the repo on local machine
- Run `npm ci` & `npm test` locally to verify that everything is running as expected

Steps

- Login into [CircleCI](https://circleci.com/) using Github credentials.
- Select the organization under which your repo exists.
- Click on `Set Up Project` to select the project you want to run with CircleCI.
- Select `Use Existing Config` option so that CircleCi uses the config file from the repo (.circleci/[config.yml](.circleci/config.yml)).
- Finally click the `Start Building` button to trigger the pipeline.

## 2 - Jobs & Workflows

We have a NodeJs-Jest project setup for deployment.

The goal of this sub task is to run `npm install`, `npm build` and `npm test` commands within a job and verify the results in the Circle CI portal.

The run should look something like this

![alt text](images/CC1.png)

As you can notice the there is keyword `workflow` and the reason this generic name appears is because we haven`t setup workflows yet.
In simple terms, workflows allow you to set rules for defining a collection of jobs and their run order.

So, next step will be to add the following workflow block on the top of the code and the result should be like the image below.

    workflows:
      version: 2
      build_and_test: #name of the workflow
      jobs:
        - (Add name of the job here)

![alt text](images/CC2.png)

## 3 - Sequential Job Execution

Next we will do a dummy deployment of our code. Please copy paste the sample deployment job under the `build-test` job.

    deploy:
        working_directory: ~/project
        docker:
        - image: circleci/node:8
        steps:
        - checkout
        - run:
            name: Deployment
            command: echo "SUCCESS - App deployment completed"

By default the two jobs will run in parallel which means that the `build-test` will happen at the same time the `deploy` job is running.
Therefore, we need to make sure that the deployment job waits for the successful completion of the `build-test` job and doesn't initiate if the `build-test` fails.

## 4 - Manual Approval Deployment

In a real life scenario it would be ideal if you could verify if all the builds and tests are passing before Circle CI does deployment.
So, that is why we need to add a manual approval button that waits for the user to click `Approve` before kicking in deployment.

Follow this [documentation](https://circleci.com/docs/2.0/workflows/?utm_medium=SEM&utm_source=gnb&utm_campaign=SEM-gb-DSA-Eng-uscan&utm_content=&utm_term=dynamicSearch-&gclid=Cj0KCQjws-OEBhCkARIsAPhOkIZGYE3L0OMR6SumDhQCQ9xW_wxSnFd6uW-zuJ4ASC8NComBDWhKQdkaAsVrEALw_wcB) to create manual approval step to hold the deployment.
