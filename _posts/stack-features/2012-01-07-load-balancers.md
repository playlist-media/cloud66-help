---
layout: post
template: two-col
title:  "Load balancers"
nav_sticky: false
date:   2094-01-28 16:27:22
categories: stack-features
lead: Adding a load balancer couldn't be easier
---

To add a load balancer, click on <i>Add a Load Balancer</i> in the Protips of the stack:

![Add Load Balancer](http://cdn.cloud66.com.s3.amazonaws.com/images/help/load_balancer_protip.png)

<div class="notice">
		<h3>Important</h3>
		<p>This feature is only available if you have deployed using a cloud vendor.</p>
</div>

Depending on which cloud provider you use, this load balancer will be set up differently:

- **Amazon AWS**: [Elastic Load Balancing](http://aws.amazon.com/elasticloadbalancing/)
- **Rackspace**: [Rackspace Load Balancing](http://www.rackspace.com/cloud/load-balancing/)
- **DigitalOcean**: [HAProxy](http://haproxy.1wt.eu/)
- **Linode**: [NodeBalancer](https://www.linode.com/nodebalancers/)
- **Joyent**: [HAProxy](http://haproxy.1wt.eu/)
- **Telefonica**: [HAProxy](http://haproxy.1wt.eu/)

The time required to set up your load balancer will depend on which cloud provider you use. Once your load balancer is set up, it will be ready to distribute the load between your web servers. <strong>All your existing web servers</strong> will automatically be added to the load balancer.

When you have a load balancer on your stack, your deployments will now take place in serial to reduce downtime. This means that each server will first be removed from the load balancer, deployed to and then added back to the load balancer in sequence.