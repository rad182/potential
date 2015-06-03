# Mobile Team Playbook

## Team Information

### Culture

We collectively created an article for the magazine [objc.io on Artsy Mobiles's culture](http://www.objc.io/issue-22/artsy.html).

### Team Goals

* Create delightful, focused, mobile native experiences.
* Aggressively Open by Default.

### Who is on the Mobile Team?

Current members of the Mobile Team can be found on the [Artsy/Mobile](https://github.com/artsy/mobile/) GitHub repo.

### Logistics

* Bi-weekly stand-up on Tuesday / Thursday 11:00 (NYC time)
    * What are you working on or need help with?
    * Explain any new features or requests.

* Work is done through GitHub issues within sprint cycles.
    * Sprint Cycles timing varies between projects.
    * Milestones, and features are discussed and setup at a pre-sprint meeting.
    * Issues are created and added to the sprint milestone.

* Team-wide issues and TODOs can be found in [Artsy/Mobile](https://github.com/artsy/mobile/).
* The Mobile team specific [calendar](https://www.google.com/calendar/embed?src=artsymail.com_bke4sctkn8o072rjgtcsrrun3s%40group.calendar.google.com&ctz=America/New_York) is used for Out Of Office / Events.
* Chat in [#mobile](https://artsy.slack.com/messages/mobile/)
* Each app has its own focused channel in Slack [#eigen](https://artsy.slack.com/messages/eigen/), [#folio](https://artsy.slack.com/messages/folio) and [#eidolon](https://artsy.slack.com/messages/eidolon/).
* Github notifications, etc. in [#mobile-notifications](https://artsy.slack.com/messages/mobile-notifications/).

### Our Apps

* [Eigen](https://github.com/artsy/eigen): The Artsy iOS App.
* [Energy](https://github.com/artsy/energy): Artsy Partner's Portfolio App.
* [Eidolon](https://github.com/artsy/eidolon): Kiosk App for Auctions.

* See the [engineering projects map](https://trello.com/b/VLlTIM7l/artsy-engineering-projects-map) for a comprehensive list of Mobile Team libraries.

### Our Key Technologies

#### Objective-C

Swift is nice, but not production ready.

#### CocoaPods + Bundler

We rely on dependency managers, and plugins within them. If this is new to you, we recommend reading the [Podfiles](https://guides.cocoapods.org/) and [Gemfiles](https://guides.cocoapods.org/using/a-gemfile.html) guides on CocoaPods.

#### Testing

We test our apps to ensure that our bevior is what we expect. We use a collection of [FBSnapshot](http://www.objc.io/issue-15/snapshot-testing.html) Tests and Unit Tests via [Specta/Expecta](https://github.com/specta/specta).

#### Eigen

* **Useful:** [ORStackView](https://github.com/orta/ORStackView/), [ARAnalytics](https://github.com/orta/ARAnalytics) + [RAC DSL](https://github.com/artsy/eigen/blob/6bcacede194ca5b948e916746d313e7c96ec085e/Artsy/Classes/ARAppDelegate%2BAnalytics.m) and [ReactiveCocoa](http://reactivecocoa.io).
* **Essentials:** Auto Layout, [JLRoutes](https://github.com/joeldev/JLRoutes)

#### Energy

* **Useful:** [ORStackView](https://github.com/orta/ORStackView/), [ARAnalytics](https://github.com/orta/ARAnalytics).
* **Essentials:** Core Data, [DRBOperationTree](https://cocoapods.org/pods/DRBOperationTree)

#### Eidolon

* **Essentials:** Swift, [ReactiveCocoa](http://reactivecocoa.io), Storyboards
