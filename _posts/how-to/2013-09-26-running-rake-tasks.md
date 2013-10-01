---
layout: post
title:  "Running Rake Tasks"
date:   2013-09-26 15:33:13
categories: how-to
---

<p class="lead">You can use Cloud 66 to easily run rake tasks either automatically or manually on your stack</p>

## Introduction
You can choose to run your rake tasks automatically or manually. Running them automatically involves either scheduling them by using the rake task add-on or by using deployment hooks.
Alternatively, you can run them manually on your server. This guide will walk you through each of these.

### Scheduled
Please read more about this [rake task add-in](/add-ins/rake-task.html) in the documentation.

### Deployment hooks

You can use [deployment hooks](/stack-features/deploy-hooks.html) to execute your rake task at any point of your deployment.

Simply add a bash script to your stack that contains the rake task: for example, create the file */.cloud66/scripts/rake_task.sh* as below:
<pre class="terminal">
&#35;!/bin/bash
&#35; access your Rails stack path
cd $RAILS_STACK_PATH
&#35; run your rake task
bundle exec rake your:task
</pre>

Then, add a deploy_hook to execute the above script on each deploy: create the file *.cloud66/deploy_hooks.yml* as below:
<pre class="terminal">
production:
  after_rails:
    - source: /.cloud66/scripts/rake_task.sh
      destination: /tmp/rake_task.sh
      target: rails
      execute: true
      run_on: all_servers
      apply_during: all
      sudo: true
</pre>

## Manually
This is done by starting a [terminal connection to your server](/how-to/shell-to-your-servers.html) and executing your rake task.

<pre class="terminal">
&#35; Access your Rails stack path
cd $RAILS_STACK_PATH
&#35; Run your rake task
bundle exec rake your:task
</pre>