---
layout: post
template: one-col
title:  "Manifest files"
so_title: "manifest"
nav_sticky: false
date:   2092-01-25 16:27:22
categories: stack-definition
lead: You can be more explicit about your stack composition
search-tags: []
tags: ['Deployment']
---

<h2>Contents</h2>                                                                           
<ul class="page-toc">
	<li><a href="#intro">What are manifest files?</a></li>
         
    <li><a href="#composition">Composition of a manifest file</a></li>   
        <li><ul>
            <li><a href="#sample">Sample manifest file</a></li>
            <li><a href="#environments">Environments</a></li>   
            <li><a href="#apps">Application types</a></li>
            <li><a href="#servers">Server configurations</a></li>
            <li>
                <ul><li><a href="#shared">Shared</a></li></ul>
            </li>
            <li>
                <ul><li><a href="#external">External</a></li></ul>
            </li>
    <li><a href="#app-specific">Application specific</a></li>
            <li><ul><li><a href="#elastic">ElasticSearch</a></li></ul></li>
            <li><ul><li><a href="#memcached">Memcached</a></li></ul></li>
            <li><ul><li><a href="#mongo">MongoDB</a></li></ul></li>
            <li><ul><li><a href="#postgis">PostGIS</a></li></ul></li>
            <li><ul><li><a href="#postgresql">PostgreSQL</a></li></ul></li>
            <li><ul><li><a href="#rails">Rails</a></li></ul></li>
            <li><ul><li><a href="#redis">Redis</a></li></ul>
            <li><ul><li><a href="#sinatra">Sinatra</a></li></ul>
        </li>
    <li><a href="#loadbalancer">Load balancers</a></li>
            <li><ul><li><a href="#aws_elb">AWS load balancer</a></li></ul></li>
            <li><ul><li><a href="#gce_lb">GCE load balancer</a></li></ul></li>
            <li><ul><li><a href="#haproxy">HAProxy</a></li></ul></li>            
            <li><ul><li><a href="#nodebalancer">Linode Nodebalancer</a></li></ul></li>
            <li><ul><li><a href="#rs_loadbalancer">Rackspace load balancer</a></li></ul></li>
        </li>
    </ul>
    <li><a href="#environment_variables">Environment variables in the manifest</a></li> 
</li>    
</ul>

<h2 id="intro">What are manifest files?</h2>

A manifest file allows you to be more explicit about your stack composition by specifying additional packages you wish to install, server sizes/regions and other options. <b>Unless otherwise stated, manifest file specifications are only applied during the initial build of your stack.</b>

To use this functionality, you need to place a file called **manifest.yml** in a folder named **.cloud66**, that is in turn located in the root of your source code and checked into your repository.

