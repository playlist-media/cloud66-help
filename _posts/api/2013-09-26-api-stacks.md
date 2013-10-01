---
layout: post
title:  "Stacks API"
date:   2013-09-24 10:51:22
categories: API
---

<p class="lead">List Stacks and trigger deployments via the API</p>

## Listing Stacks
#### Endpoint
<p><kbd>/stacks.json</kbd></p>
#### Method
GET
#### Required Scope
public
#### Description
Lists all the stacks for the user
### Results
JSON array of all stacks belonging to the user.
Each item in the array contains the following attributes:
<table class="table table-bordered table-striped">
	<thead>
		<tr>
			<th>Attribute</th>
			<th>Value</th>
			<th>Comments</th>
		</tr>
  </thead>
	<tbody>
		<tr><td>uid</td><td>Stack Unique Id</td><td></td></tr>
		<tr><td>name</td><td>Stack Name</td><td></td></tr>
		<tr><td>git</td><td>Git repository path</td><td></td></tr>
		<tr><td>git_branch</td><td>Git repository branch</td><td></td></tr>
		<tr><td>environment</td><td>Running environment</td><td>development, staging, production</td></tr>
		<tr><td>cloud</td><td>Used Cloud</td><td>AWS, Joyent, Linode, Telefonica, Rackspace, DigitalOcean or no_cloud</td></tr>
		<tr><td>fqdn</td><td>Full URL for the Stack</td><td></td></tr>
		<tr><td>language</td><td>Stack language</td><td>ruby</td></tr>
		<tr><td>framework</td><td>Stack framework</td><td>rails</td></tr>
		<tr><td>status</td><td>Stack status</td><td>See <a href='stack_status'>Stack Status</a></td></tr>
		<tr><td>health</td><td>Stack health</td><td>See <a href='stack_health'>Stack Health</a></td></tr>
		<tr><td>redeployment_hook</td><td>Redeployment hook URL</td><td>See <a href='redeployment_hook'>Redeployment Hook</a>. This is only available with redeploy scope.</td></tr>
		<tr><td>last_activity</td><td>Last Activity</td><td>Date Time in UTC</td></tr>
		<tr><td>created_at</td><td>Record created at</td><td>Date Time in UTC</td></tr>
		<tr><td>updated_at</td><td>Record updated at</td><td>Date Time in UTC</td></tr>
	</tbody>
</table>
#### Example
<pre class="terminal">
{[
	"uid" : "46ae3ee56f94ff4b2be8c887f8a6d343",
	"name" : "My Awesome Stack",
	"git" : "git://github.com/khash/stacks-test.git",
	"git_branch" : "master",
	"environment" : "development",
	"cloud" : "AWS",
	"fqdn" : "my-awesome-stack.development.dev.c66.me",
	"language" : "ruby",
	"framework" : "rails",
	"status" : 2,
	"health" : 2,
	"redeploy_hook" : "http://www.cloud66.com/hooks/v1/stacks/redeploy/9aee67fec10cdcbbbc46eebd41234cb9/46ae3ee56f94ff4b2be8c887f8a6d343",
	"last_activity" : "2013-01-25T14:13:18+00:00",
	"updated_at" : "2013-01-25T14:13:18+00:00",
	"created_at" : "2013-01-25T14:13:18+00:00"
]}
</pre>

## Show Stack Details
#### Endpoint
<p><kbd>/stacks/[stack_uid].json</kbd></p>
#### Method
GET
#### Required Scope
public
#### Description
Returns the details of a single stack
### Results
JSON object of the stack:
<table class="table table-bordered table-striped">
	<thead>
		<tr>
			<th>Attribute</th>
			<th>Value</th>
			<th>Comments</th>
		</tr>
  </thead>
	<tbody>
		<tr><td>uid</td><td>Stack Unique Id</td><td></td></tr>
		<tr><td>name</td><td>Stack Name</td><td></td></tr>
		<tr><td>git</td><td>Git repository path</td><td></td></tr>
		<tr><td>git_branch</td><td>Git repository branch</td><td></td></tr>
		<tr><td>environment</td><td>Running environment</td><td>development, staging, production</td></tr>
		<tr><td>cloud</td><td>Used Cloud</td><td>AWS, Joyent, Linode, Telefonica, Rackspace, DigitalOcean or no_cloud</td></tr>
		<tr><td>fqdn</td><td>Full URL for the Stack</td><td></td></tr>
		<tr><td>language</td><td>Stack language</td><td>ruby</td></tr>
		<tr><td>framework</td><td>Stack framework</td><td>rails</td></tr>
		<tr><td>status</td><td>Stack status</td><td>See <a href='stack_status'>Stack Status</a></td></tr>
		<tr><td>health</td><td>Stack health</td><td>See <a href='stack_health'>Stack Health</a></td></tr>
		<tr><td>redeployment_hook</td><td>Redeployment hook URL</td><td>See <a href='redeployment_hook'>Redeployment Hook</a>. This is only available with redeploy scope.</td></tr>
		<tr><td>last_activity</td><td>Last Activity</td><td>Date Time in UTC</td></tr>
		<tr><td>created_at</td><td>Record created at</td><td>Date Time in UTC</td></tr>
		<tr><td>updated_at</td><td>Record updated at</td><td>Date Time in UTC</td></tr>
	</tbody>
</table>
#### Example
<pre class="terminal">
{
	"uid" : "46ae3ee56f94ff4b2be8c887f8a6d343",
	"name" : "My Awesome Stack",
	"git" : "git://github.com/khash/stacks-test.git",
	"git_branch" : "master",
	"environment" : "development",
	"cloud" : "AWS",
	"fqdn" : "my-awesome-stack.development.dev.c66.me",
	"language" : "ruby",
	"framework" : "rails",
	"status" : 2,
	"health" : 2,
	"redeploy_hook" : "http://www.cloud66.com/hooks/v1/stacks/redeploy/9aee67fec10cdcbbbc46eebd41234cb9/46ae3ee56f94ff4b2be8c887f8a6d343",
	"last_activity" : "2013-01-25T14:13:18+00:00",
	"updated_at" : "2013-01-25T14:13:18+00:00",
	"created_at" : "2013-01-25T14:13:18+00:00"
}
</pre>

#### Error
<pre class="terminal">
bad_request - no stack UID provided
not_found - invalid stack UID provided
</pre>

## Trigger Deployment
#### Endpoint
<p><kbd>/stacks/(:stack_uid)/redeploy.json</kbd></p>
#### Method
POST
#### Required Scope
redeploy
#### Description
Starts the redeployment of a stack on user's behalf
### Results
#### Success
<pre class="terminal">
{ "ok" : true,  "message" : "Stack queued for redeployment" }
</pre>

#### Error
<pre class="terminal">
bad_request - no stack UID provided
not_found - invalid stack UID provided
</pre>