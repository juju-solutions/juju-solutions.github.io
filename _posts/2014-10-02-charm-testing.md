---
layout: post
title: "Charm Testing"
date: 2014-10-02 11:00
comments: true
categories: [cloud, juju]
tags: ["testing"]
author: "Tim Van Steenburgh"
author_url: "@tvansteenburgh"
---

To help ensure the quality of items in the Charm Store, we've
been working on automated testing for charms and bundles. What follows
is a summary of the current state of charm testing, plans for the
future, and some testing-related tips and recommendations for charm
authors.

## Overview

We've set up a [Jenkins
slave](http://juju-ci.vapour.ws:8080/job/charm-bundle-test/) dedicated
to charm and bundle testing. A new test run is triggered when:

* A new or updated charm or bundle is pushed to the store.
* A new or updated charm or bundle enters the [Review
  Queue](http://review.juju.solutions/).

All test results are published
[here](http://reports.vapour.ws/charm-tests-by-charm). Items in the
Review Queue are also automatically updated to reflect the status of their
latest test run.

## What's Tested?

Tests are executed by
[bundletester](https://github.com/juju-solutions/bundletester), which by
default will run:

* `charm proof`, the charm linter
* Any executable files in a `tests/` dir in the charm. Typically these
  will be [amulet](https://github.com/marcoceppi/amulet) tests.
* `make test` and `make lint`, if a Makefile exists in the charm and
  these targets are defined.

The Jenkins job doesn't actually run `bundletester` directly. Instead,
it runs
[charmguardian](https://github.com/juju-solutions/charmguardian), a
wrapper around `bundletester` that adds some features handy for
automated testing, including:

* Fetching charm and bundles sources from a variety of different url
  formats
* Testing related bundles when a charm changes
* Running tests in parallel across different substrates
* Aggregating json test results

## Tips for Charm Authors

Automated testing is becoming the first step in every charm review.
Tests will need to pass before a Charmer will review your charm. Here
are some guidelines for good tests:

### Run bundletester

To make sure your tests pass, run them the way Jenkins will -- with
`bundletester`. [Install
it](https://github.com/juju-solutions/bundletester#installation), then
run `bundletester -F` in your charm directory.

### Ensure dependencies are installed

If your charm has `make lint` or `make test` targets, make sure
they have prerequisite targets that install the dependencies. Here's a
sample charm Makefile that does this:

<script
src="https://gist.github.com/tvansteenburgh/927a8f440fd456630106.js"></script>

### Configure bundletester behaviour

Bundletester uses reasonable defaults to find and execute your tests,
but you can [change its
behaviour](https://github.com/juju-solutions/bundletester#test-directory)
by including a `tests/tests.yaml` file in your charm.

## Next Steps

* Merge charmguardian and bundletester projects, combining best features
  of both into one tool.
* Add container (lxc or docker) support to bundletester so that test runs can be
  executed in a clean, isolated, repeatable environment.

Thanks for reading! If you have questions or comments, feel free to ping
tvansteenburgh in #juju on Freenode!
