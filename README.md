# Integration of SEO4Ajax in Cloudflare


[SEO4Ajax](https://www.seo4ajax.com) is a service that allows AJAX websites
(e.g. based on React, Vue.js, Svelte, Angular, Backbone, Ember, jQuery etc.) to
be indexable by search engines, social and ad networks.

This file describes how to integrate SEO4Ajax in a [Cloudflare Worker](https://workers.cloudflare.com/).

## Instructions

1. Go to the homepage of the Cloudflare console and select `Workers > Manage workers > Create a worker`.

2. Copy the code below and paste it in the code editor.

```js
const SITE_TOKEN = "SEO4AJAX_SITE_TOKEN";
const USER_AGENT_TEST = /google|bot|lighthouse|spider|pinterest|crawler|archiver|flipboardproxy|mediapartners|facebookexternalhit|insights|quora|whatsapp|slurp/i;
const FILENAME_EXTENSION_TEST = /\.(?!htm)[^.]+$/; 
const USER_AGENT_HEADER = "user-agent";
const API_URL = "https://api.seo4ajax.com/" + SITE_TOKEN;

addEventListener("fetch", event => {
  const { url, headers } = event.request;
  const userAgent = headers.get(USER_AGENT_HEADER);
  if (userAgent && USER_AGENT_TEST.test(userAgent)) {
    const { pathname, search } = new URL(url);
    if (!FILENAME_EXTENSION_TEST.test(pathname)) {
      event.respondWith(fetch(API_URL + pathname + search));
    }
  }
});
```

3. Replace `SEO4AJAX_SITE_TOKEN` (i.e. the value of `SITE_TOKEN`) with the token provided in the site settings page in the console of SEO4Ajax (e.g. `12918fa3c2e945aaf4625a81c93a3181`).

4. Click on `Save and Deploy`.

5. Go to the `Workers` page in the console of Cloudflare and click on `Triggers > Add route`.

6. Set the `route` value to `www.example.com/*` where `www.example.com` is the domain name of your website.

7. Click on `Save`.
