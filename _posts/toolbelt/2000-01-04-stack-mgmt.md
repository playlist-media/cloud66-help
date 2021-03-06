---
layout: post
template: two-col
title:  "Stack management"
date:   2015-01-27 01:01:01
categories: toolbelt
lead: Deploy your stacks from the command line
search-tags: ['deployment','toolbelt','commandline']
tags: ['Toolbelt']
---

<h2>Contents</h2>
<ul class="page-toc">
    <li><a href="#x">Stack management</a></li>
            <li>
                <ul>
                <li><a href="#redeploy">Redeploy your stack</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#list-set">List and set stack settings</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#restart">Restart Nginx</a></li>
                </ul>
            </li> 
            <li>
                <ul>
                <li><a href="#clear">Clear caches</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#list">List your stack servers</a></li>
                </ul>
            </li>              
            <li>
                <ul>
                <li><a href="#open">Open your website</a></li>
                </ul>
            </li>                                                                               
</ul>

<h2 id="redeploy">Redeploy your stack</h2>

Trigger the deployment of a stack from the command line, just like clicking on <i>redeploy</i> in the UI.

<h3 id="usage-redeploy">Usage</h3>
{% highlight bash %}
$ cx redeploy [-s <stack>]
{% endhighlight %}

<h3 id="params-redeploy">Parameters</h3>
<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
        <tr>
        	<td><i>e</i> (optional)</td>
        	<td>Your stack environment</td>
        </tr>
    </tbody>
</table>

<h3 id="example-redeploy">Example</h3>
{% highlight bash %}
$ cx redeploy -s "My Awesome App" -e production
{% endhighlight %}

Deploying a stack that is already being deployed will enqueue your redeploy command and will run it immediately after the current deployment is finished.

<h2 id="list-set">List and set stack settings</h2>

Start off by listing the possible settings for a specific stack.

{% highlight bash %}
$ cx settings [-s <stack>]
{% endhighlight %}

These are the available settings:

<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Setting</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>git.branch</i></td>
            <td>Change the Git branch of the stack repository</td>
        </tr>
        <tr>
            <td><i>git.repository</i></td>
            <td>Change the Git repository URL</td>
        </tr>
        <tr>
            <td><i>reconfigure.nginx</i></td>
            <td>If set to true, it will regenerate Nginx configuration and restart it (only on the next deployment)</td>
        </tr>
        <tr>
            <td><i>allowed.web.source</i></td>
            <td>IP addresses that are allowed to access a stacks web servers (with IP addresses in the command or a CSV file with IP addresses as input)</td>
        </tr>
        <tr>
            <td><i>asset.prefix</i></td>
            <td>If you change your default Rails assets folder, you can set it here</td>
        </tr>
        <tr>
            <td><i>stack.name</i></td>
            <td>Change your stack name</td>
        </tr>
    </tbody>
</table>



<h3 id="usage">Usage</h3>
{% highlight bash %}
$ cx set [-s <stack>] <setting> <value>
{% endhighlight %}

<h3 id="parameters">Parameters</h3>

<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
        <tr>
            <td><i>setting_name</i></td>
            <td>A valid setting from the list above</td>
        </tr>
        <tr>
            <td><i>value</i></td>
            <td>A valid value for the setting</td>
        </tr>
        <tr>
            <td><i>e</i> (optional)</td>
            <td>Your stack environment</td>
        </tr>
    </tbody>
</table>

<h3 id="example">Example</h3>

{% highlight bash %}
$ cx set -s "My Awesome App" git.repository git://github.com/cloud66-samples/rails-mysql.git -e production
{% endhighlight %}

<h2 id="restart">Restart Nginx</h2>
Allows you to restart Nginx on your stack with one simple command.

<h3 id="restart-usage">Usage</h3>
{% highlight bash %}
$ cx restart [-s <stack>]
{% endhighlight %}

<h3 id="restart-params">Parameters</h3>
<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
        <tr>
            <td><i>e</i> (optional)</td>
            <td>Your stack environment</td>
        </tr>
    </tbody>
</table>

<h3 id="restart-example">Example</h3>
{% highlight bash %}
$ cx restart -s "My Awesome App"
{% endhighlight %}

<h2 id="clear">Clear caches</h2>
For improved performance, volatile code caches exist for your stack. It is possible for a those volatile caches to become invalid if you switch branches, change git repository URL, or rebase or force a commit. Since switching branch or changing git repository URL is done via the Cloud 66 interface, your volatile caches will automatically be purged. However, rebasing or forcing a commit doesn't have any association with Cloud 66, so this command can be used to purge the exising volatile caches.

<h3 id="x-usage">Usage</h3>
{% highlight bash %}
$ cx clear-caches [-s <stack>]
{% endhighlight %}

<h3 id="x-params">Parameters</h3>
<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
    </tbody>
</table>

<h3 id="x-example">Example</h3>
{% highlight bash %}
$ cx clear-caches -s "My Awesome App"
{% endhighlight %}

<h2 id="list">List your stack servers</h2>
<h3 id="y-usage">Usage</h3>
{% highlight bash %}
$ cx servers [-s <stack>] [<names>]
{% endhighlight %}

<h3 id="y-params">Parameters</h3>
<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
        <tr>
            <td><i>names</i> (optional)</td>
            <td>A list of server names entered as separate parameters.</td>
        </tr>
    </tbody>
</table>

<h3 id="y-example">Example</h3>
{% highlight bash %}
$ cx servers -s "My Awesome App"
{% endhighlight %}


<h2 id="open">Open your website</h2>
<h3 id="z-usage">Usage</h3>
{% highlight bash %}
$ cx open [-s <stack>] [<server name>|<server ip>|<server role>]
{% endhighlight %}

<h3 id="z-params">Parameters</h3>
<table class='table table-bordered table-striped table-small'>
    <thead>
        <tr>
            <th align="center">Parameter</th>
            <th align="center">Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><i>stack</i></td>
            <td>Name of your stack</td>
        </tr>
        <tr>
            <td><i>server name</i> (optional)</td>
            <td>&mdash;</td>
            <td>Name of the server to access</td>
        </tr>
        <tr>
            <td><i>server ip</i> (optional)</td>
            <td>&mdash;</td>
            <td>IP of the server to access</td>
        </tr>
        <tr>
            <td><i>server role</i> (optional)</td>
            <td>&mdash;</td>
            <td>Role of the server to access (eg. web)</td>
        </tr>
    </tbody>
</table>

<h3 id="z-example">Example</h3>
{% highlight bash %}
$ cx open "My Awesome App"
{% endhighlight %}