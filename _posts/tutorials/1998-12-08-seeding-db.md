---
layout: post
template: two-col
title:  "Seeding your database"
so_title: "seed"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   1750-12-08 09:33:13
categories: 
lead: Seed your database with your data during deployment
search-tags: ['']
tags: ['Database']
tutorial: true
difficulty: 0
---
New databases commonly require seed data in order to be functional for an application.
When you deploy a new stack with Cloud 66 (and choose to have a new database provisioned), we will provision that database for you. However, the database will not have any data in it.

To address this, you can make use of a [deploy hook](http://help.cloud66.com/deployment/deploy-hooks.html) to execute your required scripts and seed your database during deployment. This guide will show you how to create a deploy hook and custom script to seed your database.

## Seed script

Simply add a bash script to your repository that contains the script for seeding your database.

This could be a *custom script*, a *custom rake command*, or the default Rails `rake db:seed command` depending on your requirements/implementation. 
In our example, we will use the default Rails [rake db:seed command](http://edgeguides.rubyonrails.org/migrations.html#migrations-and-seed-data). 

Create the file `/.cloud66/dbseed.sh` as below:

<pre class="prettyprint">
&#35;!/bin/bash
cd $STACK&#95;PATH
bundle exec rake db:seed
</pre>

## Deploy hook

Add a deploy hook to execute the above script during the first deploy (on the first server only). 

Create the file `.cloud66/deploy_hooks.yml` as below (replacing *production* with your target environment).

<pre class="prettyprint">
production:
  after&#95;symlink: # Or use after_rails depending on your application
    - source: /.cloud66/dbseed.sh
      destination: /tmp/dbseed.sh
      target: rails
      execute: true
      run&#95;on: single&#95;server
      apply&#95;during: build&#95;only      
</pre>

<div class="notice">
    <h3>Note</h3>
    <p>The deploy hook example above will only execute during the <i>build</i> for a new stack. If you want to seed an existing stack you could either
    <ul class="article-list">
    	<li>Execute the seed command manually, or</li>
    	<li>Change the <i>apply&#95;during</i> specification of the deploy hook (could be used for DB data resets during subsequent testing deploys for instance)</li>
    </ul>
</div>
