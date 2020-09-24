---
layout: post
title: GitHub Pages With A Custom Domain
subtitle: Linking your custom domain to your GitHub Pages Website
description: Linking your custom domain to your GitHub Pages Website
date: 2020-09-24
image: /img/blog/dns-settings.jpg
hero_image: /img/blog.jpg
hero_height: is-medium
hero_darken: True
tags: website domain github-pages 
canonical_url: https://www.nickneos.com/2020/09/24/github-pages-with-a-custom-domain/
---

This website was created with [GitHub Pages](https://pages.github.com/). By default, websites created with GitHub Pages are accessed via `username.github.io`. For example this website can be accessed via [nickneos.github.io](https://nickneos.github.io). If you would like to point a custom domain you own (or plan to purchase) to your GitHub Pages, then read on.

First of all you need to own a custom domain. If you want to purchase one, I found <a href="https://www.ionos.com/domain-overview?ac=OM.US.USf11K357089T7073a&kwk=692957808" target="_blank">IONOS</a> to be the cheapest, and is what I used to purchase this domain.

With your custom domain you can direct both the `www` subdomain to GitHub Pages; and your apex domain to GitHub Pages. The apex domain is basically the root level domain. 

For example `nickneos.com` is my apex domain, and `www.nickneos.com` is my `www` subdomain. I could have other subdomains if I wanted as well, such as `blog.nickneos.com`, `shopping.nickneos.com`, etc. However I just use the apex domain and `www` subdomain for my GitHub Pages site.

So to set all this up, login to your account on your domain provider's website and access your domain's DNS settings.

Create a `CNAME` record as below:

* `CNAME` record for `www` pointing to `username.github.io` (replace username with your GitHub username)

Create 4 separate `A` records as below (Note that the IP addresses below were up to date as of September 24 2020. If these ever change you can obtain from [here](https://docs.github.com/en/github/working-with-github-pages/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain).):

* `A` record for `@` pointing to `185.199.108.153`
* `A` record for `@` pointing to `185.199.109.153`
* `A` record for `@` pointing to `185.199.110.153`
* `A` record for `@` pointing to `185.199.111.153` 

Save all the changes. Below is a screenshot of how this will look in the DNS settings if your domain is with <a href="https://www.ionos.com/domain-overview?ac=OM.US.USf11K357089T7073a&kwk=692957808" target="_blank">IONOS</a>.

![DNS Settings](/img/blog/dns-settings.jpg)

Now go over to your GitHub repository for your page and click the settings button for the repository. Scroll down to the GitHub pages section and enter either your www or apex domain as your custom domain depending which you want to be the default. For example, if you set it to `www.nickneos.com` and visit `nickneos.com`, it will take you to `www.nickneos.com`.

Press save. The Enforce HTTPS button won't be clickable at first. But give it a few minutes then refresh the page and you should be able to tick the Enforce HTTPS button which is recommended.

![GitHub Pages Settings](/img/blog/github-pages-settings.jpg)

To test this out, type in your apex or `www` subdomain into your browser's address bar, and it should take you to your GitHub Pages website!
