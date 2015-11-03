# Monitoring

---

## High-level team KPIs

https://artsy-dashing.herokuapp.com/platform

* Uses Shopify's [dashing](https://github.com/Shopify/dashing)
* Bidding: [https://artsy-dashing.herokuapp.com/bidding](https://artsy-dashing.herokuapp.com/bidding
)

Repo: [https://github.com/artsy/artsy-dashing](https://github.com/artsy/artsy-dashing)

---

## General time-series data

http://velocity.artsy.net

* Counts, timers, etc.
* Uses [Graphite](http://graphite.wikidot.com/) and [statsd](https://github.com/etsy/statsd)
* See:
  * Graphite dashboard
  * JSON
  * Saved charts

Repo: [https://github.com/artsy/velocity](https://github.com/artsy/velocity)

---

## Real-time dashboards

https://artsy-tasseo.herokuapp.com

* Uses [tasseo](https://github.com/obfuscurity/tasseo) library
* Based on graphite/statsd data
* Overall health: https://artsy-tasseo.herokuapp.com/velocity
* Bidding: https://artsy-tasseo.herokuapp.com/bidding
* Gemini (image-processing): https://artsy-tasseo.herokuapp.com/bidding

Repo: [https://github.com/artsy/artsy-tasseo](https://github.com/artsy/artsy-tasseo)

---

## New Relic (request- or host-level metrics)

* [gravity-production](https://rpm.newrelic.com/accounts/334984/applications/1919032) (rails app servers)
* [gravity-production (workers)](https://rpm.newrelic.com/accounts/334984/applications/10113497)
* (Warning: sampled)
* [force-production](https://addons-sso.heroku.com/apps/force-production/addons/315ec54e-9bd5-4c2a-a1cd-96f29696d660)

---

## [AWS Opsworks](https://console.aws.amazon.com/opsworks/home?region=us-east-1) Monitoring (CPU, memory, etc.)

E.g., [gravity-production stack](https://console.aws.amazon.com/opsworks/home?region=us-east-1#/stack/64e2d852-99d2-4235-bd1f-46e7e6fa4e94/monitoring)

---

## AWS Elastic Load Balancer Metrics (health, latency)

[AWS Elastic Load Balancer](https://console.aws.amazon.com/ec2/v2/home?region=us-east-1#LoadBalancers:)

---

## Heroku monitoring (errors, memory, throughput)

E.g., [force-production](https://dashboard.heroku.com/apps/force-production/metrics/web)

---

## Pingdom (uptime)

* Dashboard: [https://my.pingdom.com/dashboard/checks](https://my.pingdom.com/dashboard/checks)
* Status page: [http://status.artsy.net](http://status.artsy.net)

---

## Logs

[Papertrail](https://papertrailapp.com/) for current/recent API logs

* Can filter (e.g., `worker1`)
* 3 days of logs retained
* Older logs archived to [artsy-logs S3 bucket](https://console.aws.amazon.com/s3/home?region=us-east-1&bucket=artsy-logs&prefix=)

Tailing via [momentum](https://github.com/artsy/momentum):

`rake ow:logs[production,worker1,/srv/www/gravity/shared/log/*.log]`

---

## Other AWS logs and metrics:

### AWS Cloudwatch

[https://console.aws.amazon.com/cloudwatch/home?region=us-east-1](https://console.aws.amazon.com/cloudwatch/home?region=us-east-1)

### AWS Cloudtrail

[https://console.aws.amazon.com/cloudtrail/home?region=us-east-1#/events](https://console.aws.amazon.com/cloudtrail/home?region=us-east-1#/events)

---

## Database metrics

[Compose.io](https://app.compose.io/artsy/deployments)

[MongoDB Cloud Manager](https://cloud.mongodb.com)

---

## Honorable mention

Data changes: https://github.com/artsy/gravity/blob/master/doc/TrackingDataChanges.md

## Deprecated

* Snowplow (real-time events pipeline): http://artsy-tasseo.herokuapp.com/snowplow
