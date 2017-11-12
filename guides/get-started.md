# Get started

This guide will walk you through creating your first project with Simpla, using Github as a backend and [Netlify](https://netlify.com) for authentication.

## Create Github repo

Open a [Github](https://github.com) account if you don’t already have one. Either create a new repository or pick an existing repo you’d like Simpla to use (all data will be stored in a `_content` folder).

> **Note:** Simpla doesn’t currently support bare repos. If you are creating a new repo, initialise it with any file before connecting it to Simpla

## Register OAuth app

Register a new OAuth application for Simpla in your Github account using the button below, with the following settings:

| Setting | Description |
| --- | --- |
| Name | Name of your website |
| Homepage URL | URL of your website |
| Authorization callback URL | Set to `https://api.netlify.com/auth/done` for Netlify auth adapter (below) |

<p style="display:inline-block; width:100%;">
  <a href="https://github.com/settings/applications/new" class="simpla-button">register app</a>
</p>

> **Note:** you can also register OAuth apps on a Github organization account, see [this guide](https://developer.github.com/apps/building-integrations/setting-up-and-registering-oauth-apps/registering-oauth-apps/)

## Setup auth provider

Simpla provides [adapters](/guides/adapters) for authenticating users with Github, and this guide uses the [Netlify](https://www.netlify.com) adapter.

Setup Netlify for OAuth:

1.  Take note of the **Client ID** and **Client Secret** of the Github OAuth app you created in the previous step
2.  Open an account on Netlify, and create a free site from your Github repo
3.  In your Netlify site, navigate to **Settings** > **Access control** > **OAuth**
4.  Under **Authentication Providers**, click **Install Provider**
5.  Select **GitHub** and enter the **Client ID** and **Client Secret**, then save

## Setup Simpla

Include this snippet in the `<head>` of your page, and update the configuration settings in `Simpla.init` to match your site

```html
<!-- Include Simpla, Netlify auth adapter, and cross-browser polyfill-->
<script src="https://unpkg.com/simpla@^3.0.0"></script>
<script src="https://unpkg.com/simpla@^3.0.0/adapters/netlify.js"></script>
<script src="https://unpkg.com/webcomponents.js@^0.7.24/webcomponents-lite.min.js" async></script>

<!-- Init Simpla -->
<script>
  Simpla.init({

    // The Github repo you want Simpla to use
    repo: 'username/repo',

    // Netlify auth adapter, initialize with your Netlify site name
    auth: new SimplaNetlify({ site: 'mysite' }),

    // Public content source (optional)
    // Defaults to fetching directly from Github
    source: 'https://mysite.netlify.com'

  });
</script>
```

You can also import Simpla as an ES6/UMD module, and set additional options in `init`. Read more about [Simpla.js](/guides/simpla-js)

> **Pro tip:** If you’re storing your content alongside the rest of your site, set `source` to `window.location.origin` so you can read content from `localhost` in development

## Get elements

Simpla uses new HTML elements to manage content, install the ones you need with [Bower](https://bower.io) (NPM support coming soon). For example:

```sh
bower i simpla-text simpla-img simpla-admin --save
```

Add elements to your page with HTML imports in your `<head>`

```html
<!-- Import Simpla elements -->
<link rel="import" href="/bower_components/simpla-text/simpla-text.html">
<link rel="import" href="/bower_components/simpla-img/simpla-img.html">
<link rel="import" href="/bower_components/simpla-admin/simpla-admin.html" async>
```

Then use them wherever you want editable content. Give each element a unique `path`, which determines your content model.

```html
<!-- Block of editable richtext -->
<simpla-text path="/text"></simpla-text>

<!-- An editable image -->
<img is="simpla-img" path="/section/img">
```

> Each elements has its own documentation, find elements in the [element catalogue](https://www.webcomponents.org/collection/simplaio/simpla-elements)

## Publish and save

Make sure you have the included `simpla-admin` element, then upload your site to a static server (eg: Netlify, GitHub Pages) or run one locally.

Add `#edit` to the end of your page’s URL, and login with your Simpla account when prompted. Add content to the elements you used, then press save.

Hey presto! You just built and published your first project with Simpla.
