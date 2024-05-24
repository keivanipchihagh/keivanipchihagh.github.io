---
layout: post
title: How Cloudflare’s Workers Help You Bypass Blockages
description: Navigating ISP Blockage - How Cloudflare’s Workers Help You Bypass Blockages
date: 2024-04-12 07:15 +0330
# modified: 2024-04-18 07:15 +0330
description: 
tag:
  - tips
  - technology
  - security
  - vpn
  - networking
  - cloudflare
toc: true
---

There’s nothing more frustrating than finding out that your domain is occasionally blocked by Internet Service Providers (ISPs) for no apparent reason. In this article, I’ll share my personal experience and how Cloudflare Workers came to the rescue.

## Understanding ISP Blockage
ISPs may block websites due to various reasons, including legal requirements, content filtering, regional restrictions, or perhaps just a digital whim. But here’s the twist: innocent websites like mine can get caught in the crossfire. Suddenly, functionality stumbles, performance wavers, and I’m left scratching my head in the darkness. 😕

## The Frustration of Blocked Access
Imagine the shock of realizing that your website is inaccessible to certain when you desperately need it. It’s like having a physical store with a *“Closed”* sign during peak business hours. It’s a real bummer!

I’ve set up my personal cloud workstation on a remote server. It houses everything from [Prometheus](https://prometheus.io/) and [Grafana](https://grafana.com/) for monitoring my cluster and services to [pgAdmin](https://www.pgadmin.org/) for database management. This server isn’t just a convenience; it’s my lifeline for monitoring my Kubernetes cluster’s health and performance.

Yet, it gets blocked at times. The digital barricades rise, leaving me stranded outside my own virtual storefront. Why? Who knows! But I refuse to accept defeat ⚔️. Enter Cloudflare Workers — the heroes that swoop in when ISPs play gatekeepers.

## The Battle Against ISP Blockage
Assuming you already have your domain set up on Cloudflare, let’s begin setting up your subdomain:

### 1. Deploy your worker
Head to your Cloudflare panel's *“Workers and Pages”* section and hit the *“Create application”* button. Create a worker and give it an awesome name (For example, `my-awesome-site`). Then, copy and paste the following code and replace the placeholders with your domain. Finally, hit the deploy button. 🚀

```js
HOSTNAME = "my-awesome-site.com"
PROTOCOL = "https"
INBOUND_PORT = 443;

addEventListener(
    "fetch", event => {
        let url = new URL(event.request.url);

        url.hostname = HOSTNAME;
        url.protocol = PROTOCOL;
        url.port = INBOUND_PORT;

        event.respondWith(
            fetch(new Request(url, event.request))
        )
    }
)
```

The code above turns your worker into a simple HTTPS router.

### 2. Giving your worker an address
Your worker already has an address, but it’s an ugly one and not very memorable. To solve this, head to the *“Custom Domains”* of your worker settings and create a new one pointing to your worker. Wait a few minutes for it to create the certificates and set the DNS settings and Voilà!

**Pro tip**: I usually address my workers with a subdomain. If my actual domain is `my-awesome-site.com`, I’ll address my worker as `cfw.my-awesome-site.com` which makes it easy to remember.

## Benefits of Using Cloudflare Workers
- Speed: Cloudflare’s global network ensures faster response times. It will reduce latency, even when from distant locations.
- Reliability: Cloudflare’s infrastructure is redundant and scalable. No more worrying about downtime due to ISPs.
- Security: Workers operate at the edge, providing DDoS protection and SSL/TLS encryption.
- IP protection: Cloudflare workers hide your server IP address.

## Real-Life Success Story
After implementing Cloudflare Workers, my website’s accessibility improved. I simply switch to my secondary subdomain whenever my original domain isn’t doesn’t load. ISPs can still (*technically*) block Cloudflare workers, but that’s very unlikely as many websites depend on it.

## Conclusion
Don’t let ISP blockage hinder your online presence. Cloudflare Workers empower website owners to take control. Happy browsing! 🌐🚀
