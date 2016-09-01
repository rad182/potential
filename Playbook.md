<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Goals of this document](#goals-of-this-document)
- [Working together](#working-together)
  - [Project Management](#project-management)
  - [Workflow](#workflow)
    - [Fork-and-branch](#fork-and-branch)
    - [Commits](#commits)
    - [Pull requests](#pull-requests)
  - [Continuous improvement](#continuous-improvement)
  - [Coordinating between teams](#coordinating-between-teams)
- [High-level architecture and technology choices](#high-level-architecture-and-technology-choices)
  - [Monoliths vs. microservices](#monoliths-vs-microservices)
  - [Extracting services](#extracting-services)
  - [Choosing an architecture](#choosing-an-architecture)
  - [Communication patterns](#communication-patterns)
  - [Choosing a stack](#choosing-a-stack)
  - [Platform/Host](#platformhost)
  - [Naming and locating projects](#naming-and-locating-projects)
  - [When to open-source](#when-to-open-source)
- [Application-level choices](#application-level-choices)
  - [Authentication and authorization](#authentication-and-authorization)
  - [Analytics](#analytics)
  - [Metrics](#metrics)
  - [Testing](#testing)
  - [Monitoring](#monitoring)
  - [Alerting](#alerting)
  - [Configuration](#configuration)
  - [DNS](#dns)
  - [Licenses](#licenses)
  - [CI](#ci)
  - [Deploys](#deploys)
  - [Documentation](#documentation)
- [Designing APIs](#designing-apis)
- [Working with the main API (Gravity)](#working-with-the-main-api-gravity)
- [Platform roadmap](#platform-roadmap)
  - [Out with the old:](#out-with-the-old)
  - [And in with the new:](#and-in-with-the-new)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Goals of this document

This playbook was inspired by [Thoughtbot's playbook](https://thoughtbot.com/playbook), as well as internal discussions of ways for Artsy's engineering team to share a common vision and hard-won lessons internally. It aims to make good decisions easy, leaving plenty of space for experiments and even radical departures, but improving consistency where helpful. Rather than a set of policies for everyone to conform to, it's here to:
* help refine our own thinking
* create a forum for future discussion and iteration
* establish shared vocabulary
* offer a framework for when it's desired

This is a living document--a perpetual work in progress as we acquire new experiences, struggle through post-mortems, and experiment with new approaches. _You_ are the co-author, so when you're convinced a new tool or practice should be incorporated, submit a PR!


# Working together

## Project Management

Teams at Artsy use a variety of project management processes and tools including:
* [Github issues](https://guides.github.com/features/issues/)
* [Zenhub](https://www.zenhub.com/)
* [Trello](https://trello.com/)
* [Geekbot](https://geekbot.io/)
* (Others?)

Teams tend to work in short (1-2 week) _sprints_, with a planning meeting at the start and sometimes a review or retrospective at the end.

(This section is largely _TBD_, as teams don't yet share a coherent project management practice.)

## Workflow

All developers have access to all repositories. Pull requests are always welcome, but check in with responsible teams or developers for help getting up to speed.

### Fork-and-branch

Artsy teams employ the [fork-and-branch](
http://blog.scottlowe.org/2015/01/27/using-fork-branch-git-workflow/) model of project collaboration popular in open source. That means work happens on developers' own repositories (e.g., [mzikherman/force](https://github.com/mzikherman/force)) forked from the "trunk" ([artsy/force](https://github.com/artsy/force)). [Pull requests](#pull-requests) are then made from developers' working branches to the trunk's `master` branch.

### Commits

> A well-crafted git commit message is the best way to communicate context about a change to fellow developers (and indeed to [our] future selves).
> -[How to write a git commit message](http://chris.beams.io/posts/git-commit/)

The code change itself should be internally consistent and coherent. If you're modifying a method signature, propagate the change to calling code. If you've implemented a new behavior, update test assertions to match. Except in rare cases where you intend to share a work-in-progress, commits should not leave tests in a failing state.

Commit subjects should concisely describe the enhancement or fix in the imperative form ("Add tracking of email deliveries" or "Undo home page A/B test"). Git subcommands (like `git blame`, `revert`, `log`...) expect this format and become more transparent to use when you adhere to that convention.

Expand on the subject to explain _why_ and add any helpful background or links in the commit body (separated by 1 line). If your project uses Github issues, you can include keywords (like `closes #23`) to [close issues via commit](https://help.github.com/articles/closing-issues-via-commit-messages/).

Sometimes bad commit messages aren't apparent until later. Feel free to [squash](https://github.com/blog/2141-squash-your-commits) and rewrite commits on your working branch (even when it's in pull request form).

### Pull requests

The best pull requests start with good [commits](#commits). If commits are a unit of incremental, coherent change, pull requests represent sensible deploy points. (This doesn't mean they represent a complete feature or launch. See [continuous improvement](#continuous-improvement).)

At Artsy, PRs are the _most frequent_ form of collaboration.

Once again, the title should concisely explain the change or addition. The description should clearly state:
* the original problem or rationale,
* the solution eventually agreed upon,
* any alternatives considered,
* and any to-dos, considerations for the future, or fall-out for other developers.
* Finally, any pre- or post-deploy migration steps should be clearly explained. (Add the `#migration` label to call these pull requests out.)

_Assign_ another team member to the PR. All team members are encouraged to contribute, but that team member has the explicit responsibility to carefully review, consider any unanticipated impact, monitor discussion, and ultimately merge or close the PR. You may also want to @-mention other developers to request specific input.

A successful PR leaves both submitter and reviewer of 1 mind, able to justify rationale, debug unintended consequences, and monitor the graceful transition. For this reason, it's not uncommon to have PR bodies that are longer than the corresponding code changes. Pull requests are often referred to _years_ later to understand the events surrounding design decisions or code changes of interest. Keep this future reader in mind.

It's almost never too early to open a PR. Seeing real code encourages feedback that might not flow as freely in an abstract discussion. A single failing test can be enough to trigger discussion, and clearly indicates that the PR can't yet be merged. (We prefer intentionally failing tests to labels like `#dontmerge` for this purpose.)

Further reading:
* See some epic examples of great PRs: [artsy/gravity#9787](https://github.com/artsy/gravity/pull/9787), [artsy/gravity#9557](https://github.com/artsy/gravity/pull/9557)
* [On empathy and pull requests](https://slack.engineering/on-empathy-pull-requests-979e4257d158)
* [Rules of communicating at Github](http://ben.balter.com/2014/11/06/rules-of-communicating-at-github/)

## Continuous improvement

Artsy exists in a changing business environment and is itself building a shifting suite of services. Even over its short lifetime, we've witnessed systems be conceived, grow beyond reasonable maintainabilty, and be abandoned or fall out of favor. Since there's no end state (at least, that we know about), we try to be disciplined and practiced at dealing with change.

**Big things don't ship.** To ship anything, it must first be made small. Large projects can be expedited not by everyone combining efforts toward a grand vision, but by identifying the incremental, graceful steps that can be taken _now_ in the correct direction.

To that end, we want to be able to **experiment quickly and easily**. A/B tests, feature flags, role-based access, and robust metrics are investments that quickly help make new ideas more concrete.

The tools of change, including aggressive **refactoring**, **data migrations**, **frequent deploys**, and a robust **test suite**, are essential. Obstacles such as slow tests, inconsistent tooling, or unreliable deploys can hamper our efforts, so are worth significant investment despite being disconnected from user-facing features.

Finally, Artsy aims for a culture of continuous improvement to both code _and_ process. We try to share wins, teach best practices and offer constructive feedback. Tools like lunch-and-learns, [postmortems](https://github.com/artsy/post_mortems), [kaizen](https://en.wikipedia.org/wiki/Kaizen) or [retrospectives](https://en.wikipedia.org/wiki/Retrospective) help us reflect on our own practices and make change.

## Coordinating between teams

Often there's tension between meeting a single product team's goals and building what could be shared or most useful across the platform. We resist this dichotomy. Artsy's platform _is_ the cumulative work of its product teams, and will only succeed when our efforts combine to enable higher and higher-level features rather than local solutions.

Examples of such wins include:
* Kinetic and Watt libraries being shared broadly to unify management applications
* Devising a common pattern for exporting core data that is reused by many apps and consumed for accounting, analytics, and machine-learning purposes
* ...

When questions arise about worthwhile effort and investment, they can usually be resolved by respective teams conferring. Often there are opportunities to join forces and/or make deliberate, short-term compromises in pursuit of a healthful state.

# High-level architecture and technology choices

These are some of the most important and carefully considered decisions we make as engineers.

## Monoliths vs. microservices

## Extracting services

## Choosing an architecture

## Communication patterns

## Choosing a stack

## Platform/Host

## Naming and locating projects

## When to open-source

# Application-level choices

## Authentication and authorization

## Analytics

## Metrics

## Testing

## Monitoring

## Alerting

## Configuration

## DNS

## Licenses

## CI

## Deploys

## Documentation


# Designing APIs

# Working with the main API (Gravity)

# Platform roadmap

The Artsy platform is a family of interdependent tools and services that originate in a variety of teams and are often adapted far beyond their initial purpose. Components are constantly being expanded upon, maintained, abused, and eventually deprecated. Here is a snapshot of some components in transition.

## Out with the old:

**Gravity processing auction bids.** Historically, auctions were handled by the main API, with bids processed carefully by a single background worker to ensure atomic, sequential updates. Causality includes a better-suited architecture for near-real-time event handling. It already handles processing of live auction events, and over time should handle online bidding as well.

**Inquiry conversations.** Inquiries initially comprised simple email notifications of collectors' interest in an artwork. Since then, inquiries have grown to support shows, sophisticated email routing and aliasing, engagement metrics, invoices, and more real-time behaviors. [Impulse](https://github.com/artsy/impulse) was extracted from Gravity to be the authoritative source on collector/partner sales conversations.

**Slugs** (such as `andy-warhol-flowers`) were commonly used to programmatically identify objects from the API in place of unique, permanent identifiers (such as `5127a6d691b1fe0cfb000135`). Because names change, we now use true IDs wherever possible. For convenience, slugs are still supported in URLs and some APIs will accept slugs and redirect to authoritative resources.

**[Heat](https://heat.artsy.net/) "builds"** are a custom tool for performing large, parellelized batch processes, usually based on data exports from Gravity. These include pre-computing artwork filter results, calculating artwork similarity, and compiling user recommendations. More recently, we are transitioning to more scalable and better-supported external tools (such as Spark for large-scale in-memory batch processing or Elasticsearch for searching and filtering), purpose-built services (such as Delta for trending data), or simply strategic use of in-app background tasks.

**[Torque](https://admin.artsy.net)** or "old admin" is the original administration interface that grew complex and difficult to maintain. Our current preference is to deploy more focused management utilities, such as [CMS](https://cms.artsy.net) ([Volt](https://github.com/artsy/volt)) for partners, [Helix](https://helix.artsy.net) for genomers, and [auctions.artsy.net](https://auctions.artsy.net) ([Ohm](https://github.com/artsy/ohm)) for managing sales. We aim to _opportunitistically_ transition remaining administrative functions (such as for inquiries or site "features") to new tools.

**Gravity's v1 API** is still [very] heavily used, but some endpoints reflect an old style. Given the choice, we now prefer endpoints to be situated at the root (e.g., `/api/v1/artworks`) rather than nested (e.g., `/api/v1/partner/:id/artworks` or `/api/v1/artist/:id/artworks`). This provides maximum flexibility for supporting unanticipated relationships and filter criteria in the future. We demand that API objects comprise consistent, well-known properties (and so we avoid mixing in properties based on requester or context).

**Solr** full-text search powers the site's autocomplete (the `match/suggest` endpoint) as well as search for certain individual models. We prefer Elasticsearch for full-text search as well as sophisticated filtering these days, and are in the process of transitioning more of these endpoints to it.

**Google site search** [powers](http://artsy.github.io/blog/2014/10/23/how-we-customized-google-site-search-at-artsy/) the site's main search results page. The lack of direct control over these results has been a recurring complaint (e.g., from partners wanting changes reflected more quickly or artists upset about inaccurate titles or images). Though the API already supports full text search of major models, it doesn't include editorial articles or certain static pages. We hope to eventually transition to Elasticsearch-powered search results.

## And in with the new:

The most significant change in our work as a team is that we now pursue smaller, focused solutions where our first impulse used to be to expand the main API to support every new feature. [Causality](https://github.com/artsy/causality) and [Positron](https://github.com/artsy/positron) are examples of this.

**Spark** is where you'll find our most data-intensive operations these days. Patterns exist for [our cluster](http://spark.artsy.net:7180/) to load data from the main Gravity database as well as other systems, distribute processing among all of the available nodes, and interact with S3, HDFS, or Redshift.

**Hypermedia-style APIs** can be found in many of our recent projects. The hypermedia style avoids API and developer needing to share out-of-band information (and, e.g., implementing custom client code). Instead, the API is self-describing, with each response documenting explicitly the actions that can be performed with that data and the further data relationships that can be traversed. Gravity's v2 API, Impulse, and certain [Gris](https://github.com/artsy/gris)-based services try to adhere to the [HAL](http://stateless.co/hal_specification.html) hypermedia format.

**Gravity's v2 API** reflects some of the preferences outlined above. It also accommodates clients that are external to Artsy (usually only permitting them to access public-domain works). It is still far less complete than v1, but has strong patterns in place for making additions. Clients wanting to avoid implementing custom API-consuming code or ensure greater forward-compatibility can leverage v2 today.

**Elasticsearch** is the data store serving sophisticated filtering of artworks, partners, and sales, as well as full-text search for many other models. We expect to transition all search responsibilities to it from Solr.

**Kafka** (_experimental_) offers a stream to which our applications can publish or subscribe. This is interesting because it can decouple applications _originating_ events from those that may want to _react_ to them. E.g., a user registration could be published, with other applications receiving notice and delivering emails, creating database records, or triggering background work without the original application knowing or caring about them.

