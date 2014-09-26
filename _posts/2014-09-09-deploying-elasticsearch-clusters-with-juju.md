---
layout: post
title: "Deploying ElasticSearch clusters with Juju and Ansible"
date: 2014-09-09 10:06
comments: true
categories: [cloud, juju]
author: "Jorge Castro"
author_url: "@castrojo"
---

[ElasticSearch](http://www.elasticsearch.org/) is one of those tools that's just
handy to have. When combined with Logstash and Kibana, you can use the "ELK"
stack for [all sorts of things](http://www.elasticsearch.org/overview/).

One thing we'd like to see is to bring these capabilities to our users. So I've
been working on bundling together some of our ElasticSearch resources in Ubuntu
so we can bring them to the cloud.

<!-- more -->

This is a standalone ElasticSearch cluster. I've added a few things:

- A preconfigured Kibana dashboard
- A script that preloads the cluster with the works of Shakespeare, so you can [start the 10 minute introduction](http://www.elasticsearch.org/guide/en/kibana/current/using-kibana-for-the-first-time.html) right away and get to work.
- [Paramedic](https://github.com/karmi/elasticsearch-paramedic) and [Bigdesk](http://bigdesk.org/) so you can monitor your cluster.

## A Production-ready Stack

One of the nice things about this bundle is it uses our [ElasticSearch
charm](https://github.com/charms/elasticsearch). Right off the bat you're
getting a charm that we're using in production today. That means it's tested
(see the included
[tests](https://github.com/charms/elasticsearch/tree/trusty/tests), and it also
uses many of the modern techniques for writing a charm. In this case, Michael Nelson leveraged [Ansible](http://www.ansible.com/home) to do most of the heavy lifting. Our ability to consume multiple tools to get the job done is one of the nice things that we can support in charms, so if you
prefer a certain tool, then by all means use it! It also just consumes upstream
packages, so you'll always have a fresh version.

### Why ElasticSearch?

The major use of ElasticSearch is within Ubuntu's Software Store for Unity 8. On Ubuntu Touch anytime a user is searching for and navigating the store they're using ElasticSearch. ElasticSearch was chosen for a few reasons.

- It allowed us to design the store where every category is basically a search term. This allows the team to dynamically generate categories based on certain criteria. For example a list of "Top 10 apps" might be different depending whether you are in the US or in China.
- Since everything is designed around search, it lets the team "future proof" the Software Store for future categories and queries.
- ElasticSearch was designed to scale much more than other solutions we looked at. ElasticSearch can be horizontally scaled by just `juju add-unit` with no extra configuration.  
- Since the team didn't have to worry about learning how to make search it let them concentrate on the store itself and let ElasticSearch do the heavy lifting.
- Since ElasticSearch is driving the backend, this will enable Ubuntu Phone partners to offer customized views on top of the existing store without having to do major engineering work around building a branded store.
- Our team found the ElasticSearch documentation to be excellent.

## The quick way to get up and running

Ok so let's deploy this badboy. Assuming you're brand new:

    sudo apt-get install juju juju-quickstart
    juju quickstart bundle:elasticsearch/cluster

At this point follow the ncurses menus to pick which cloud you want to deploy to
and add your credentials and then Juju will bootstrap and start deploying ES.

Total deployment time will depend on where you're deploying to, but on AWS US
East 1 I can go from zero to cluster in about 10 minutes. By default I give you
one Kibana node and one ElasticSearch node. For ElasticSearch we ensure you're
node has at least 4 CPU cores and 16GB of RAM. You can [follow the
instructions](https://jujucharms.com/bundle/elasticsearch/14/cluster/?text=elasticsearch#readme)
to load up the sample data and the other sample plugins I've included. Just go
to the ip address of the Kibana unit in your browser and start searching:


Now you're ready to horizontally scale:

     juju add-unit elasticsearch

To add a new node.

## And that's it

You now have an ElasticSearch cluster. Using the same best practices that we're
doing in production. Amir Sanjar and I are prototyping bundling the
[ElasticSearch Hadoop
plugin](http://manage.jujucharms.com/bundle/~jorge/hadoop-es/baremetal) in a
bundle as well, though that's not quite ready (testing and help wanted!).

As you can see, we've barely scratched the surface. Chuck Butler's been [reworking the
Logstash
charm](https://code.launchpad.net/~lazypower/charms/trusty/logstash/trunk) for
Trusty, we expect that to land soon. We plan to make the charm a subordinate, you can
just plop it onto any unit. The idea is to enable people to put logstash's
indexer on every unit they can shove all they care about into ElasticSearch.
