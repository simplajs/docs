# Adapters

Simpla uses additional adapters to provide pluggable, service-agnostic functionality on top of the core library. All adapters outlined in this reference are bundled with the `simpla` package on NPM.

## SimplaNetlify

```method
new SimplaNetlify(config: Object): Object
```

| Constructor parameter |  |  |
| --- | --- | --- |
| `site` | `{String}` | The name/ID of your Netlify site |

| Adapter location |
| --- |
| `/adapters/netlify.js` |

Use [Netlify](https://www.netlify.com)’s OAuth services to authenticate Github collaborators with Simpla. Instantiate it with your Netlify site name when you call `Simpla.init`.

```js
Simpla.init({
  auth: new SimplaNetlify({
    site: 'mysite'
  }),
  ...
});
```