<pre class="terminal">
[source&#95;repo]/.cloud66/manifest.yml
</pre>

The manifest.yml file is **YAML** formatted and is split by environment just like database.yml or mongoid.yml. This allows for different configurations per environment within one file. YAML files are very particular about formatting, and an extra space or tab somewhere can render the file unreadable. You can use <a href="http://yamllint.com/" target="_blank">Yamllint.com</a> to check your YAML syntax before committing.

Although you are technically able to specify any number of infrastructure combinations in your manifest file, your basic stack infrastructure is still primarily defined by the Cloud 66 stack analysis result. For example, even though you can specify that you want a node.js server running, this will be ignored unless Cloud 66 supports that type explicitly.

<div class="notice notice-danger">
	<h3>Important</h3>
    <p>The result of your Cloud 66 analysis will override configurations specified in your manifest file.</p>
</div>

<h2 id="composition">Composition of a manifest file</h2>

<h3 id="sample">Sample manifest file</h3>

This simple example shows the power of **manifest.yml** files.

{% highlight yaml %}
production: # 1. Environment type
    rails: # 2. Application type
        servers:
            server: # 3. Server type (optional)
                unique_name: frontend
        configuration: # 4. Application specific configurations
            ruby_version: 1.9.3
{% endhighlight %}

The above manifest is only scoped to *production* stacks. Here we have specified that we want to install Ruby version 1.9.3 on the rails server, and that it should be called <i>frontend</i>.

The manifest file is divided into four broad sections:

<ul>
<li>Environments</li>
<li>Application type</li>
<li>Server configurations</li>
<li>Application-specific configurations</li>
</ul>

What follows is an in-depth guide into how each section can be used.

<hr>

<h3 id="environments">Environments</h3>
You can select from the following environments:

- Production
- Development
- Staging
- QA

<h3 id="apps">Application type</h3>
Cloud 66 currently recognizes the following application types in your manifest file:

<ul class="page-toc">
<li><a href="#elastic">ElasticSearch</a></li>
<li><a href="#memcache">Memcached</a></li>
<li><a href="#mongo">MongoDB</a></li>
<li><a href="#postgis">PostGIS</a></li>
<li><a href="#psql">PostgreSQL</a></li>
<li><a href="#rails">Rails</a></li>
<li><a href="#redis">Redis</a></li>
</ul>

<h3 id="servers">Server type</h3>
Every application defined in the manifest file must be bound to a server. However, if you'd like configurations to apply to all servers in an application type, you don't need to specify a server type. Servers can be deployed specifically to host that application, be shared between multiple applications (eg. Rails and MySQL on the same server) or be an external server (eg. using an external database).

Here is an example of a server definition:
{% highlight ruby %}
... server:
        unique_name: frontend
{% endhighlight %}

These are the parameters that the <i>server</i> section can take:

**unique_name**
(_Required_)

<div class="notice">
	<h3>Note</h3>
    <p>This field is only required if you are specifying a server type.</p>
</div>

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

**vendor**
(_Optional, BYOC Only_)

Cloud vendor to fire up the server on. Valid values:

- aws
- rackspace
- digitalocean
- joyent
- linode
- telefonica
- vexxhost
- googlecloud

{% highlight yaml %}
... server:
        unique_name: frontend
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

[Data center region](http://developers.cloud66.com/#cloud-vendor-instance-regions) to fire up the server in.

**size**
(_Optional, BYOC Only_)

[Size of the server instance](http://developers.cloud66.com/#cloud-provider-instance-names) created.

**address**
(_Optional, BYOS Only_)

Address of the server. For BYOS servers, <i>address</i>, <i>username</i> and <i>ssh_key_name</i> can be defined:

{% highlight yaml %}
... server:
        unique_name: frontend
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

Username for the server. This is only applicable to Bring Your Own Server setups, and you need to be a sudoer root user on the box.

**ssh_key_name**
(_Optional, BYOS Only_)

Name of the SSH key used to access the server. You can add this SSH key via Cloud 66 web UI.

<h4 id="shared">Shared Servers</h4>

You can share a server between two applications. This could be in cases like using the same server for both your Rails app and the MySQL server behind it.

Each shared server definition specifies the name of another server definition in the manifest file for which the applications will then share the physical server:

{% highlight yaml %}
... same_as: *another_existing_servers_unique_name*
{% endhighlight %}

<h4 id="external">External Servers</h4>

If you would like to use an external server for an application (like using your own MySQL or AWS RDS for example), you can define that server as external.

External server definitions specify that the application is hosted on a server external to Cloud 66. This is not a valid target for your main application (ie. Rails) but may be appropriate for another application type (ie. MongoDB):

<pre class="terminal">
... server: external
</pre>

<h3 id="app-specific">Application Type Section</h3>

<div class="notice">
        <h3>Important</h3>
        <p>You are <b>required</b> to specify a <a href="/stack-definition/manifest-files.html#servers">server</a> for application types, whereas configurations are <b>optional</b>.</p>
</div>

<hr>

<h4 id="elastic">ElasticSearch</h4>

- **version**<br/>
Specify the version of ElasticSearch you want to install (does not apply to external servers types)

<pre class="terminal">
... elasticsearch:
		servers:
		    server: ...
        configuration:
            version: 0.90.7
</pre>

<hr>

<h4 id="memcached">Memcached</h4>

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

<h4 id="mongo">MongoDB</h4>

- **version**<br/>
Specify the version of MongoDB you want to install (does not apply to external servers types)

<pre class="terminal">
... mongodb:
		servers:
		    server: ...
        configuration:
            version: 2.4.8
</pre>

<hr>

<h4 id="postgresql">PostgreSQL</h4>

- **version**<br/>
Specify the version of PostgreSQL you want to install (does not apply to external servers types)
- **postgis**<br/>
Specify whether to include PostGIS (Note: unlike the PG version, this can be added after initial database creation)

<pre class="terminal">
... postgresql:
		servers:
		    server: ...
        configuration:
           	version: 9.3.4
           	postgis: true
</pre>

<h5 id="postgis">PostGIS version configuration</h5>

- **version**<br/>
Specify the version of PostGIS you want to install

<pre class="terminal">
production:
   postgresql:
	   servers:
	       server: ...
       configuration:
           postgis:
               version: 2.1.1
</pre>

<hr>

<h4 id="rails">Rails</h4>
A Rails application type in the manifest file gives you fine control over things like the Ruby version or the server the rails application is deployed on.

- <b>ruby&#95;version</b><br/>
Specify the version of Ruby to use (overridden if present in Gemfile)
- <b>asset&#95;pipeline&#95;precompile</b><br/>
Specify whether to use asset pipeline compilation - this will be taken into account during redeployment
- <b>do&#95;initial&#95;db&#95;schema&#95;load</b><br/>
Specify whether to perform "rake db:schema:load" on new builds
- <b>reserved&#95;server&#95;memory</b><br/>
A value in MB that Cloud 66 will assume should be left available. This will affect any automatically calculated values, and will be taken into account during redeployment
- <b>passenger&#95;process&#95;memory</b><br/>
A value in MB that Cloud 66 will use for each passenger process when calculating the passenger&#95;max&#95;pool&#95;size (Passenger-based stacks only) - this will be taken into account during redeployment
- <b>activeprotect</b><br/>
Specify a whitelist of IPs that should be ignored by your ActiveProtect configuration
- <b>nginx</b><br/>
Specify configurations for Nginx, eg. CORS and [Perfect Forward Secrecy](http://en.wikipedia.org/wiki/Perfect_forward_secrecy) - this will be taken into account when your Nginx configuration is reloaded.

<pre class="terminal">
... rails:
		servers:
			server: ...
		configuration:
			ruby&#95;version: 1.9.3
			asset&#95;pipeline&#95;precompile: true
			do&#95;initial&#95;db&#95;schema&#95;load: false
			reserved&#95;server&#95;memory: 0 (default value)
			passenger&#95;process&#95;memory: 200 (default value)
			activeprotect:
                whitelist: 123.123.123.123,234.234.234.234
			nginx:
				cors: true
				perfect&#95;forward&#95;secrecy: true
</pre>

#### CORS configuration

If you want to, you can also specify the origin and methods for CORS.
<pre class="terminal">
... rails:
		servers:
		    server: ...
		configuration:
			nginx:
				cors:
					origin: '*'
					methods: 'GET, OPTION'
</pre>

<hr>

<h4 id="redis">Redis</h4>

- **version**<br/>
Specify the version of Redis you want to install (does not apply to external servers types - see below)

<pre class="terminal">
... redis:
		servers:
		    server: ...
		configuration:
			version: 2.6.10
</pre>

<hr>

<h4 id="sinatra">Sinatra</h4>
A Sinatra application type in the manifest file gives you fine control over things like the Ruby version or which server the application is deployed on.

- <b>ruby&#95;version</b><br/>
Specify the version of Ruby to use (overridden if present in Gemfile)
- <b>do&#95;initial&#95;db&#95;schema&#95;load</b><br/>
Specify whether to perform "rake db:schema:load" on new builds
- <b>reserved&#95;server&#95;memory</b><br/>
A value in MB that Cloud 66 will assume should be left available. This will affect any automatically calculated values, and will be taken into account during redeployment
- <b>passenger&#95;process&#95;memory</b><br/>
A value in MB that Cloud 66 will use for each passenger process when calculating the passenger&#95;max&#95;pool&#95;size (Passenger-based stacks only) - this will be taken into account during redeployment
- <b>activeprotect</b><br/>
Specify a whitelist of IPs that should be ignored by your ActiveProtect configuration
- <b>nginx</b><br/>
Specify configurations for Nginx, eg. CORS and [Perfect Forward Secrecy](http://en.wikipedia.org/wiki/Perfect_forward_secrecy) - this will be taken into account when your Nginx configuration is reloaded.

<pre class="terminal">
... sinatra:
        servers:
            server: ...
        configuration:
            ruby&#95;version: 1.9.3        
            do&#95;initial&#95;db&#95;schema&#95;load: false
            reserved&#95;server&#95;memory: 0 (default value)
            passenger&#95;process&#95;memory: 200 (default value)
            activeprotect:
                whitelist: 123.123.123.123,234.234.234.234
            nginx:
                cors: true
                perfect&#95;forward&#95;secrecy: true
</pre>

#### CORS configuration

If you want to, you can also specify the origin and methods for CORS.
<pre class="terminal">
... sinatra:
        servers:
            server: ...
        configuration:
            nginx:
                cors:
                    origin: '*'
                    methods: 'GET, OPTION'
</pre>

<hr>

<h3 id="loadbalancer">Load balancers</h3>

<h4 id="aws_elb">AWS load balancer</h4>
Use a manifest file to the AWS load balancer deployed by Cloud 66. These changes will only apply when you create a new load balancer.

Available settings:

- **httpchk**<br/>
The URL visited to check your server health

<pre class="terminal">
... load_balancer:
        configuration:
            httpchk: / 
</pre>

<hr>

<h4 id="gce_lb">GCE load balancer</h4>
Use a manifest file to the GCE load balancer deployed by Cloud 66. These changes will only apply when you create a new load balancer.

Available settings (refer to the [GCE documentation](https://cloud.google.com/compute/docs/load-balancing/network/target-pools) for more information):

- **httpchk**<br/>
The URL visited to check your server health

- **balance**<br/>
The load balancing strategy. You can use these values: NONE, CLIENT_IP or CLIENT_IP_PROTO.

<pre class="terminal">
... load_balancer:
        configuration:
            httpchk: / 
            balance: CLIENT_IP_PROTO 
</pre>

<hr>

<h4 id="haproxy">HAProxy</h4>
Use a manifest file to configure your HAProxy load balancer deployed by Cloud 66. These changes will be either be applied when you redeploy a stack with more than one server, rebuild HAProxy or edit [HAProxy CustomConfig](/load-balancing/haproxy.html).

Available settings (refer to the [HAProxy documentation](http://haproxy.1wt.eu/download/1.3/doc/configuration.txt) for more information):

- **httpchk**<br/>
The health-check configuration
- **balance**<br/>
The load balancing strategy
- **errorfile&#95;\*** <br/>
Location of your own custom error page content to serve in the case of receiving a HTTP error code on the load balancer

<pre class="terminal">
... load_balancer:
        configuration:
            httpchk: HEAD / HTTP/1.0 (default value)
            balance: roundrobin (default value)
            errorfile&#95;400: /etc/haproxy/errors/400.http
            errorfile&#95;403: /etc/haproxy/errors/403.http
            errorfile&#95;408: /etc/haproxy/errors/408.http
            errorfile&#95;500: /etc/haproxy/errors/500.http
            errorfile&#95;502: /etc/haproxy/errors/502.http
            errorfile&#95;503: /etc/haproxy/errors/503.http
            errorfile&#95;504: /etc/haproxy/errors/504.https
</pre>

<hr>

<h4 id="nodebalancer">Linode Nodebalancer</h4>
Use a manifest file to the Linode Nodebalancer deployed by Cloud 66. These changes will only apply when you create a new load balancer.

Available settings (refer to the [Linode documentation](https://www.linode.com/docs/platform/nodebalancer/nodebalancer-reference-guide) for more information):

- **httpchk**<br/>
The health-check configuration
- **balance**<br/>
The load balancing strategy. You can use these values : roundrobin, leastconn or source.

<pre class="terminal">
... load_balancer:
        configuration:
            httpchk: / 
            balance: leastconn 
</pre>

<hr>

<h4 id="rs_loadbalancer">Rackspace load balancer</h4>
Use a manifest file to the Rackspace load balancer deployed by Cloud 66. These changes will only apply when you create a new load balancer.

Available settings (refer to the [Rackspace documentation](http://docs.rackspace.com/loadbalancers/api/v1.0/clb-devguide/content/Algorithms-d1e4367.html) for more information):

- **balance**<br/>
The load balancing strategy. You can use these values : ROUND_ROBIN, RANDOM or LEAST_CONNECTIONS.

<pre class="terminal">
... load_balancer:
        configuration:
            balance: LEAST_CONNECTIONS 
</pre>

<hr>

<h2 id="environment_variables">Environment variables in the manifest</h3>
You can add your environment variables to your manifest files.

Here is an example:

{% highlight yaml %}
production:
    environment_variables:
        SOME_VARIABLE: value
        ANOTHER_ONE: another_value
        THIRD_ONE: AUTO_GENERATE
        LONG_ONE: AUTO_GENERATE_15
{% endhighlight %}

If you need to auto generate a value, you can use the `AUTO_GENERATE` keyword. It generates a 10 character long random string unless you specify the length after it: `AUTO_GENERATE_15` which generates a 15 character random string.

Environment variables set in your manifest file will only apply during the initial build of your stack. Please refer to our documentation on <a href="/deployment/env-vars.html">environment variables</a> if you'd like to set them beyond this point.

Any environment variable that is generated by the result of the code analysis (like database addresses) will override any value specified in the manifest file. In other words, you cannot specify a value like `MYSQL_ADDRESS` in your manifest file as it will be ignored.
