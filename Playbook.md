# Engineering Playbook

## Goals of this document

This playbook was inspired by [Thoughtbot's playbook](https://thoughtbot.com/playbook), as well as internal discussions of ways for Artsy's engineering team to share a common vision and hard-won lessons internally. It aims to make good decisions easy, leaving plenty of space for experiments and even radical departures, but improving consistency where helpful. Rather than a set of policies for everyone to conform to, it's here to:
* help refine our own thinking
* create a forum for future discussion and iteration
* establish shared vocabulary
* offer a framework for when it's desired

This is a living document--a perpetual work in progress as we acquire new experiences, struggle through post-mortems, and experiment with new approaches. _You_ are the co-author, so when you're convinced a new tool or practice should be incorporated, submit a PR!


## Working together

### Project Management

Teams at Artsy use a variety of project management processes and tools including:
* [Github issues](https://guides.github.com/features/issues/)
* [Zenhub](https://www.zenhub.com/)
* [Trello](https://trello.com/)
* [Geekbot](https://geekbot.io/)
* (Others?)

Teams tend to work in short (1-2 week) _sprints_, with a planning meeting at the start and sometimes a review or retrospective at the end.

### Workflow

### Continuous improvement


## High-level architecture and technology choices

These are some of the most important and carefully considered decisions we make as engineers.

### Monoliths vs. microservices

### Extracting services

### Choosing an architecture

### Communication patterns

### Choosing a stack

### Platform/Host

### Naming and locating projects


## Application-level choices

### Authentication and authorization

### Analytics

### Metrics

### Testing

### Monitoring

### Alerting

### Configuration

### DNS

### Licenses

### CI

### Deploys

### Documentation


## Designing APIs

## Working with the main API (Gravity)

## Platform roadmap

The Artsy platform is a family of interdependent tools and services that originate in a variety of teams and are often adapted far beyond their initial purpose. Components are constantly being expanded upon, maintained, abused, and eventually deprecated. Here is a snapshot of some components in transition.

### Out with the old:

**Gravity processing auction bids.** Historically, auctions were handled by the main API, with bids processed carefully by a single background worker to ensure atomic, sequential updates. Causality includes a better-suited architecture for near-real-time event handling. It already handles processing of live auction events, and over time should handle online bidding as well.

**Inquiry conversations.** Inquiries initially comprised simple email notifications of collectors' interest in an artwork. Since then, inquiries have grown to support shows, sophisticated email routing and aliasing, engagement metrics, invoices, and more real-time behaviors. [Impulse](https://github.com/artsy/impulse) was extracted from Gravity to be the authoritative source on collector/partner sales conversations.

**Slugs** (such as `andy-warhol-flowers`) were commonly used to programmatically identify objects from the API in place of unique, permanent identifiers (such as `5127a6d691b1fe0cfb000135`). Because names change, we now use true IDs wherever possible. For convenience, slugs are still supported in URLs and some APIs will accept slugs and redirect to authoritative resources.

**[Heat](https://heat.artsy.net/) "builds"** are a custom tool for performing large, parellelized batch processes, usually based on data exports from Gravity. These include pre-computing artwork filter results, calculating artwork similarity, and compiling user recommendations. More recently, we are transitioning to more scalable and better-supported external tools (such as Spark for large-scale in-memory batch processing or Elasticsearch for searching and filtering), purpose-built services (such as Delta for trending data), or simply strategic use of in-app background tasks.

**[Torque](https://admin.artsy.net)** or "old admin" is the original administration interface that grew complex and difficult to maintain. Our current preference is to deploy more focused management utilities, such as [CMS](https://cms.artsy.net) ([Volt](https://github.com/artsy/volt)) for partners, [Helix](https://helix.artsy.net) for genomers, and [auctions.artsy.net](https://auctions.artsy.net) ([Ohm](https://github.com/artsy/ohm)) for managing sales. We aim to _opportunitistically_ transition remaining administrative functions (such as for inquiries or site "features") to new tools.

**Gravity's v1 API** is still [very] heavily used, but some endpoints reflect an old style. Given the choice, we now prefer endpoints to be situated at the root (e.g., `/api/v1/artworks`) rather than nested (e.g., `/api/v1/partner/:id/artworks` or `/api/v1/artist/:id/artworks`). This provides maximum flexibility for supporting unanticipated relationships and filter criteria in the future. We demand that API objects comprise consistent, well-known properties (and so we avoid mixing in properties based on requester or context).

**Solr** full-text search powers the site's autocomplete (the `match/suggest` endpoint) as well as search for certain individual models. We prefer Elasticsearch for full-text search as well as sophisticated filtering these days, and are in the process of transitioning more of these endpoints to it.

**Google site search** [powers](http://artsy.github.io/blog/2014/10/23/how-we-customized-google-site-search-at-artsy/) the site's main search results page. The lack of direct control over these results has been a recurring complaint (e.g., from partners wanting changes reflected more quickly or artists upset about inaccurate titles or images). Though the API already supports full text search of major models, it doesn't include editorial articles or certain static pages. We hope to eventually transition to Elasticsearch-powered search results.

### And in with the new:

The most significant change in our work as a team is that we now pursue smaller, focused solutions where our first impulse used to be to expand the main API to support every new feature. [Causality](https://github.com/artsy/causality) and [Positron](https://github.com/artsy/positron) are examples of this.

**Spark** is where you'll find our most data-intensive operations these days. Patterns exist for [our cluster](http://spark.artsy.net:7180/) to load data from the main Gravity database as well as other systems, distribute processing among all of the available nodes, and interact with S3, HDFS, or Redshift.

**Hypermedia-style APIs** can be found in many of our recent projects. The hypermedia style avoids API and developer needing to share out-of-band information (and, e.g., implementing custom client code). Instead, the API is self-describing, with each response documenting explicitly the actions that can be performed with that data and the further data relationships that can be traversed. Gravity's v2 API, Impulse, and certain [Gris](https://github.com/artsy/gris)-based services try to adhere to the [HAL](http://stateless.co/hal_specification.html) hypermedia format.

**Gravity's v2 API** reflects some of the preferences outlined above. It also accommodates clients that are external to Artsy (usually only permitting them to access public-domain works). It is still far less complete than v1, but has strong patterns in place for making additions. Clients wanting to avoid implementing custom API-consuming code or ensure greater forward-compatibility can leverage v2 today.

**Elasticsearch** is the data store serving sophisticated filtering of artworks, partners, and sales, as well as full-text search for many other models. We expect to transition all search responsibilities to it from Solr.

**Kafka** (_experimental_) offers a stream to which our applications can publish or subscribe. This is interesting because it can decouple applications _originating_ events from those that may want to _react_ to them. E.g., a user registration could be published, with other applications receiving notice and delivering emails, creating database records, or triggering background work without the original application knowing or caring about them.

