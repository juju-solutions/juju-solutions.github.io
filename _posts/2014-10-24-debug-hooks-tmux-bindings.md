---
layout: post
title: "Using the Default tmux Key Bindings with debug-hooks"
date: 2014-10-24 15:00
comments: true
categories: [cloud, juju]
tags: ["debug-hooks", "tmux", "customization"]
author: "Cory Johns"
author_url: "@johnsca"
---

The Juju `debug-hooks` command is a great tool when developing charms.
However, one point of friction I&apos;ve had with it is the lack of
customizability of the environment it creates, particularly with the
screen-style key bindings that tmux is configured to use.

This week, however, I discovered it was fairly trivial to revert the
key bindings for tmux in a debug-hooks session.  All that is needed
is to create (or replace) `/home/ubuntu/.tmux.conf` with an empty file.
This can be done quite easily with a `juju ssh` command, and so I set
up the following alias on my local machine:

{% highlight bash %}
function juju-debug-hooks() {
    unit="$1"
    juju ssh $unit 'echo -n > .tmux.conf' && \
    juju debug-hooks "$@"
}
{% endhighlight %}

Now, instead of `juju debug-hooks`, I use `juju-debug-hooks` and I get
normal tmux bindings, including using Ctrl-B as the prefix instead of
Ctrl-A so that I can once again use Ctrl-A to jump to the start of the
current command line in bash.
