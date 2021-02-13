---
title: "CDK Quick start guide"
permalink: /aws/getting-started-with-aws-cdk/quick-start-guide
classes: wide
tags:
  - "aws"
  - "cdk"
---


AWS resources can be created and managed in many ways; console, cli, API, and cloudformation.
Cloudformation is a great tool that defines resources in expressive and easy way in JSON or YAML format. but cloudformation has a few drawbacks.
1. As the number of resources getting bigger, the cloudformation script gets longer and more challenging.
2. YAML and JSON are not much flexible and dynamic.
3. Extending CLoudformation templates is hard.

AWS CDK is a new tool that solves above issues by defining cloud resources in a programming language.
You might think it's like working with AWS API with one of the AWS SDKs, not it's not.

### so how does it work then?

When you define a resource in CDK, CDK transpiles it in Cloudformation template.
your CDK script does not directly connect to AWS API, it only helps you to generate cloudformation templates. then you use these cloudformation templates to actually create resources.

For example this CDK code snippet

``` typescript
const sg = new ec2.SecurityGroup(this, 'sg',{
    vpc: vpc,
    allowAllOutbound: true,
});
const allowed_ports = [80, 443];
for(const port of allowed_ports){
    sg.addIngressRule(
    ec2.Peer.anyIpv4(),
    ec2.Port.tcp(Number(port))
    );
}
```

generates this cloudformation template

``` json
"sg29196201": {
    "Type": "AWS::EC2::SecurityGroup",
    "Properties": {
    "GroupDescription": "example-stack/sg",
    "SecurityGroupEgress": [
        {
        "CidrIp": "0.0.0.0/0",
        "Description": "Allow all outbound traffic",
        "IpProtocol": "-1"
        }
    ],
    "SecurityGroupIngress": [
        {
        "CidrIp": "0.0.0.0/0",
        "Description": "Allow from anyone on port 80",
        "FromPort": 80,
        "IpProtocol": "tcp",
        "ToPort": 80
        },
        {
        "CidrIp": "0.0.0.0/0",
        "Description": "Allow from anyone on port 443",
        "FromPort": 443,
        "IpProtocol": "tcp",
        "ToPort": 443
        }
    ],
    "VpcId": {
        "Ref": "vpcA2121C38"
    }
    },
    "Metadata": {
    "aws:cdk:path": "example-stack/sg/Resource"
    }
},
```

AWS CDK has the following advantages:

1. you can use the full power of programming languages (conditions, loops, classes ...).
2. programming languages data types helps you to more easily write your infrastructure, code completion saves you a lot time.
2. less code is more managable and easier to maintain.
3. you can use modules and inheritance to share and extend your code more effectively with your colleagues.
4. as you define resources and relate them together in your code, CDK adjust these resources to meet your requirements, for example it automatcally creates roles, add the required rules to security groups and etc. you just need to review the resulting cloudformation template.
4. besides the mapping of aws cloudformation types to CDK constructs, AWS has written a lot of patterns which you can import them to your projects. these patterns use the best practices and create the whole resources for you, you just need to set the important parameters to adjust it to your needs.
