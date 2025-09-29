# Garage Assist Infrastructure

The templates in the `/garage-assist` directory are required to provision the resources necessary to run the 
Garage Assist API, Garage Assist UI and related applications.

## Directions

### Provision Using AWS Management Console

1. Navigate to the  [AWS CloudFormation Console](https://us-east-2.console.aws.amazon.com/cloudformation) for the target account
2. Create a stack for each group in the `garage-assist-api` project folder in this order: buckets, topics, policies, roles, repositories