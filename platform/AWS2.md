# Overview of AWS 2.0 setup

Currently, all Amazon Web Services applications (except Solr) run under one account with the login `joey@artsy.net`, and each software engineer has an IAM user for that account.

To better separate devevelopment, staging, and production environments, we have created three new AWS accounts. IAM users in the `ArtsyEngineering` group have access to the Dev environment, which can be reached by clicking on the dropdown with your name and choosing `Switch Role`. You can switch back to your main IAM account by choosing "back to [your name]" in the dropdown.

Engineers in the ArtsyStaging and ArtsyProduction account will have access to those accounts. New hires should first be added to the `ArtsyEngineering` group. 

The process that was used to set up cross-account access is described [here](https://blogs.aws.amazon.com/security/post/Tx70F69I9G8TYG/How-to-Enable-Cross-Account-Access-to-the-AWS-Management-Console), but only Tasks 1 and 2 are needed now.

At the moment, there are no systems running in these stacks, but the plan is to move all systems to one or more of these three over time. All new projects should use the new accounts.

The links an IAM user can use for one-time setup of their Role switching are below. When you click on the link, we recommend using something like Development, Staging, or Production for the description.

* [development](https://signin.aws.amazon.com/switchrole?account=857168411456&roleName=CrossAccountSignin)
* [staging](https://signin.aws.amazon.com/switchrole?account=468683317992&roleName=CrossAccountSignin)
* [production](https://signin.aws.amazon.com/switchrole?account=404343144811&roleName=CrossAccountSignin)

[MFA authenticaion](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable.html) must be enabled on your account if you have access to Production.
