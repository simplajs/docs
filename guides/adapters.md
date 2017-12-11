# Adapters

Simpla uses additional adapters to provide pluggable, service-agnostic functionality on top of the core library. Currently authentication is the only interface that uses adapters, in future more functionality may be split from the core into seperate adapters.

All official adapters are bundled with the `simpla` package on NPM and Unpkg, under the `adapters` folder.

## SimplaNetlify

The `SimplaNetlify` adapter uses [Netlify](https://www.netlify.com)’s OAuth services to authenticate collaborators on your Github repo with Simpla. It’s located under `/adapters/netlify.js`.

You will need to have an OAuth application for Simpla registered on your Github account (see [getting started](/guides/get-started)). Set the **Authorization callback URL** for your OAuth app to `https://api.netlify.com/auth/done`, and take note of the **Client ID** and **Client Secret**.

To setup Netlify:

1.  In your Netlify site, navigate to **Site Settings** > **Access Control** > **OAuth**
2.  Under **Authentication Providers**, click **Install Provider**
3.  Select **GitHub** and enter the **Client ID** and **Client Secret**, then save

Import the adapter and instantiate it with your Netlify site name on the `auth` property in `Simpla.init`

```js
import SimplaNetlify from 'simpla/adapters/netlify';

Simpla.init({
  auth: new SimplaNetlify({
    site: 'mysite'
  }),
  ...
});
```

You can also include the adapter as a UMD module in a `<script>` tag, exposed as `window.SimplaNetlify`

```html
<script src="/node_modules/simpla/adapters/netlify.js"></script>
```

> **Pro tip:** Linking your Github repo to Netlify automatically pushes all your content to a high-performance CDN, set `source` in `Simpla.init` to your Netlify site URL (eg: `https://[site].netlify.com`) for instant super performance
