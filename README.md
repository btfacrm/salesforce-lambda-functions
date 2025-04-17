# Salesforce Lambda Functions

## Overview

These AWS Lambda functions were published by Salesforce into the AWS Serverless
Application Repository under the name AmazonConnectSalesforceLambda. The purpose
of this repo is to store the template that deploys the functions and related
resources, as well as the changes that have been made to the Python code to
correctly authenticate with Salesforce.

## Lambda Functions

Two copies of the Lambda function code are included in the repo, one with the
original authorization logic that was published by Salesforce, and another with
updated logic that has been modified to mitigate an unresolved issue where the
functions cannot reauthenticate with Salesforce once their cached credentials in
Secrets Manager have expired.

The only differences between the files in these two directories can be found by
comparing the [salesforce.py](functions/original_auth/salesforce.py) file in the
`original_auth` directory with the
[salesforce.py](functions/updated_auth/salesforce.py) file in the `updated_auth`
directory.

## CloudFormation Templates

There are 3 templates included in the repo:

- [template.yaml](templates/template.yaml): this template was included in the
  original zip file that was published by Salesforce. It is the source template
  which is packaged and transformed using the [`aws cloudformation package` CLI
  command][1]

- [transformed.yaml](templates/transformed.yaml): this template is the result of
  the `aws cloudformation package` CLI command. It is what's currently deployed
  into the serverlessrepo-AmazonConnectSalesforceLambda CloudFormation stack,
  which creates all Dev Lambda functions, roles, state machines, and related
  resources.

- [prod-template.yaml](templates/prod-template.yaml): this template is a
  modified version of `transformed.yaml`, which references the 3 zip files used
  by the Lambda layer, the functions with updated authentication logic, and the
  functions with the original authentication logic.

## Zip Files

The zip files that are referenced in
[prod-template.yaml](templates/prod-template.yaml) are included here, and are
also uploaded to the `273332161721-salesforce-lambda-functions-source` S3 bucket
that is used in the template.

[1]:
  https://docs.aws.amazon.com/cli/latest/reference/cloudformation/package.html
