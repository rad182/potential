class: center, middle, inverse

# Working with the Gravity API

(View in slide format at https://remarkjs.com/remarkise.)

.footnote[Artsy / Joey Aghion / 2015-10-07 / last updated 2016-09-09]

---

# Outline

* Development environments (briefly)
* Production environment
* Exploring from the console
* Data models and relations
* API organization and patterns
* Tracing a request
* Other references

---

# Development environments

* [rbenv](https://github.com/sstephenson/rbenv) and [ruby-build](https://github.com/sstephenson/ruby-build)
* [doc/GettingStarted.md](https://github.com/artsy/gravity/tree/master/doc/GettingStarted.md)
* `script/bootstrap`
* (Must copy staging/backup data)
* [Docker!](https://github.com/artsy/gravity/tree/master/doc/Docker.md)

# Continuous Integration

* CI on [Codeship](https://codeship.com/projects/113406)
* Crons on [Jenkins](http://joe.artsy.net:9000/)

---

# Production environment

## [AWS IAM](https://aws.amazon.com/iam/)
* Have a user? (and group authorization?)
* Have access to gravity stack(s)?
* Uploaded public SSH key?

## [AWS Opsworks](https://aws.amazon.com/opsworks/)
* Adjust capacity
* Update config

---

# Exploring from the console

```
bundle exec rake ow:console[production]
```

```ruby
kara = Artist.find('kara-walker')
# => => #<Artist _id: 4df2a118bde46c000100406c, ...>
kara.artworks.published.count
# => 46
User.find('4ec1365bd7e9bb000100028d')
# => #<User _id: 4ec1365bd7e9bb000100028d, ...>
joey = User.find_by_email('joey@artsy.net')
# => #<User _id: 4ec1365bd7e9bb000100028d, ...>
pp joey.credit_cards.last.as_document
# =>
{"_id"=><BSON::ObjectId:0x114652340 data=555248497261692a11840000>,
 "_type"=>"StripeCreditCard",
 "address_line1_check"=>"pass",
 "address_zip_check"=>"pass",
 "brand"=>"Visa",
 "created_at"=>2015-05-12 18:36:57 UTC,
 "cvc_check"=>"pass",
 "expiration_month"=>5,
 "expiration_year"=>2018,
 "external_id"=>"card_161iFP2fGpeDoSYIKmSs6GCj",
 "last_digits"=>"9246",
 "name"=>"Joseph Aghion"}
```

---

# Data models and relations

* [High-level models](https://cloud.githubusercontent.com/assets/28120/10327510/19bcfa1a-6c76-11e5-92e1-4b8524834193.png)
* [Users](https://cloud.githubusercontent.com/assets/28120/10270142/0d68183c-6ab7-11e5-814d-3188dd9e13b9.png)
* [Partners](https://cloud.githubusercontent.com/assets/28120/10270147/1a5d3a4a-6ab7-11e5-8443-2d4df4e0cc92.png)
* [Sales](https://cloud.githubusercontent.com/assets/28120/10270148/2cebad54-6ab7-11e5-8767-34be783d00a4.png)

---

# Data models and relations (cont'd)

```ruby
auction = Sale.find('she-inspires-art')
# => #<Sale _id: 55ca68087261696f8100000b, ...>
auction.sale_artworks.size
# => 24
auction.sale_artworks.map(&:artwork).flat_map(&:artists).uniq.size
# => 12
auction.sale_artworks.map(&:artwork).flat_map(&:artists).uniq.map(&:name).sort
# => ["Etel Adnan", "Fabienne Verdier", "George Shaw", "Idris Khan", "Jonathan Trayte", "Journeys by Design", "Kevin Francis Gray", "Massimo Izzo", "Sevan Bıçakçı", "VINIV", "Zaha Hadid", "Zhu Jinshi"]
auction.sale_artworks.first.bidder_positions.map(&:bids).map(&:last).map(&:amount_dollars)
# => [75000.0]
```

---

# API organization and patterns

* Rails-style file organization
* [Root endpoint](https://github.com/artsy/gravity/blob/master/app/api/v1/root_endpoint.rb)
* Anatomy of a Grape endpoint ([example](https://github.com/artsy/gravity/blob/master/app/api/v1/genes_endpoint.rb))
* Helpers ([example](https://github.com/artsy/gravity/blob/master/app/api/util/pagination_parameters.rb))
* `json_fields` ([example](https://github.com/artsy/gravity/blob/ae3405a77fc767d0ded1685c7777db566a945811/app/models/domain/sale.rb#L98))
* Versions:
  * **V1** is full-featured and heavily-exercised by client apps.
  * **V2** is a work-in-progress, but is self-describing (HAL/[hypermedia](http://apievangelist.com/2014/01/07/what-is-a-hypermedia-api/)-style) and employs strong patterns.

---

# Tracing a request

[https://api.artsy.net/api/v1/me/collector_profile](https://api.artsy.net/api/v1/me/collector_profile)

---

# Other references

* Logs
  * [Papertrail](https://papertrailapp.com/groups/262621/events)
  * [S3](https://console.aws.amazon.com/s3/home?region=us-east-1&bucket=artsy-logs&prefix=)
* Statsd/Graphite: [velocity.artsy.net](http://velocity.artsy.net)
* Exports
  * [S3](https://console.aws.amazon.com/s3/home?region=us-east-1&bucket=artsy-data&prefix=reports/)
  * Redshift
  * [Looker](https://artsy.looker.com/spaces/8)
* Data changes (Redshift)

```sql
SELECT *
FROM gravity.changes
WHERE root_type = 'Order'
ORDER BY time DESC
LIMIT 10

SELECT users.email, gravity.changes.*
FROM gravity.changes
LEFT JOIN users ON users.id = gravity.changes.modifier
WHERE root_type = 'Order' AND
  root_key = '56117036726169642700008d'
ORDER BY time
```

---

# Appendix

* [Running tests locally](https://cloud.githubusercontent.com/assets/28120/10327740/2e0f7c4c-6c79-11e5-9e97-c05e319dd130.gif)
* Finding a [failing spec](https://codeship.com/projects/113406/builds/18108528)
* Generate API docs at [http://petstore.swagger.io/](http://petstore.swagger.io/) with `http://api.artsy.net/api/v1/swagger_doc`
