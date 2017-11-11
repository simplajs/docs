# Simpla.js

Simpla’s core JavaScript library provides the tools for elements and developers to work with data, manage state, authenticate users, and react to changes in content.

## Simpla.init

```method
init(config: Object): undefined
```

| Config Parameter |  |  |
| --- | --- | --- |
| `repo` | `{String}` | GitHub repository where your content will be stored |
| `auth` | `{Object}` | Authentication adapter to connect users with Github |
| `[source]` | `{String}` | The public URL to access your content from, defaults to fetching directly from GitHub (optional) |
| `[branch]` | `{String}` | Git branch Simpla commits new content to, defaults to ‘master’ (optional) |
| `[public]` | `{String}` | Base directory for Simpla to store its `_content` folder in, defaults to the root of the repo (optional) |

Initialize Simpla, this must be done before any other methods are called.

```js
Simpla.init({
  repo: 'username/repo',
  auth: new SimplaNetlify({ site: 'mysite' }),
  source: window.location.origin,
  branch: 'master',
  public: 'dist'
});
```

## Simpla.login

```method
login(): Promise<undefined>
```

| Return |  |
| --- | --- |
| `Promise` | Empty promise that resolves when user has been successfully logged in |

Prompt the authentication adapter you provided in `Simpla.init` for login. For OAuth adapters (like `SimplaNetlify`), this will open a window to login to Github.

