# AWS CloudFormation

an IaC (Infa as Code) service that allows you to manage your whole infrastructure using code.

Provision AWS resources in an automated, orderly and predictable fashion

A CloudFormation template can be written in YAML or JSON

We can create stack, the collection of resources defined in your CloudFormation template

Changes are carried out using changesets, a summary of proposed changes before committing them.

We can import templates from S3 buckets

## CloudFormation Template anatomy

List of properties required in the yml file

`AWSTemplateFormatVersion: 2010-09-09`

- `Parameters`
    - set key value pairs to be used later
    - e.g.
        - `EC2Instancetype`
        - `Description: "Choose an instance type"`
        - `Type: String`
        - `AllowedValues:`
            - `t2.nano`
            - `t2.micro`
        - `Default: t2.micro`
    - used like `!Ref EC2Instancetype`
- `Mappings`
    - The optional section matches a key to a set of named values
    - For example, if you want to set values based on a region, you can create a mapping that uses the region name as a key and contains the values you want to specify for each specific region. 
    - You use the `Fn::FindInMap` intrinsic function to retrieve values in a map.
    - e.g.
        ```yml
           Mappings:
               Regions:
                   us-east-1:
                       MyAMI: ami_id_1
                   us_east-2:
                       MyAMI: ami_id_2
        ```
    - Used like `ImageId: !FindInMap [Regions, !Ref "AWS::Region", MyAMI]`
- `Conditions`
    - The optional `Conditions` section contains statements that define the circumstances under which entities are created or configured
    - e.g.
        ```yml
        Conditions:
            CreateEIP: 
                !Equals [!Ref EIP, "yes"]
        ```
- `Resources` - (Required)\
    - e.g.
    ```yml
    Resources:
        MyEC2Instance:
            Type: AWS::EC2::Instance
            Properties:
                ImageId: "String"
                InstanceType: "String"
    ```
- `Outputs`
    - declares output values that you can import into other stacks
    - e.g.
    ```yml
    Outputs:
        MyEc2Instance:
            Description: 'Instance Id'
            Value: !Ref MyEc2Instance
            Export:
                Name: "MyEC2Instance"
    ```
- `Transform`
    - used for AWS SAM Templates, see next section


## AWS SAM

AWS Serverless Application Model is a AWS CloudFormation macro that transforms SAM templates into CloudFormation templates

A SAM template is an extension of a CloudFormation template that provides a shorthand syntax for defining serverless resources

To use the SAM transform, add `AWS::Serverless-2016-10-31` to the `Transform` section of your CloudFormation template

Before deployment, a SAM tempplate is transformed and expanded to a native CloudFormation template

Benefits of using the SAM transform include:

* Built-in best practices and sane defaults.
* Local testing and debugging with the AWS SAM CLI.
* Extension of the CloudFormation template syntax.

### Useful SAM CLI Commands

`sam init` - initializes and provides option to get a quickstart SAM template
`sam package` - packages artifacts and writes output tempplate file to S3
`sam deploy` -  deploys the template
`sam local invoke` - invokes the local SAM repo with arguments