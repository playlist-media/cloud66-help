---
layout: post
template: two-col
title:  "Load balancer"
date:   2060-09-24 10:51:22
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
categories: add-ins
lead: Adding a load balancer couldn't be easier
tags: ['lb', 'load balancer']
---

To add a load balancer, access the add-ins menu of your stack:

![Tja](http://cdn.cloud66.com/images/help/addin_lb.png)

<div class="notice">
		<h3>Important</h3>
		<p>This feature is only available if you have deployed using a cloud vendor.</p>
</div>

Depending on which cloud provider you use, this load balancer will be set up differently:

- **Amazon AWS**: [Elastic Load Balancing](http://aws.amazon.com/elasticloadbalancing/)
- **DigitalOcean**: [HAProxy](http://haproxy.1wt.eu/)
- **Google Cloud Engine**: [Forwarding rules, target pools & health checks](https://developers.google.com/compute/docs/load-balancing/)
- **Joyent**: [HAProxy](http://haproxy.1wt.eu/)
- **Linode**: [NodeBalancer](https://www.linode.com/nodebalancers/)
- **Rackspace**: [Rackspace Load Balancing](http://www.rackspace.com/cloud/load-balancing/)
- **Telefonica**: [HAProxy](http://haproxy.1wt.eu/)
- **Vexxhost**: [HAProxy](http://haproxy.1wt.eu/)

The time required to set up your load balancer will depend on which cloud provider you use. Once your load balancer is set up, it will be ready to distribute the load between your web servers. <strong>All your existing web servers</strong> will automatically be added to the load balancer.

When you have a load balancer on your stack, your deployments can take place in serial to reduce downtime, or in [parallel](/stack-features/parallel-deployment.html). Deploying in serial involves removing each server from the load balancer, deploying to it and then re-adding it to the load balancer in sequence.