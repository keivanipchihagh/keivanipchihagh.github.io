---
layout: post
title: Deploy Your Resume Into Production!
description: Why not apply the same concept to your resume?
date: 2024-01-06 11:10 +0330
modified: 2024-01-06 11:10 +0330
tag:
  - resume
  - docs
  - production
  - cv
  - cloudflare
toc: true
---

In the tech world, “deploying into production” signifies the final step of making an application live and accessible to users. So why not apply the same concept to your resume? 🚀

By deploying your resume into production, you can make it easily accessible to recruiters and hiring managers with a simple URL. Plus, you don’t have to worry about updating all your copies every time you make a change.

In this article, I’ll show you how I deployed my resume written in [Google Docs](https://docs.google.com/) on [Cloudflare](https://www.cloudflare.com/). Although, you can use any other hosting for your resume. Here’s a step-by-step guide on how I did it:

## Step 1: Make Your Resume Accessible
First, ensure your resume in Google Docs is polished and ready for the spotlight. Then, share it with anyone with the link, but with viewer access only!

<figure>
<img src="{{ '/deploy-your-resume-into-production/docs.jpg' | relative_url }}" alt="Google Docs general access">
<figcaption>Fig 1. Google Docs general access</figcaption>
</figure>

## Step 2: Set Up a Subdomain on Cloudflare
This is where you flex your DNS muscles. Just don’t go down the rabbit hole of DNS configuration for too long — we’ve all been there.

Log in to your domain registrar and navigate to the *DNS* settings. Create a new *CNAME* record for your subdomain (e.g., `resume.example.com`) and point it to somewhere you like (it doesn’t matter where because we only need the alias). Don’t forget to set its proxy status to Proxied as shown below:

- Type: `CNAME`
- Name: `resume`
- Target: `domain.com`
- Proxy status: `Proxied`
- TTL: `Auto`

<figure>
<img src="{{ '/deploy-your-resume-into-production/cloudflare.jpg' | relative_url }}" alt="Cloudflare DNS settings snapshot">
<figcaption>Fig 2. Cloudflare DNS settings snapshot</figcaption>
</figure>

## Step 3: Create a redirect rule
Head to the “Rules” section and create a new “Redirect Rule” with the following config:

If…
- Tupe: `Custom filter expression`
- Field: `Hostname`
- Operator: `equals`
- Value: `resume.example.com(same as the one in step 2)`

Then…
- Type: `Dynamic`
- URL: `concat(“https://docs.google.com/document/d/...", http.request.uri.path) - (replace your share link)`
- Status Code = `301` (permanently moved)
- Preserve query string: `Checked`

<figure>
<img src="{{ '/deploy-your-resume-into-production/rules.jpg' | relative_url }}" alt="Cloudflare Redirect Rules">
<figcaption>Fig 3. Cloudflare Redirect Rules</figcaption>
</figure>

**What is the “Preserve query string”?** This way you can pass query parameters to your URL. This is crucial if you want to create a download link for your resume.

To view your resume: `https://resume.example.com`

To download your resume: `https://resume.example.com/export?format=pdf`

This approach not only makes your resume accessible and professional but also adds a touch of fun and creativity to the process. By following these steps, you can ensure your resume is always available and up-to-date, ready to impress potential employers. Plus, it’s a great conversation starter in interviews — who wouldn’t be impressed by a resume that’s been deployed to production?