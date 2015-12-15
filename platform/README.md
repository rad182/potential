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

### Logistics

* Daily stand-up is at 11:15 AM (Eastern) on Mondays through Thursdays. We meet in person (at HQ) or at [https://appear.in/platform](https://appear.in/platform).
    * What are you working on or need help with?
    * Review the [Trello board](https://trello.com/b/2lTTggr8/platform-engineering)
    * Explain any new additions or requests
* Work is described in a kanban-style [Trello board](https://trello.com/b/2lTTggr8/platform-engineering)
    * Cards describe "stories": deployable chunks, focusing on user impact
    * Work that’s well-understood starts in the "Next" column, from which it’s pulled into "Now," moved to "Done" when your work is done and you are available to work on something else, deployed to "Shipped/Monitoring," then finally "Delivered" when it’s confirmed to work as expected in production.
    * We sometimes track less well-understood or larger projects in the "Ideas" column
* Chat in [#platform-humans](https://artsy.slack.com/messages/platform-humans/)
* GitHub notifications, etc. in [#platform-machines](https://artsy.slack.com/messages/platform-machines/)
* We hold **Future Fridays**, where we take a break from in progress work to focus on longer-term improvements.

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

### Our Dashboards

* Team KPIs: http://artsy-dashing.herokuapp.com/platform
* Real-time overall health stats: http://artsy-tasseo.herokuapp.com/velocity
* Bidding: http://artsy-tasseo.herokuapp.com/bidding
* Other time series data (graphite): http://velocity.artsy.net
* Snowplow (realtime events): http://artsy-tasseo.herokuapp.com/snowplow

See [Monitoring](Monitoring.md) for more information.

## Onboarding

### Our Technologies

#### Rails

Most of the Platform Practice's apps are built on Rails. If you are unfamiliar with Rails, take some time to go through a tutorial, such as [Michael Hartl's Rails Tutorial](https://www.railstutorial.org/book).

#### Backbone

[Torque](https://github.com/artsy/torque), the main admin app, uses Backbone, so if you find yourself working on that app it might be helpful to browse the [Backbone docs](http://backbonejs.org/) for more info.

### API

Our API is currently made using [Grape](https://github.com/intridea/grape), and housed in [Gravity](https://github.com/artsy/gravity).

### Installing Gravity

[Gravity](https://github.com/artsy/gravity) was Artsy’s original website. It was a monolithic Rails app that included our API and front-end code. At some point, we broke out client-side code into its own application, [Force](https://github.com/artsy/force).

Follow the [Gravity docs](https://github.com/artsy/gravity/blob/master/doc/GettingStarted.md) to get started.
