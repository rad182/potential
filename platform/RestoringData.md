# Restoring Data to Gravity

Things happen, and sometimes (hopefully infrequently) data is lost.

### What do you do when data is missing from the Gravity Production database?

1. Try to find an alternate source of truth
    - Is there any way to restore the data without needing a backup?
        -  Is the model that was deleted a reference to other models? (i.e. `    PartnerArtistArtworks`)
        -  Was the data recently created (and not updated) in a way that is easy     to replicate?
        -  Is the data _actually_ deleted? (i.e. we store `Artworks#deleted` if     they were deleted with `delete`, not `destroy`)
    - Has the data changed since the last copy to staging?
        - If not, you may be able to back up the necessary objects directly from     staging.
    - Can the data be inferred from a nightly report in Redshift?

    If none of the approaches above will work, then you'll need to restore data from one of our mongodb backups. At this point, it's probably worth bringing up the issue in [#platform-humans](https://artsy.slack.com/messages/platform-humans/) and pairing with someone to get an extra set of eyes.

2. Find the relevant backup
    - Is the data backed up hourly?
        - Check http://joe.artsy.net:9000/view/Gravity/job/gravity-production-hourly-backup-cron/. The collections that are backed up every hour are listed under "Configure". 
    - Log in to s3 using the _old_ (it@artsymail.com) credentials, found in 1Password. Navigate to the `/gravity-production-db-backup/db/backup` bucket and find the most recent (either daily or hourly) backup folder.

3. Figure out which backup includes the data you need (make sure to check the timezone!). Then download and unpack the necessar tar file from s3.

4. Restore the relevant collections to your _local_ mongodb.

    ```
    # i.e. if you need to restore artworks and sale_artworks
    mongorestore --db gravity_development --collection artworks ~/Desktop/tmp/db/c0.gravity.member4.mongolayer.com_27017/gravity/artworks.bson
    mongorestore --db gravity_development --collection sale_artworks ~/Desktop/tmp/db/c0.gravity.member4.mongolayer.com_27017/gravity/artworks.bson
    ```

5. Unless you have to restore the _entire_ collection, filter to just the objects you want using a mongo query, and save the output to a file.

    ```
    mongodump --host 127.0.0.1 --db gravity_development --collection artworks --    query '{"partner_id": ObjectId("57d17993a09a6759d8000d9e")}'
    mongodump --host 127.0.0.1 --db gravity_development --collection sale_artworks  --query '{"sale_id": ObjectId("57d17c71cd530e65fb00033c")}'
    ```

6. Restore the relevant collections to staging first!! (find the username and password under `MONGOHQ_URL` in Stack Settings in OpsWorks)

    ```
    mongorestore -u <user> -p <password> -h candidate.62.mongolayer.com --port  10331 --db gravity-staging -c artworks dump/gravity_development/artworks.bson
    mongorestore -u <user> -p <password> -h candidate.62.mongolayer.com --port  10331 --db gravity-staging -c sale_artworks  dump/gravity_development/sale_artworks.bson
    ```

7. When you are confident that everything is restored correctly in staging, restore the data to production (find the username and password under `MONGOHQ_URL` in Stack Settings in OpsWorks)

    ```
    mongorestore -u <user> -p <password> -h c0.gravity.member3.mongolayer.com --db  gravity -c artworks dump/gravity_development/artworks.bson
    mongorestore -u <user> -p <password> -h c0.gravity.member3.mongolayer.com --db  gravity -c sale_artworks dump/gravity_development/sale_artworks.bson
    ```
    
8. Tie up loose ends.
    - Does the collection have any `dependent: destroy` collections that aneed to be restored?
    - Is any of the restored data stale since the daily or hourly backup?
    - Do any partners or users need to be notified?
    - Does the Gravity and/or Force cache need to be cleared?

