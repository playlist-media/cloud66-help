---
layout: post
template: two-col
title:  "Running rake tasks"
so_title: "rake"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   1870-09-26 15:33:13
categories: 
lead: Run rake tasks automatically or manually on your stack
search-tags: ['']
tags: ['Customization']
tutorial: true
difficulty: 0
---


## Introduction
You can choose to run your rake tasks automatically or manually. Running them automatically involves either scheduling them by using the rake task add-on or by using deploy hooks. Alternatively, you can run them manually on your server. This guide will walk you through each of these.

### Scheduled
Read more about the [rake task add-in](http://help.cloud66.com/stack-definition/rake-task.html) in the documentation.

### Deployment hooks

You can use [deploy hooks](http://help.cloud66.com/deployment/deploy-hooks.html) to execute your rake task at any point of your deployment.

Simply add a bash script to your stack that contains the rake task: for example, create the file `/.cloud66/scripts/rake_task.sh` as below:

<pre class="prettyprint">
&#35;!/bin/bash
cd $STACK&#95;PATH
bundle exec rake your:task
</pre>

Then, add a deploy&#95;hook to execute the above script on each deploy: create the file `.cloud66/deploy_hooks.yml` as below:

<pre class="prettyprint">
production:
  after&#95;rails:
    - source: /.cloud66/scripts/rake&#95;task.sh
      destination: /tmp/rake&#95;task.sh
      target: rails
      execute: true
      run&#95;on: all&#95;servers
      apply&#95;during: all
      sudo: true
</pre>

## Manually
This is done by starting a [terminal connection to your server](http://help.cloud66.com/stack-definition/ssh-to-server.html) and executing your rake task.

<pre class="prettyprint">
$ cd $STACK&#95;PATH
$ bundle exec rake your:task
</pre>