# aws-cf-templates

Your source for free [AWS CloudFormation](https://aws.amazon.com/cloudformation/) templates. Bootstrap high-quality AWS infrastructure within minutes.

## Project goals and philosophy

* Accelerate the development of complex AWS infrastructure: Provide a set of common building blocks as templates that are easy to combine.
* Top-notch template quality: peer-reviewed by an expert (certified AWS solutions architect Professional) and verified with automated tests.
* Production-ready. If no other limitations are documented, templates are:
    * Highly available: no single point of failure
    * Scalable: increase or decrease the capacity based on utilization
    * Frictionless deployment: deliver new versions of your application automatically without downtime
    * Secure: using the latest operating systems and software components, follow the least privilege principle in all areas, backups enabled
    * Operator-friendly: provide tools like logging, monitoring and alerting to recognize and debug problems
* Premium Support available: Get help in case of small and big emergencies or submit a feature request.

## How it works

CloudFormation turns a template (JSON or YAML) into a stack like the following figure shows.

![CloudFormation](./img/cloudformation.png)

You can apply updates to an existing stack with an updated template. CloudFormation will figure out what needs to be changed.

> Never make manual changes to infrastructure managed by CloudFormation!

aws-cf-templates provides a collection of templates designed as common building blocks that work together. Imagine you want to set up a Jenkins automation server. You could do this in a single template. But you will soon discover that many parts of the templates are not related to Jenkins. That's why we split our templates into building blocks that you can combine in creative ways. Back to the Jenkins example. The following figure shows how four templates are used to spin up a production-ready Jenkins environment.

![Example: Modules](./img/example-modules.png)

If you create a stack, you sometimes have to supply parameters that start with `Parent`. That's the mechanism to pass dependent stacks into a stack.

Let's look at the first dependency.

#### VPC dependency (required)

Many templates depend on a VPC stack. The VPC is a required dependency. 

![VPC dependency](./img/example-vpc.png)

#### Alert dependency (optional)

If you want to receive alerts when things go wrong, you can optionally supply an alert stack. 

![Alert dependency](./img/example-alert.png)

I highly recommend using an alert stack. Otherwise, you will not know when things go wrong (and they will!).

#### SSH bastion host dependency (optional)

If you want to add some extra security, you can use an SSH bastion host.

![SSH bastion host dependency](./img/example-ssh-bastion-host.png)

The bastion host has a optional dependency on the alert stack. So if you want to receive alerts if your bastion hosts is in trouble, supply an alert stack.

#### Jenkins

Finally, you can create the Jenkins stack.

![Jenkins](/img/example-jenkins.png)

The cool thing is that you can re-use the dependencies. E.g., you can use the same SSH bastion host for Jenkins and WordPress.

## Related projects

### widdix CLI
[widdix, a CLI tool to manage Free Templates for AWS CloudFormation](cli.md).

### cfn-modules
[Easy-going CloudFormation](https://github.com/cfn-modules/docs): Modular, production ready, open source.

## Infrastructure Templates
Choose from our template collection:

* [Elastic Compute Cloud (EC2)](ec2.md)
* [EC2 Container Service (ECS)](ecs.md)
* [Jenkins ](jenkins.md)
* [Operations](operations.md)
* [Security](security.md)
* [State / Data](state.md)
* [Static Website](static-website.md)
* [Virtual Private Cloud (VPC)](vpc.md)
* [WordPress](wordpress.md)

## License
All templates are published under Apache License Version 2.0.

## Help needed?
You will probably find an answer to your question on [Stack Overflow](https://stackoverflow.com/questions/tagged/amazon-cloudformation). If not, use the tag `amazon-cloudformation` to post your question, and the chances are high that we or someone from the community will point you in the right direction. We are not able to answer your questions via email or the project's issue tracker.

## Premium Support
Are you in need of a feature or does a bug cause you sleepless nights? Please let us know by using the project's [issue tracker](https://github.com/widdix/aws-cf-templates/issues). We work on bug fixes and new features as time permits. Are you in need of an urgent bug fix or important feature request? [Contact us](mailto:hello@widdix.net) to sponsor a feature or bug fix.

## Training and Consulting
Do you want to accelerate your start with AWS CloudFormation and our templates? We do offer remote and on-site training for you and your team. Are you looking for guidance on how to use or adapt our templates to your use case? We offer consulting services as well. [Contact us](mailto:hello@widdix.net), and we’ll accelerate your project.
