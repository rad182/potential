# Platform Practice Overview

* [Principles of platform engineering](Principles.md)
* [FAQ](FAQ.md)
* [Domain models](DomainModels.md)

## Team Information

### Team Goals

* Powerful, intelligent data and APIs
* Robust internal tools and infrastructure
* Platform for future growth

### Who is on the Platform Practice?

Current members of the Platform Practice are: [Joey](https://github.com/joeyaghion), [Barry](https://github.com/bhoggard), [Matt](https://github.com/mzikherman), [Will](https://github.com/wrgoldstein), [Sarah](https://github.com/sweir27), [Ashkan](https://github.com/ashkan18) and [Anil](https://github.com/cavvia).

### Process

* Our process is centered on the kanban-style [Platform engineering Trello board](https://trello.com/b/2lTTggr8/platform-engineering)
* Every other Monday morning, we meet to plan the coming 2-week sprint _[Experimental]_
  * In advance, we brainstorm suitable goals for the sprint. These become _Epics_.
  * After reviewing and refreshing the board, we discuss and prioritize the stories each epic demands. These well-understood work items enter the _Next_ column.
* "Daily" stand-up is at 11:15 AM (Eastern) on Mondays through Thursdays. We meet in person (at HQ) and video-conference at [https://appear.in/platform](https://appear.in/platform).
    * Taking turns: what are you working on or need help with?
    * Switching to the [board](https://trello.com/b/2lTTggr8/platform-engineering), we review and update the inner columns:
      * _Next_ - well-understood work items, approximately prioritized and sized to reflect deployable changes with [ideally] some user impact
      * _Now_ - in-progress work
      * _Done_ - completed work ready to be merged or deployed
      * _Monitoring_ - deployed
      * _Delivered_ - confirmed to work as expected in production, including any necessary communication or training
    * Explain any new additions or requests
* We chat in [#platform-humans](https://artsy.slack.com/messages/platform-humans/)
* Automated notifications appear in [#platform-machines](https://artsy.slack.com/messages/platform-machines/)
* Significant alerts go to [#platform-alerts](https://artsy.slack.com/messages/platform-alerts/)
* Fridays are **Future Fridays**, where we take a break from in progress work to focus on longer-term improvements. (See [blog post](http://artsy.github.io/blog/2015/12/22/future-fridays/).) _[Experimental]_

### Our Apps

* [Gravity](https://github.com/artsy/gravity): main API
* [Heat](https://github.com/artsy/heat): offline/batch processing
* [Gemini](https://github.com/artsy/gemini): image-processing
* [Resistance](https://github.com/artsy/resistance): experiments and analysis
* [Radiation](https://github.com/artsy/radiation): inquiry email tracking
* [Reflection](https://github.com/artsy/reflection): page pre-rendering for SEO
* [Lattice](https://github.com/artsy/lattice): custom report generator
* [Helix](https://github.com/artsy/helix): dedicated app for genoming artworks/artists
* See the [engineering projects map](https://trello.com/b/VLlTIM7l/artsy-engineering-projects-map) for a comprehensive list of Platform Practice apps.

Most projects include some metadata in their README describing their deployment, continuous integration, point-people, etc. (e.g., [Radiation's](https://github.com/artsy/radiation#meta)). We tend to use Jenkins for crons, but most projects use Travis, Semaphore, or Codeship (Gravity) for CI. Many simple projects are deployed to [Heroku](https://dashboard.heroku.com/. A few larger-scale or more custom projects are deployed to [AWS Opsworks](https://aws.amazon.com/opsworks/).

### Our Dashboards

* Team KPIs: http://artsy-dashing.herokuapp.com/platform
* Real-time overall health stats: http://artsy-tasseo.herokuapp.com/velocity
* Bidding: http://artsy-tasseo.herokuapp.com/bidding
* Other time series data (graphite): http://velocity.artsy.net
* Lots more is available in Redshift and [Looker](https://artsy.looker.com/)

See [Monitoring](Monitoring.md) for more information.

## Onboarding

### Our Technologies

#### Rails

Most of the Platform Practice's apps are built on Rails. If you are unfamiliar with Rails, take some time to go through a tutorial, such as [Michael Hartl's Rails Tutorial](https://www.railstutorial.org/book).

#### Backbone

[Torque](https://github.com/artsy/torque), the main admin app, uses Backbone, so if you find yourself working on that app it might be helpful to browse the [Backbone docs](http://backbonejs.org/) for more info.

### API

Our API is implemented in the [Gravity](https://github.com/artsy/gravity) project and uses [Grape](https://github.com/intridea/grape).

### Installing Gravity

[Gravity](https://github.com/artsy/gravity) was Artsy’s original website. It was a monolithic Rails app that included our API and front-end code. At some point, we broke out client-side code into its own application, [Force](https://github.com/artsy/force).

Follow the [Gravity docs](https://github.com/artsy/gravity/blob/master/doc/GettingStarted.md) to get started.
