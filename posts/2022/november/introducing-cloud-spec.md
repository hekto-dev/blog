---
title: Introducing Cloud Spec
date: 2022-11-29T22:00:00
author: Sebastian Korfmann
tags:
  - cloud spec
  - aws
  - aws cdk
---

Testing Infrastructure as Code (IaC) applications can still be challenging. Besides from the question which test paradigms to follow, there's some complexity in setting up an end2end test lifecycle. [Cloud Spec](https://github.com/hekto-co/cloud-spec) provides a thin abstraction on top of [Jest](https://jestjs.io/) to stream line integration test workflows.

<!--more-->

[Cloud Spec](https://github.com/hekto-co/cloud-spec) is focusing on the Typescript ecosystem for now. Support for Python or other languages might be something to consider depending on demand. Currently, there's an adapter for the [AWS CDK](https://github.com/aws/aws-cdk), with adapters for [CDK for Terraform](https://cdk.tf) and [Pulumi](https://pulumi.com) to follow.

Here's a simple exmple for an integration test. It will create a test aws cdk application behind the scenes, do a deployment and pass the outputs to the test. This expects valid credentials in the ENV or a `AWS_PROFILE` env being set.

```js
import { cdkSpec as cloud, createTestApp, ForceEphemeralResources } from '@hekto/cloud-spec-aws-cdk'
import { CfnOutput, Aspects } from 'aws-cdk-lib'
import { Bucket } from 'aws-cdk-lib/aws-s3'

// create and synthesize Test Stack
const testApp = createTestApp({
  creator: (stack) => {
    const component = new Bucket(stack, 'MyBucket')

    new CfnOutput(stack, 'BucketName', {
      value: component.bucketName,
    })

    Aspects.of(stack).add(new ForceEphemeralResources())
  },
})

describe('Repository', () => {
  // actually deploy the synthesized application
  cloud.setup({
    testApp,
  })

  cloud.test(
    'should be defined',
    async (stackOutputs) => {
      const sfnArn = stackOutputs[testApp.stackName].BucketName
      expect(sfnArn).toEqual(expect.stringMatching(/mybucket/))
    },
    300_000,
  )
})
```
## Mechanics

To enable faster iterations, the test stack has a static name with the following schema: `TestStack-<Username>-<hash-of-test-file-path>`. With that, each developer has their own test stack to iterate. In the context of Github, the username is replaced by the Gitref, e.g. `TestStack-<GitRef>-<hash-of-test-file-path>`.

While it's possible to cleanup the test stacks as part of the test lifecycle, I'd do this out of the normal test iteration. One idea could be, to trigger a deletion of the related test stacks when a Github Pull Request is closed. However, that's not automated at the moment - but contributions are more than welcome.

## Conclusion

[Cloud Spec](https://github.com/hekto-co/cloud-spec) is still in early days but already quite useful for integration testing Typescript based CDK apps. Please give it a try, feedback would be very appreciated. Feel free to reach out via [LinkedIn](https://www.linkedin.com/in/skorfmann/) or [skorfmann@mastodon.cloud](https://mastodon.cloud/@skorfmann).