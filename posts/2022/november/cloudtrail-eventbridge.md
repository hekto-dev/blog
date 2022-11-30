---
title: "CloudTrail & EventBridge"
date: 2022-11-30T22:00:00
author: Sebastian Korfmann
tags:
  - aws
  - aws cdk
  - "#til"
---

To receive CloudTrail Events in EventBridge, one has to create a dedicated Trail.

<!--more-->

It's mentioned briefly on the page which lists the [supported services](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-service-event.html) for EventBridge.

> To record events with a `detail-type` value of `AWS API Call via CloudTrail`, a CloudTrail trail with logging enabled is required.

Interesting sidenote, a lot of these services support `best effort` delivery only - including CloudTrail.

> Each AWS service that generates events sends them to EventBridge as either `best effort` or `guaranteed` delivery. Best effort delivery means that the service attempts to send all events to EventBridge, but in some rare cases an event might not be delivered. Guaranteed delivery means that all events from the service are successfully delivered to EventBridge.

### Example

The following AWS CDK line is all which is necessary to make this work.

```js
import { Trail } from 'aws-cdk-lib/aws-cloudtrail'
//...
new Trail(this, 'CloudTrail');
```
