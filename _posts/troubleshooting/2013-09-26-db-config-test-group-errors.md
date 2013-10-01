---
layout: post
title:  "Errors due to different group configs in database.yml"
date:   2013-09-26 15:33:13
categories: troubleshooting
---

<p class="lead">Errors can occur if your specified adapter is different between your "development" and "test" groups</p>

## The basics

Errors can occur during deployments due to there being different adapters defined in the "development" and the "test" groups in your database.yml file.
Your error will differ depending on the adapters you've chosen.

For example, if your database.yml file's "development" group contains:
<pre class="terminal">adapter: postgresql</pre>

And it also contains a "test" group with:
<pre class="terminal">adapter: mysql2</pre>

This will result in the following slightly obtuse error during your code deployment:
<div class="error">
uninitialized constant Mysql2
</div>