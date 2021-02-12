---
title: "Structure"
permalink: /aws/getting-started-with-aws-cdk/structure
toc: true
---

CDK comprises of the following components.

### Constructs
Constructs represent aws resources, sometimes a contstruct can define multiple related aws resources.
we have different L1 Constructs and L2 Constructs.

each construct has scope, ID and properties. CDK create a tree of costructs, each construct might have other constructs within itself. CDK maintains this tree with the help of scope which is passed through the first argument of construct's constructor function.

the ID of a construct is an identifier of resource within the Stack's scope, so it's duplication among different stacks has no problem, but be aware that this ID is used to form Cloudformation Logical, so if you change it after the deployment it causes the cloudformation to create a new resource in the next run.

there are two types of Constructs, L1 and L2.
#### L1 Constructs
these constructs are exact mapping of Cloudformation types, their names start with 'Cfn' for example 'CfnBucket'

#### L2 Constructs
these are higher level constructs which encapsulate many details of aws resources with default values, these constructs are the key components that differentiates CDK from Cloudformation.


### Stacks
Stacks are the construct implementations of Cloudformation's stacks. a stack can have many constructs or even another nested stacks.

### Apps
one or many stacks forms an app, app is not an actual deployable entity, its more of a logical entity that defines the scope, instantiates stacks and creates the cloudformation templates. app is the root construct of CDK's construct tree, so it does not need to be instantiated.


### Environments
each stack is bound to an environment which defines the region and aws account to be deployed in. if you do not specify it the environment is determined by by the profile of your aws cli.

