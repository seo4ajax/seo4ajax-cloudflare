# Integration of SEO4Ajax in Cloudflare


[SEO4Ajax](https://www.seo4ajax.com) is a service that allows AJAX websites
(e.g. based on React, Vue.js, Svelte, Angular, Backbone, Ember, jQuery etc.) to
be indexable by search engines and social networks.

This file describes how to integrate SEO4Ajax in a [Cloudflare Worker](https://workers.cloudflare.com/).

## Instructions

1. Select your website in the console of Clouflare and select `Workers > Manage workers > Create a worker`.

2. Copy the code below and paste it in the code editor.

```js
const TOKEN = "<your site token in SEO4Ajax>";
const USER_AGENT_TEST = /google|bot|spider|pinterest|crawler|archiver|flipboardproxy|mediapartners|facebookexternalhit|insights|quora|whatsapp|slurp/i;
const FILENAME_EXTENSION_TEST = /\.[^.]+$/; 
const USER_AGENT_HEADER = "user-agent";
const API_URL = "https://api.seo4ajax.com/";

addEventListener("fetch", ({ request, respondWith }) => {
  const userAgent = request.headers.get(USER_AGENT_HEADER);
  if (userAgent && USER_AGENT_TEST.test(userAgent)) {
    const { pathname, search } = new URL(request.url);
    if (!FILENAME_EXTENSION_TEST.test(pathname)) {
      respondWith(fetch(API_URL + TOKEN + pathname + search));
    }
  }
});
```

3. Replace `TOKEN` with the value provided in the site settings page in the console of SEO4Ajax.

4. Click on `Save and Deploy`.

5. Go to the `Workers` page in the console of Cloudflare and click on `Add route`.

6. Set the `route` value to `example.com/*` where `example.com` is the domain name of your website.

7. Click on `Save`.
