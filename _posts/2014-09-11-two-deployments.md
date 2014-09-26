---
layout: post
title: "A Tale of Two Deployments"
date: 2014-09-11 10:06
comments: true
categories: [cloud, juju]
author: "Benjamin Saller"
author_url: "@bcsaller"
---

At the Juju Solutions sprint this week we saw a nice example of using the Rails
charm to deploy a sample app/workload right out of github
<https://github.com/leereilly/github-high-scores.git>. The rails charm is a
great example of framework charm in that it helps automate many of the common
tasks around running and managing a Ruby Rack application. I've spent the last few
months working on a system that also helps deploy and manage applications,
CloudFoundry.

By adding a few lines to a clean checkout of the project (a manifest.yml) I was
able to deploy the same application to a Juju deployed and managed CloudFoundry
instance.

<!-- more -->

Just to show the scope of change, look at the following:

    ---
    applications:
    - name: highscore
      buildpack: https://github.com/cloudfoundry/heroku-buildpack-ruby.git
      command: ruby app.rb $PORT


This is all that is needed to bring a supported type of application into
management under CloudFoundry. Because all the conventions around application
management are provided by CF itself this is all that is needed to 'charm' an
application.

From this perspective I'd like to compare and contrast these two solutions to
running this simple app. With the rails charm you'd set service options to
point to the github repo and the charm is written in such a way as to pull down
the app and bootstrap it. This isn't that different that what you'd do with cf
push.

So, differences? By default with Juju you get a new machine per Rails app you
want to run. This means that deploying the Rails app now means a bootstrap,
unit allocation and then the spin up of the configured application. Let's say
this whole process is about 15 minutes for the first unit and about 10 for each
subsequent unit. Its not fast but you're only paying for the machines you need
in a public cloud situation.

In the CloudFoundry world you'll have to deploy that first. Thats a 45 minute
process that depending on your configuration will start with 1 very large
instance or more commonly about 9 medium sized ones. If you're using the
platform to run a simple app like this one it would be a very expensive way to
do it. That said once its running you have a platform that supports running
many isolated applications with security, user and org management, and oh the
application deployments are very fast. In this case it took about 30 seconds to
provision the application the first time and each additional takes less (as
internally is using an image for its container).

Speed isn't the only difference though. In the Juju world you'd be able to
relate your application to anything the Rails charm supported. For example your
Juju deployed database. If the Rails charm predefined a mysql relation then you
could make that available to your application and changing that relation could
trigger restarts of the app.

CloudFoundry will have its own system for dealing with "relations". While not
the same thing in their model they also support a service model and the ability
to add catalogs of services that can be used by a pushed app. In this model the
service bindings are declared as needed and its up to the application to take
advantage of them. Its important to understand that in that model if your Ruby
code supported MongoDb and that service is in the catalog you could use it. In
the Juju model if the Rails charm doesn't support MongoDB you have to fork what
is supposed to be a generic charm to add that relation to metadata.yaml or
forego the use of Juju to orchestrate the applications use of Mongo.

I'm hopeful that we'll be able to provide a sensible bridge to CF application
so they can take advantage of the Juju Charm catalog in the future, but I'm
equally hopeful that extend Juju's model in such a way that we can separate the
handling of deploying a Rails charm with its needs and requirements from those
of the application payload that it manages. This is the idea of Juju deploying
things that we deploy things onto. CF might be one example of a Juju deployed
thing we deploy other things to, but at the same time, so is the Rails charm,
so is Mesos, so is Kubernetes. Deploying things into things we've deployed is
an exciting growth path for Juju and a topic for another day.