The returned token is stored in localStorage to automatically authenticate all subsequent requests, until it either expires or is removed with [`Simpla.logout`](#simplalogout).

```js
Simpla.login()
.then(() => {
  console.log('Login successful')
})
.catch((error) => {
  console.log('Login failed', error)
});
```

## Simpla.logout

```method
logout(): Promise<undefined>
```

| Return |  |
| --- | --- |
| `Promise` | Empty promise that resolves when user has been successfully logged out |

Removes the user’s token, logging them out locally from the current session.

```js
Simpla.logout().then(() => {
  console.log('User logged out');
});
```

## Simpla.get

```method
get(path: string): Promise<Object>
```

| Parameter |  |  |
| --- | --- | --- |
| `path` | `{String}` | Content path to fetch |

| Return |  |
| --- | --- |
| `Promise<Object>` | Promise with object containing data of item fetched |

Fetch the content at a given path. If data exists locally (cached from a previous request or set with [`Simpla.set`](#simplaset)) it will be returned instantly, otherwise it will be fetched remotely and then cached before resolving. This means the result may not exist in Github yet.

If the item doesn’t exist at all the returned Promise will resolve to `null`. If the given path is invalid it will reject with an error.

The returned item object will have the following properties

| Property | Type | Description |
| --- | --- | --- |
| `path` | `String` | Path to the item in your project |
| `type` | `String` | Type of content |
| `data` | `Object` | Custom data |
| `createdAt` | `String` | Timestamp of when this path was created |
| `updatedAt` | `String` | Timestamp of when this path was last modified |

> If the item has been set locally but doesn’t yet exist remotely `createdAt` and `updatedAt` will be `undefined`


```js
// Example usage
Simpla.get('/title').then((item) => {
  console.log(item);
});
```

```js
// Example response
{
  "path": "/title",
  "type": "Text",
  "data": {
    "text": "A title"
  },
  "createdAt": "2017-04-16T09:58:56.276Z",
  "updatedAt": "2017-05-09T09:25:36.835Z"
}
```

## Simpla.set

```method
set(path: string, data: Object): Promise<Object>
```

| Parameter |  |  |
| --- | --- | --- |
| `path` | `{String}` | Path to set the new data to |
| `data` | `{Object}` | Object containing mutable data (`type` and `data`) to set to path |

| Return |  |
| --- | --- |
| `Promise<Object>` | Promise that resolves with new item data |

Set data to a path in the local buffer (commited to Github when [`Simpla.save`](#simplasave) is called).

Only `data` and `type` can be set, all other properties are protected and trying to set to them will fail. The `data` object contains all custom user content, and `type` is an arbitrary hint (_not_ JavaScript type) of what kind of content the path contains. The data object is immutable and gets overwritten every time it is set.

Returns a promise that resolves with the full item object when a path has been successfully updated.

You can set an entire new path at once, any nonexistant intermediary paths will be implicitly created by Simpla with an empty data object and a type of `null`. If the given `path` is invalid, it will reject with an error.

> The `createdAt` `updatedAt` properties will only be set (or updated) after a `save` operation, as such they may be initially `undefined`.


```js
// Example usage
Simpla.set('/home/title', {
  type: 'Text',
  data: {
    text: 'The modular CMS for frontend developers'
  }
}).then((item) => {
  console.log('Updated item', item);
});
```

```json
// Example response
{
  "path": "/home/title",
  "type": "Text",
  "data": {
    "text": "The modular CMS for frontend developers"
  }
}
```

## Simpla.remove

```method
remove(path: string): Promise<null>
```

| Parameter |  |  |
| --- | --- | --- |
| `path` | `{String}` | Path to recursively remove |

| Return |  |
| --- | --- |
| `Promise` | Empty promise that resolves when paths removed |

Recursively flags a given path and all descendents for removal in the local buffer (removed from Github when [`Simpla.save`](#simplasave) is called).

```js
Simpla.remove('/pages/about').then(() => {
  console.log('About page and all child paths removed');
});
```

## Simpla.save

```method
save([path]: String): Promise<undefined>
```

| Parameter |  |  |
| --- | --- | --- |
| `path` | `{String}` | Optional path to save, if not specified all changes in buffer will be saved |

| Return |  |
| --- | --- |
| `Promise` | Empty promise that resolves when all content has been saved |

Performs a diff with remote data and persists local changes to your project.

```js
// Save all changes
Simpla.save().then(() => {
  console.log('all changes saved');
});

// Save specific path
Simpla.save('/home/title').then(() => {
  console.log('changes to /home/title saved');
});
```

## Simpla.getState

`getState(state: string): mixed`method

| Parameter |  |  |
| --- | --- | --- |
| `state` | `{String}` | State to get |

Get the current value of a part of Simpla’s public state. Simpla has the following public states:

| State |  |  |
| --- | --- | --- |
| `editable` | `{Boolean}` | Whether Simpla is in edit mode |
| `authenticated` | `{Boolean}` | Whether there is an authenticated user |
| `buffer` | `{Object}` | Metadata about the items currently in Simpla’s data buffer |
| `config` | `{Object}` | The parameters that Simpla was initialized with |
| `token` | `{String}` | Authenticated user’s JWT token |

```js
Simpla.getState('editable');
// returns True | False
```

## Simpla.editable

```method
editable(value: Boolean): undefined
```

| Parameter |  |  |
| --- | --- | --- |
| `value` | `{Boolean}` | Value to set editable to |

Update the value of the `editable` state (ie: enter/exit edit mode).

```js
// Enter edit mode
Simpla.editable(true);

// Exit edit mode
Simpla.editable(false);
```


## Simpla.observe

```method
observe(path: string, callback: Function<Object>): Object
```

| Parameter |  |  |
| --- | --- | --- |
| `path` | `{String}` | Path to observe |
| `callback` | `{Function<Object>}` | Callback to run when path changes |

| Return |  |
| --- | --- |
| `Object` | Observer object with `unobserve()` method |

Observe changes to local data at a given path. The callback is given the same item object returned by [`Simpla.get`](#simplaget). Returns an object with an `unobserve()` method used to destroy the observer.

```js
// Create observer
let observer = Simpla.observe('/content/path', (item) => {
  console.log(item.data);
});

// Destroy observer
observer.unobserve();
```

## Simpla.observeState

```method
observeState(state: String, callback: Function<mixed>): Object
```

| Parameter |  |  |
| --- | --- | --- |
| `state` | `{String}` | State to observe |
| `callback` | `{Function<Mixed>}` | Callback to run when state changes |

| Return |  |
| --- | --- |
| `Object` | Observer object with `unobserve()` method |

Observe changes to parts of Simpla’s public state. The callback is given the new value of the state. Returns an object with an `unobserve()` method used to destroy the observer.

```js
// Create observer
let stateObserver = Simpla.observeState('authenticated', (authed) => {
  console.log('User logged in?', authed);
});

// Destroy observer
stateObserver.unobserve();
```

## Simpla.version

```method
version: String
```

Get the current semver version of the Simpla library


```js
Simpla.version // returns '3.0.0'
```