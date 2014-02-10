---
layout: post
template: two-col
title:  "Backup download"
date:   2013-01-18 01:01:01
categories: toolbelt
lead: Download your database backups through the command line
---

Allows you to download a database backup through the command line, concatenating separate files into one automatically if it consists of numerous files.

## Usage
{% highlight bash %}
$ c66 download_backup --stack STACK_UID --backup-id BACKUP_ID
{% endhighlight %}

<h3>Parameters</h3>
* stack - UID of the stack (alias: _s_)
* backup_id - The ID of the backup you'd like to download (alias: _b_)

<h3>Example</h3>
{% highlight bash %}
$ c66 download_backup -s 704dc1729888eae6912f22ce24f68129 -b 15
{% endhighlight %}