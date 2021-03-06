---
layout: post
template: two-col
title:  "Cloud 66 WebSocket support"
so_title: "websocket"
cloud66_text: "Try Cloud 66 for free"
cloud66_sticky: true
date:   1970-09-26 15:33:13
categories: 
lead: Implement WebSocket on your server
search-tags: ['']
tags: ['Web server']
tutorial: true
difficulty: 2
---


## About WebSocket

[WebSocket](http://www.websocket.org) allows bi-directional web communication between client and server and provides a true standard that can be used to build scalable, real-time web applications.

## Cloud 66 configuration for WebSocket

Cloud 66 opens **8080** and **8443** ports by default on your rails servers to allow you to use WebSocket.

If you want to use WebSocket with Cloud 66, your WebSocket servers need to listen to the following ports:

- **8080**
- **8443** for SSL

You can use a different port to use WebSocket (not supported by Cloud 66) but you will need manually open the ports to allow external connections to your Rails servers.

Find out more about [Stack networking page](http://help.cloud66.com/stack-definition/network-configuration.html).

## WebSocket through a load balancer

At any time, you can use a load balancer and scale up your servers to have more redundancy and capacity for your WebSocket servers.

<div class="notice">
        <h3>Note</h3>
        <p>ELB (Amazon) doesn't support WebSocket traffic.</p>
</div>

Learn more about [Cloud 66 HAProxy and WebSocket](/articles/configuring-haproxy-for-websocket) setup.

**Linode** alternative HTTP ports 8080 and 8443 are opened on NodeBalancers and can be used for WebSockets.

## Test your WebSocket server

To test your WebSocket server, create a <code>.html</code> file with the code below, make sure to replace *&lt;your&#95;address&gt;* with your WebSocket server IP address and finally, open it in a web browser.

<pre class="prettyprint">
&#60;html&#62;
  &#60;head&#62;
    &#60;script src="http://ajax.googleapis.com/ajax/libs/jquery/1.3.2/jquery.min.js"&#62;&#60;/script&#62;
      &#60;script&#62;
      $(document).ready(function(){
        function debug( str ) {
          $("#debug").append( str );
        };

        ws = new WebSocket("ws://your address");
        ws.onmessage = function(evt) {
          $("#msg").append("evt.data");
        };
        ws.onclose = function() {
          debug("socket closed");
        };
        ws.onopen = function() {
          debug("connected...");
          ws.send("hello server");
        };
      });
    &#60;/script&#62;
  &#60;/head&#62;
  &#60;body&#62;
    &#60;div id="debug"&#62;&#60;/div&#62;
    &#60;div id="msg"&#62;&#60;/div&#62;
  &#60;/body&#62;
&#60;/html&#62;
</pre>
