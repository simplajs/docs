# Performance tips

Simpla cares about performance. The ecosystem is built on [Web Components](https://www.webcomponents.org/introduction), which means the web platform does the heavy lifting and Simpla enjoys native-like performance in places where traditional frameworks have to load, parse, and execute large amounts of JavaScript.

For example, this is a first-paint comparison of an example app built by [Vaadin](https://vaadin.com) in both Angular 2 and Polymer (a popular Web Components library):

![Performance comparison](/assets/img/perf-comparison.gif)

But there are still guidelines you can follow to optimize your Simpla app for production.

## Push content to a CDN

Simpla stores content as flat-file JSON in a Github repo. By default Simpla will read directly from Github when loading saved content, which may be useful for development, but will result in major performance bottlenecks. For this reason we **strongly recommend** pushing your content to a CDN or static-file host that supports HTTP/2 (see below) for production.

To configure Simpla to read from a custom endpoint, update the `source` option in its init options

```js
Simpla.init({
  // Public URL of where your content has been pushed
  source: 'https://mysite.com',
  ...
});
```

Your CDN will need a webhook to update when a commit is made to your Github repo (many CDNs have this built in). Alternatively, hook your repo up to a CI (Continuous Integration) service like Travis and have that push to your CDN on every commit.

> **Pro tip:** store content alongside the rest of your site if it’s already on Github, and use a buildstep to move content to your public directory for a streamlined workflow

## Use HTTP/2

HTTP/2 is a new protocol for the web that replaces the long standing HTTP/1.1 protocol, which introduces (among many other things) request multiplexing.

While requests under HTTP/1.1 are made sequentially in small batches, HTTP/2 requests happen concurrently. This means that many small requests are more performant than a single large request, which is why Simpla elements each make their own individual request for content.

Because of this we **highly recommend** that the CDN you push your Simpla content to (see above) supports HTTP/2, otherwise you could still experience a significant bottleneck while the browser processes extra requests.

The HTML imports for Simpla’s elements also add requests, so if your codebase is in a separate repo to your content we again recommend serving it from a HTTP/2 capable server/CDN. Alternatively, you can bundle Simpla’s HTML imports using Google’s [polymer-bunder](https://github.com/Polymer/polymer-bundler) tool.

Many CDNs and static file hosts come with HTTP/2 enabled out of the box. These are some of our favourites:

*   [Netlify](https://netlify.com) is a static-site host with an exceptional HTTP/2-enabled CDN baked in. They have a generous free plan that suits most projects. [simpla.io](http://simpla.io) itself is hosted on Netlify
*   [Cloudflare](https://cloudflare.com) is a popular DNS and CDN provider, who also have HTTP/2 enabled by default
*   [Google Cloud](https://cloud.google.com)’s compute and app engine support HTTP/2, if your app needs more complex server-side logic
*   Most HTTP servers (Express, Nginx, etc) can support HTTP/2 out of the box

## Import as needed

HTML imports deduplicate and cache resources locally. You can import an element multiple times, and only a single request is made.

Because of this you should only import Simpla’s elements where you use them, rather than importing everything at once in a master `index.html` document. This is esepcially relevant for SPAs (Single Page Apps). If a given view only uses `simpla-text`, then don’t import `simpla-img`. If multiple pages import and use `simpla-text`, duplicating those imports won’t matter.