---
layout: post
template: two-col
title:  "Manifest files"
nav_sticky: false
date:   2092-01-25 16:27:22
categories: stack-features
lead: You can be more explicit about your stack composition
---

<h2>Contents</h2>
<ul class="page-toc">
	<li>
		<a href="#intro">Manifest files</a>
        <li>
            <ul>
            <li><a href="#sample">Sample file</a></li>
            </ul>
        </li>
	</li>
	<li>
		<a href="#environments">Environments</a>
	</li>
	<li>
		<a href="#apps">Application types</a>
	</li>
	<li>
		<a href="#servers">Server configurations</a>
	</li>
	        <li>
                <ul>
                <li><a href="#shared">Shared</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#external">External</a></li>
                </ul>
            </li>
	<li>
		<a href="#app-specific">Application specific</a>
	</li>
	        <li>
                <ul>
                <li><a href="#rails">Rails</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#postgresql">PostgreSQL</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#redis">Redis</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#memcached">Memcached</a></li>
                </ul>
            </li>
            <li>
                <ul>
                <li><a href="#haproxy">HAProxy</a></li>
                </ul>
            </li>
</ul>

<h2 id="intro">Manifest files</h2>

Manifest files allow you to be more explicit about your stack composition by specifying additional packages you wish to install, server sizes/regions and other options.

To use this functionality, you need to place a file called **manifest.yml** in a folder named **.cloud66**, that is in turn located in the root of your source code and checked into your repository.

