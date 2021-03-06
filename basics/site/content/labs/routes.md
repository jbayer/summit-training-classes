---
date: 2016-04-19T19:21:15-06:00
title: Routes
---

In this exercise, you will use route mapping to perform a zero-downtime upgrade of an app.


## Create a new route

We need to create a route for the app we are going to deploy. This route will be used for _all_ versions of the app, so should stay the same even when the app is updated.

* Use `cf create-route` to a create a new route for your app, making sure the domain is `cfapps.io`

### Push v1.0

* Change to the `08-domains-routes/v1.0` directory and push the v1.0 app
* _Does the push work? If not, why not? How can you fix this?_
* Use `cf help` to figure out how to map your route above to this app
* Verify your app is accessible by accessing the route you created

#### Checking Your Work

You should be able to access your app on the route created above. You should also be able to see the route/url by looking at the app details:

```sh
cf app
```

### Push v1.1

Imagine that a new version of the app has been developed. We want to push it and check that it works.

* Push version 1.1 of the app from `08-domains-routes/v1.1` and assign it a random route
* Do not map your main route yet
* Validate you can access the app by accessing the random route

If this app was being deployed automatically, this is the point that smoke tests would be run against the new version. If the new version works on the random route, we can proceed to load-balance production traffic to it.

* Use the CF cli to map traffic hitting the main route you created to v1.1

#### Checking Your Work

Check your work by accessing the main route multiple times. You should observe traffic being balanced by Cloud Foundry across both instances. You can also see the routes/urls by looking at the app details:

```sh
cf app
```

### Cutting over

We pushed the new version, smoke tested it on its own route, and then load balanced production traffic to both the new version and the old version. Now it is time to stop traffic going to the old version of the app.

* Use `cf help` to figure out how to stop sending traffic to v1.0
* _Could you leave the v1.0 app running in case you need to roll-back?_

#### Checking Your Work

Check your work by accessing the main route multiple times.  You should observe traffic only going to v1.1.  You can also see the routes/urls by looking at the app details:

```sh
cf app
```

Congratulations! You successfully updated your running application to a new version with no downtime for users.


## Beyond the Class

  * Set up a custom [SSL certificate](http://www.selfsignedcertificate.com/)
  * Use [feature flags](https://docs.cloudfoundry.org/adminguide/listing-feature-flags.html) instead of ENV vars for new features
  * Delete all routes that are no longer used

```bash
$ cf delete-orphaned-routes
```
