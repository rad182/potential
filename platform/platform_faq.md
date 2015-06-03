# Platform Team FAQ

How to...

#### Suggest a feature or point out a bug
Contact any Platform team member

#### Get a read-only data console:
https://docs.google.com/a/artsymail.com/document/d/1hCTBaynWLnSxgNQEWRTU1nm27KIizQiLgO5b0jHs7hA/edit

#### Add a new AWS user
1. Navigate to IAM from the AWS dashboard
2. Users > Create new users
3. Choose user’s name (e.g., jill) > Create
4. Copy the generated access key ID and secret access key for later
5. Users > click on the new user > Add user to groups
6. Select appropriate groups (e.g., engineering, reporting) > Add to groups
7. On user’s page, Manage password > Assign auto-generated password > Apply
8. Copy new password for later
9. The new user can log in at Artsy’s dedicated sign-in page: https://artsy.signin.aws.amazon.com/console
10. Send the user their name, password, access key ID, secret access key, and the sign-in link
11. For SSH access, the user will need to upload their SSH public key to their new account

#### Scale the API’s server capacity
(e.g., in advance of emails or expected traffic surges)

1. Log in to AWS with your IAM account at [Artsy’s dedicated sign-in page](https://artsy.signin.aws.amazon.com/console) (request an account if you don’t have one)
2. In the Services menu, choose OpsWorks and navigate to the `gravity-production` stack
3. Navigate to Time-based in the Instances menu. Under Rails App Server, this page displays all available time-based servers and solid green squares for hours when they’re scheduled to run.
4. Choose the day you would like to add capacity.

![adding instances UI](images/platform_adding_instances.png)

There are likely already some time-based servers scheduled to run during peak traffic periods (generally corresponding to the business day). Click the corresponding squares (changing them from inactive/gray to active/green) to add capacity during selected hours.

Note that hours are displayed in UTC, so be sure to convert as needed.
If you are trying to start servers immediately, note that server start-up can take 15-20 minutes (after which they’ll begin accepting requests).

#### Access the redshift data warehouse
TODO

#### Restart Jenkins
* Sometimes the jenkins server (http://joe.artsy.net:9000) goes down.
* Try SSH-ing: `ssh joe@joe.artsy.net` (password is in Engineering 1Password vault)
* Restart the jenkins service:
```
sudo su
/etc/init.d/jenkins status  # Jenkins Continuous Integration Server is not running
/etc/init.d/jenkins start  # =>  * Starting Jenkins Continuous Integration Server jenkins
```

