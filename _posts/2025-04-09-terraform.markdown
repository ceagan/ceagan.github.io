---
layout: post
title: "Terraform"
date: 2024-09-17
categories: DevSecOps
---

When I started using [Terraform][terraform], I was at a loss for how to organize my code. The flexibility provided by the authors has resulted in a variety of organizational strategies in the projects I've seen. I have some of my own opinions now based on some of what I've seen and read, but I've also identified a few areas that require some special handing in Terraform as a result of my experiments.

## Organization

When I first heard people talking about writing self-documenting code, I was aghast. Comments were a critical component of clean code in my opinion at the time. I've since changed my mind about this. Languages offer features for developers to convey information through the names of Packages, Interfaces, Classes, Methods, Fields, Variables, and more. Usually, if something isn't clear in code, it is because a layer of abstraction is missing that could add context and simplify the code. Remember that code languages are written by humans for humans. If they were written for computers, then we would all still be writing code in assembly language.

To truly deliver self-documenting code, it is important for the language to allow structuring of code that enables labelling of components with meaningful, domain-specific names. Unfortunately, I have not found HCL to be very helpful here. It is possible to name modules and blocks, like data and resources. File names are not enforced, but by convention, their names are specified as `main.tf`, `variables.tf`, `outputs.tf`, and `provider.tf`.

I opted to use [Terraform Modules][terraform-modules] with domain-specific names as a primary organization method. Since modules also support named Input Variables and Outputs, these can be used to convey domain-specific information as well. It is important to keep modules small and focused, so I will also use submodules to organize larger modules into smaller pieces. Name modules after some domain aspect and do not use the name of the primary resource as the name, because this type of naming doesn't add information. For example, it is better to name a module `address-storage` instead of `rds-db`. Modules should create more than one resource, or you will end up with way too many modules.

## Branching

[Terraform][terraform] offers only a few branching mechanisms, and some users may consider using these mechanisms for branching to be a misuse of the language. Using `count` to create as few as zero of a resource is one such trick. I have found this to be valuable when creating shared modules, but I recommend avoiding branches in application infrastructure definitions.

### Dynamic Template File Selection

There are situations in a shared module where it is helpful to extract large chunks of information to a template file and use different versions of the template file under different conditions. For this, I have used variables to define the path to the template file so that a different template file is loaded based on the value of the variable. This can be a powerful concept 

## Validation

Input validation is supported only at the variable level, and I encourage extensive use of validation to ensure inputs are correct. In scenarios where resources are loaded by name, it is often helpful to use validation as a method for ensuring the infrastructure is configured as expected.

Consider an input variable which refers to an RDS database. Loading the associated resource enables us to load additional details like the list of associated security groups. From there, we may want to filter the groups down to one that matches the name we are looking for and then add a rule to it.

If we do this using the `count` trick and the expected security group is not found, the plan will be successful, but the rule will not have been added to the security group.

Instead, we can add a submodule and pass the expected security group in as a variable. The variable can have validation that allows us to check that the security group is not empty. In this case, [Terraform][terraform] produces an error message and saves us hours of confusion and debugging effort.

## Conclusion

I have found [Terraform][terraform] to execute faster on AWS than native Cloud Formation, and since Terraform has additional providers, like Docker, Terraform can combine more of your infrastructure into a single place than Cloud Formation alone. It is highly performant and capable of synchronous resource updates.

Terraform does lack organizational expressiveness by limiting where developers are able to use custom naming and organization. Procedural options like [AWS CDK][aws-cdk] and [Pulumi][pulumi] probably make it easier to be more expressive, incorporate branching and enforce values through validation. The expense of these languages is probably that they are slower to execute and deploy changes to infrastructure, but I have not personally tested this.

On my next large project, I think I'd like to try [Pulumi][pulumi].

[aws-cdk]: https://aws.amazon.com/cdk/
[pulumi]: https://www.pulumi.com
[terraform]: https://www.terraform.io
[terraform-modules]: https://developer.hashicorp.com/terraform/language/modules
