---
layout: post
template: two-col
title:  "Nginx allow and deny by IP"
so_title: "nginx"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   2034-05-24 10:51:22
categories: 
lead: Allow and deny connections through Nginx
search-tags: ['']
tags: ['Web server']
tutorial: true
difficulty: 1
---

In addition to protecting your application (or parts of it) using [HTTP basic authentication](/articles/nginx-basic-authorization), you can use Cloud 66 [CustomConfig](http://help.cloud66.com/stack-definition/custom-config.html) to block (or allow) access to your application based on IP addresses.
Follow the instructions below to accomplish this.

<ol class="article-list">
<li>Create a file in the root of your repository called _blockips.conf_. This file will contain the IPs you wish to allow/deny.</li>
<li>To deny a single IP address, you can use the following syntax:</li>
<code>deny 1.2.3.4;</code><br>
You can also deny an entire subnet as follows:
<br><code>deny 91.212.45.0/24;</code><br>
Should you wish to only allow access to your IP address, do this:
<br><code>
allow 1.2.3.4/24;<br>
deny all;</code>
<br>There are <a href="http://www.cyberciti.biz/faq/linux-unix-nginx-access-control-howto/">lots</a> of <a href="http://wiki.nginx.org/HttpAccessModule">resources</a> about this syntax on the Internet in case you need more guidance.
<li>Now we can go ahead and customize the Nginx configuration, which you can see more about in our <a href="http://help.cloud66.com/web-server/nginx.html">Nginx CustomConfig documentation</a>.</li>

<br/>You will want to add the following code within the <i>http</i> section of your configuration, for example on line 22.

<pre class="prettyprint">
include &#123;&#123; deploy_to &#125;&#125;/current/blockips.conf;
</pre>

This will read the file from your repository directory on the server. Once you save that configuration, it will apply immediately on your server.
</ol>