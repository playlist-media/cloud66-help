---
layout: post
template: one-col
title:  "Applying upgrades"
so_title: "upgrade"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   1561-01-01 15:33:13
categories: stack-definition
lead: Best practices for applying upgrades to your stack
search-tags: ['']
tags: ['']
---

<h2>Contents</h2>
<ul class="page-toc">
    <li><a href="#about">About applying upgrade packages</a></li>
    <li><a href="#types">Upgrade package types</a></li>
        <ul style="margin-bottom:0em">
	        <li><a href="#updates">Security updates</a></li>
	        <li><a href="#ubuntu">Ubuntu</a></li>
	        <li><a href="#ruby">Ruby</a></li>
	        <li><a href="#rails">Rails</a></li>
        </ul>
    </li>
    <li>
        <a href="#manual">About manual upgrades</a>
    </li> 
</ul>

<h2 id="about">About applying upgrade packages</h2>
Cloud 66 aims to make it easier to build [immutable infrastructure](http://www.chadfowler.com/blog/2013/06/23/immutable-deployments/). Building servers and stacks from scratch is much better than modifying existing server configurations and tinkering with settings until things start to work.

Of course everyone knows that, the reasons they don't do it is that it's difficult, time consuming and can be unpredicatble. That's why we want to make building stacks from scratch as easy and as quick as possible. So in all cases of upgrade, our first recommendation is to build a new stack and redirect your traffic to the new stack using our [Elastic Address](/dns/elastic-address.html).

We are always working to make it easier to build a new stack, move your data and switch your traffic arround but it might not always be what you want to do or as easy as you would like it to be. So here is what we suggest as alternatives and exceptions.

<h2 id="types">Upgrade package types</h2>
<h3 id="updates">Security updates</h3>

In the event of a security vulnerability on any of the components we deploy on the servers, we aim to be as quick as possible to roll out the recommended patches for Ubuntu, Ruby and Rails.

<h3 id="ubuntu">Ubuntu</h3>
From the _Deploy_ menu, choose _Deploy with Options_. By selecting _Apply security upgrades_, Cloud 66 will perform operating system security package upgrades and set up [unattended upgrades](https://help.ubuntu.com/community/AutomaticSecurityUpdates) for your stack. Unattended upgrades will automatically check for and install the latest Ubuntu security packages on a daily basis.

Note that some security packages may require a server restart. We don't automatically restart your server, and it is at your discretion to do so. If the file `/var/run/reboot-required` exists, your server does in fact require a restart. To see which packages contributed to the requirement for a restart, please see `/var/run/reboot-required.pkgs`.

<h3 id="ruby">Ruby</h3>
There are generally three ways to upgrade Ruby on your stack, in decreasing magnitude of risk. Please ensure that the upgrades and patches work with your code before applying them. Upgrade and patch your development and test environments to ensure there are no issues. Backup your environment via your cloud provider where possible.

#### In-place upgrades
Performing in-place Ruby upgrades on your stack carries some risk. Our deployment process always deploys the latest release of Ruby on new servers, so all new stacks and scaled up servers will have the latest version of Ruby installed.

We roll out automatic upgrades in case of security issues, and this will be made clear in your [StackScore](/stack-definition/stack-definition.html). You will need to redeploy your stack with the _Apply Ruby upgrades_ option from _Deploy with Options_ menu which will apply the security patches and then redeploy your application as usual.

If you have updated your base Ruby version in your Gemfile, we will attempt to upgrade your Ruby version to the latest patch version of your specified base version during the _Apply Upgrades_ step - note that there may be some server downtime during the Ruby base version upgrade operation.

When you _Deploy with options_ and select _Apply Ruby upgrades_, in addition to other upgrades, we will upgrade your installed LibYAML version if we detect your version is not current.

<div class="notice notice-danger">
    <h3>Warning!</h3>
    <p>If you are upgrading your Ruby base version then you should put your stack in maintenance mode first as Passenger-based stacks (and possibly others) will have some down-time during the upgrade.</p>
</div>

#### Scaling up
As an alternative to in-place upgrades, you can specify your new Ruby version in a [manifest file](/stack-definition/manifest-files.html). Once you've pushed this change and deployed, scale up a new web server, which will use this version of Ruby. The previous server would remain on the old version of Ruby.

There are a couple of small caveats to be aware of though - after you've done this process, you'll have servers in your stack on different Ruby versions. If you were to enforce a Ruby version in your Gemfile, this would mean that your application would stop working on either one of the servers (depending on which version you chose in your Gemfile).

You can scale-down your older web server to ensure all your web servers are the correct version, but your backend servers will still be the older version of Ruby. This may or may-not have any implication, depending on what you're doing with your servers. However, if you were to then run the "Ruby upgrade" job, this would sync all your servers to your new version of Ruby, so your backend servers would be upgraded at that point too.

#### Building a new stack
Although the in-place Ruby base version upgrade path is provided for simplicity and ease, the <i>least risk strategy</i> remains to apply the version changes to a new stack in parallel, and switch over when appropriate (as per the immutable infrastructure guidelines).

<h3 id="rails">Rails</h3>
You can bump up the Rails version in your `Gemfile` and redeploy your stack. This will upgrade your Rails. Ensure that you upgrade your Ruby and Rails applications with [best practices](http://edgeguides.rubyonrails.org/upgrading_ruby_on_rails.html).

<h2 id="manual">About manual upgrades</h2>
If you need to upgrade any part of your stack the best course of action is always to build a new one. However, if that is not desired or possible, you can always perform manual upgrades.

We detect the version of all the components we have configured or deployed on your servers on a regular basis and after each deployment. If you upgrade any part of your stack manually, the upgrade will be detected by Cloud 66. This helps with the future automated upgrades.