<pre class="terminal">
[source&#95;repo]/.cloud66/manifest.yml
</pre>

The manifest.yml file is **YAML** formatted and is split by environment just like database.yml or mongoid.yml. This allows for different configurations per environment within one file.

<span class="highlighted">YAML files are very particular about formatting, and an extra space or tab somewhere can render the file unreadable.</span> You can always check the validity of your YAML file with the following command:

<pre class="terminal">
ruby -e 'require "yaml"; YAML.load&#95;file("/path&#95;to/manifest.yml")'
</pre>

Although you are technically able to specify any number of infrastructure combinations in your manifest file, your basic stack infrastructure is still primarily defined by the Cloud 66 stack analysis result.
For example, even though you can specify that you want a node.js server running, this will be ignored unless Cloud 66 supports that type explicitly.

<div class="notice notice-danger">
	<h3>Important</h3>
    <p>The result of your Cloud 66 analysis will override configurations specified in your manifest file.</p>
</div>

<h2 id="sample">Sample manifest.yml</h2>

This simple example shows the power of **manifest.yml** files.

{% highlight yaml %}
production: # 1. Environment type
    rails: # 2. Application type
        server: # 3. Server configurations
            unique_name: frontend
        configuration: # 4. Application specific configurations
            ruby_version: 1.9.3
{% endhighlight %}

The above manifest is only scoped to *production* stacks. Here we have specified that we want to install Ruby version 1.9.3 on the rails server, and that it should be called <i>frontend</i>.

The manifest file is divided into four broad sections (as seen above):

* [Environment type](/stack-features/manifest-files.html#environments)
* [Application type](/stack-features/manifest-files.html#apps)
* [Server configurations](/stack-features/manifest-files.html#servers)
* [Application-specific configurations](/stack-features/manifest-files.html#app-specific)

What follows is an in-depth guide into how each section can be used.

<h2 id="environments">Environment type</h2>
You can select from the following environment:

- Production
- Development
- Staging
- QA

<h2 id="apps">Application type</h2>
Cloud 66 currently recognizes the following application types in your manifest file:

- <a href="#rails">Rails</a>
- <a href="#mysql">MySQL</a>(MISSING)
- <a href="#psql">PostgreSQL</a>
- <a href="#mongo">MongoDB</a>(MISSING)
- <a href="#redis">Redis</a>
- <a href="#memcache">Memcached</a>
- <a href="#haproxy">HAProxy</a>
- <a href="#postgis">PostGIS</a>

<h2 id="servers">Server type</h2>
Every application defined in the manifest file must be bound to a server. Servers can be deployed specifically to host that application, [be shared between multiple applications](/stack-features/manifest-files.html#shared) (eg. Rails and MySQL on the same server) or be an [external server](/stack-features/manifest-files.html#external) (eg. using an external database).

Here is an example of a server definition:
<pre class="terminal">
... server:
        unique&#95;name: frontend
</pre>

These are the parameters that the <i>server</i> section can take:

**unique_name**
(_Required_)

A unique name for this server.

**extra_packages**
(_Optional_)

A list of extra apt packages to be installed on the server, before deploying the application. This example installs `chrony` apt package on the server before deploying the application.

{% highlight ruby %}
... server:
        unique_name: frontend
        extra_packages:
                - chrony
{% endhighlight %}

**type**
(_Optional_)

<i>Bring your own server</i> or <i>Bring your own cloud</i>. Valid values:

- BYOS
- BYOC

**vendor**
(_Optional, BYOC Only_)

Cloud vendor to fire up the server on. Valid values:

- aws
- rackspace
- digitalocean
- joyent
- linode
- telefonica

{% highlight yaml %}
... server:
        unique_name: frontend
        type: BYOC
        vendor: aws
        region: us-east-1
        size: t1.micro
{% endhighlight %}

<div class="notice notice-danger">
	<h3>Important</h3>
    <p>Only a single cloud vendor and region is supported amongst servers within a stack.</p>
</div>


**region**
(_Optional, BYOC Only_)

[Data center region](/api/instance-regions.html) to fire up the server in.

**size**
(_Optional, BYOC Only_)

[Size of the server instance](/api/instance-names.html) created.

**address**
(_Optional, BYOS Only_)

Address of the server. For BYOS servers, <i>address</i>, <i>username</i> and <i>ssh_key_name</i> can be defined:

{% highlight yaml %}
... server:
        unique_name: frontend
        type: BYOS
        address: 123.123.123.123
        username: ubuntu
        ssh_key_name: my_server_key
{% endhighlight %}

<div class="notice notice-danger">
        <h3>Important</h3>
        <p>In order to use your chosen ssh_key_name for BYOS mode, your SSH key must already be associated with your Cloud 66 account.</p>
        <p>Only one username and ssh key is currently supported amongst servers within a stack.</p>
</div>

**username**
(_Optional, BYOS Only_)

Username for the server. This is only applicable to Bring Your Own Server setup and should have be a sudoer root user on the box.

**ssh_key_name**
(_Optional, BYOS Only_)

Name of the SSH key used to access the server. You can add this SSH key via Cloud 66 web UI.

<h3 id="shared">Shared Servers</h3>

You can share a server between two applications. This could be in cases like using the same server for both your Rails app and the MySQL server behind it.

Each shared server definition specifies the name of another server definition in the manifest file for which the applications will then share the physical server:

{% highlight yaml %}
... shared_server: *another_existing_servers_unique_name*
{% endhighlight %}

<h3 id="external">External Servers</h3>

If you would like to use an external server for an application (like using your own MySQL or AWS RDS for example), you can define that server as external.

External server definitions specify that the application is hosted on a server external to Cloud 66. This is not a valid target for your main application (ie. Rails) but may be appropriate for another application type (ie. MongoDB):

<pre class="terminal">
... server: external
</pre>

<h2 id="app-specific">Application Type Section</h2>

<div class="notice">
        <h3>Important</h3>
        <p>You are <b>required</b> to specify a <a href="/stack-features/manifest-files.html#servers">server</a> for application types, whereas configurations are <b>optional</b>.</p>
</div>

<hr>

<h3 id="rails">Rails</h3>
A Rails application type in the manifest file gives you fine control over things like the Ruby version or the server the rails application is deployed on.

- <b>ruby&#95;version</b><br/>
Specify the version of Ruby to use (overridden if present in Gemfile)
- <b>use&#95;asset&#95;pipeline</b><br/>
Specify whether to use asset pipeline compilation
- <b>do&#95;initial&#95;db&#95;schema&#95;load</b><br/>
Specify whether to perform "rake db:schema:load" on new builds
- <b>reserved&#95;server&#95;memory</b><br/>
A value in MB that Cloud 66 will assume should be left available. This will affect any automatically calculated values
- <b>passenger&#95;process&#95;memory</b><br/>
A value in MB that Cloud 66 will use for each passenger process when calculating the passenger&#95;max&#95;pool&#95;size (Passenger-based stacks only)
- <b>nginx</b><br/>
Specify configurations for Nginx, eg. CORS and [Perfect Forward Secrecy](http://en.wikipedia.org/wiki/Perfect_forward_secrecy)

<pre class="terminal">
... rails:
        server: ...
        configuration:
            ruby&#95;version: 1.9.3
            use&#95;asset&#95;pipeline: true
            do&#95;initialdb&#95;schema&#95;load: false
            reserved&#95;server&#95;memory: 0 (default value)
            passenger&#95;process&#95;memory: 200 (default value)
            nginx:
            	cors: true
            	perfect&#95;forward&#95;secrecy: true
</pre>

#### CORS configuration

If you want to, you can also specify the origin and methods for CORS.
<pre class="terminal">
... rails:
        server: ...
        configuration:
            nginx:
            	cors:
            		origin: '*'
                	methods: 'GET, OPTION'
</pre>

<hr>

<h3 id="postgresql">PostgreSQL</h3>

- **version**<br/>
Specify the version of PostgreSQL you want to install (does not apply to external servers types - see below)
- **postgis**<br/>
Specify whether to include PostGIS (Note: unlike the PG version, this can be added after initial database creation)

<pre class="terminal">
... postgresql:
        server: ...
        configuration:
            version: 9.2.3
            postgis: true
</pre>

#### PostGIS version configuration

- **version**<br/>
Specify the version of PostGIS and GEOS you want to install

   <pre class="terminal">
   production:
       postgresql:
           configuration:
               postgis:
                   version: 2.0.3
               geos:
                   version: 3.3.8
   </pre>

<hr>

<h3 id="redis">Redis</h3>

- **version**<br/>
Specify the version of Redis you want to install (does not apply to external servers types - see below)

<pre class="terminal">
... redis:
        server: ...
        configuration:
            version: 2.6.10
</pre>

<hr>

<h3 id="memcached">Memcached</h3>

#### shared&#95;group:

You can use shared&#95;group to configure where your memcached server should be deployed (if it is used in your code).
By default memcached will be deployed on your web servers. However, you can set these values under "shared&#95;group" to change this behavior:

- web
- db
- redis

This will move the memcached deployment to any of these server groups.

#### configuration:
You can use the manifest file to make small configuration changes to the Memcached server deployed by cloud66.

- **memory**<br/>
Specify maximum memory(in MB) that can be used. Default value is 64
- **port**<br/>
Specify connection port. Default value is 11211
- **listen&#95;ip**<br/>
Specify which IP address to listen on. Default value is 0.0.0.0

<pre class="terminal">
... memcached:
        shared&#95;group: db
        configuration:
            memory: 1024
            port: 11215
            listen&#95;ip: 127.0.0.1
</pre>

<hr>

<h3 id="haproxy">HAProxy</h3>
You can use the manifest file to make small configuration changes to the HAProxy load balancer deployed by Cloud 66. Currently they are limited to the following options:

- **httpchk**<br/>
The health-check configuration
- **balance**<br/>
The load balancing strategy
- **errorfile&#95;\*** <br/>
Location of your own custom error page content to serve in the case of receiving a HTTP error code on the load balancer

Note: To find out about the available options for each one of the values, please refer to [HAProxy manual](http://haproxy.1wt.eu/download/1.3/doc/configuration.txt).

<pre class="terminal">
haproxy:
    configuration:
        httpchk: HEAD / HTTP/1.0 (default value)
        balance: roundrobin (default value)
        errorfile&#95;400: /etc/haproxy/errors/400.http
        errorfile&#95;403: /etc/haproxy/errors/403.http
        errorfile&#95;408: /etc/haproxy/errors/408.http
        errorfile&#95;500: /etc/haproxy/errors/500.http
        errorfile&#95;502: /etc/haproxy/errors/502.http
        errorfile&#95;503: /etc/haproxy/errors/503.http
        errorfile&#95;504: /etc/haproxy/errors/504.http
</pre>