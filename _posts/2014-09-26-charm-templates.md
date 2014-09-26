---
layout: post
title: "Charm Templates"
date: 2014-09-26 17:00
comments: true
categories: [cloud, juju]
tags: ["charm-tools", "charm-helpers"]
author: "Cory Johns"
author_url: "@johnsca"
---

Within [Charm Tools](https://juju.ubuntu.com/docs/tools-charm-tools.html),
the `charm create` command makes it easy to create a charm that follows
the recommended structure and includes all the right pieces.  While the `-t`
option is discussed in the documentation, I wanted to take a moment to call
it out specifically, as well as briefly talk about the new default template.

<!-- more -->

The purpose of the `-t` option is to create a new charm using a language-specific
template.  This makes it very easy to start a new charm in your favorite language.
For example, to create a new Bash charm, simply run the following:

{% highlight bash %}
$ charm create -t bash my_charm
{% endhighlight %}

If you prefer to write your charms in Python (which is recommended, and has been
the default for some time now):

{% highlight bash %}
$ charm create -t python my_charm
{% endhighlight %}

New templates will continue to be added for more languages and style, such
as the recently added Ansible charm template, and of course [submissions](
https://launchpad.net/charm-tools) for your favorite language are much
appreciated.


### The New Default Charm Template

With the newest version of Charm Tools, the default template is not only
Python-based and includes the [Charm Helpers](http://pythonhosted.org/charmhelpers/)
library, but it also leverages some [new features](
http://pythonhosted.org/charmhelpers/examples/services.html)
from Charm Helpers to make managing the charm lifecycle much easier.

For example, the following block from the refactored [Chamilo charm](
http://bazaar.launchpad.net/~johnsca/charms/precise/chamilo/services-framework/files)
encapsulates all of the decision-making logic that was previously spread between several
different hooks, in a single, relatively easy to understand block:

<script src="https://gist.github.com/johnsca/23d339b15c9e701edfb7.js"></script>

This is a new way of thinking about the decisions that your charm has to make to know
if and when it is ready to configure and start its software, but the template
sets your new charm up with all of the bits in place to use this new pattern, and
commented placeholders and examples to make it easy to see what to add where.
The new template also includes boilerplate test cases to encourage proper
testing of your charm.

And, if you want a more bare-bones Python charm that still includes Charm Helpers
and all the proper files in the right places, it&apos;s as easy as:

{% highlight bash %}
$ charm create -t python-basic
{% endhighlight %}
