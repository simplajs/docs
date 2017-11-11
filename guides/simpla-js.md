# Simpla.js

Simpla.js provides the tools for elements and developers to store and fetch data from Github, manage the state of a Simpla app, authenticate users, and react to changes in content.

## Installation

Simpla is available on NPM and the Unpkg CDN as `simpla`.

```sh
yarn add simpla
```

Import it as an ES6/UMD module

```js
import Simpla from 'simpla';

window.Simpla = Simpla;
```

Or link it in with a script tag.

```html
<script src="https://unpkg.com/simpla@^3.0.0"></script>
```

The `simpla` package includes the core Simpla library (`simpla.min.js`) as well as seperate auth adapters. Currently only one adapter (for [Netlify](https://www.netlify.com)) is bundled out of the box.

## Initializing

Before you can use Simpla you must initialize your project with `Simpla.init(config)`. Read the [getting started](/docs/guides/get-started) guide for more information on how to setup the supporting services (Github, auth adapter) Simpla requires.

```js
Simpla.init({
  /**
   * Github Repo
   * Repository where content will be stored (in a '_content' folder)
   */
  repo: 'username/repo',

  /**
   * Auth adapter
   * Used to authenticate users with Github from your site
   * Included separately to simpla.js core
   */
  auth: new SimplaNetlify({ site: 'mysite' }),

  /**
   * Public content source (optional)
   * Public URL of your content, defaults to fetching directly from GitHub
   * Push your content to a CDN like Netlify in production
   */
  source: window.location.origin,

  /**
   * Commit branch (optional)
   * Git branch Simpla commits new content to, defaults to 'master'
   * Change this in development to make non-production changes
   */
  branch: 'master',

  /**
   * Public directory (optional)
   * Base directory to store Simpla's '_content' folder
   * Defaults to the root of the repo
   */
   public: 'dist'
});
```

> **Pro tip:** if you save your content alongside your site’s codebase in the same github repo, set `source` to `window.location.origin` so you can save and read content locally in development

## Authentication

Authenticate users with `Simpla.login` and `Simpla.logout`. These methods hook into the authentication adapter you provided in `Simpla.init`

```js
// Prompt for login from auth adapter
Simpla.login();

// Log user out locally
Simpla.logout();
```

The `login` method prompts your auth adapter to log the user in. For OAuth adapters like `SimplaNetlify`, it will open a login window for your Github credentials. It returns a promise that resolves when the user is successfully logged in.

The `logout` method clears the user’s token and returns a promise when the user has been logged out.

> **Pro tip:** the [`simpla-admin`](https://www.webcomponents.org/element/SimplaElements/simpla-admin) component includes all the UI for your users to authenticate with Simpla, save content, and more. Use these JS methods directly when you want more control.

## Working with data

Simpla provides low-level methods for fetching and manipulating your content. All data operations are non-destructive and operate on a local data buffer until `Simpla.save` is called, which commits changes to your GitHub repo.

### Get

Fetch a content path with `Simpla.get`. It takes the path to fetch as a single argument, and returns a promise with the path’s data

```js
Simpla.get('/content/path')
  .then((data) => {
    // Content fetched
  });
```

The object returned has the following properties

| Property | Type | Description |
| --- | --- | --- |
| `path` | `String` | Path to the content |
| `type` | `String` | Type of content that is stored in this path |
| `data` | `Object` | Custom data |
| `createdAt` | `String` | Timestamp of when this path was created |
| `updatedAt` | `String` | Timestamp of when this path was last modified |

### Set

Create or modify content with `Simpla.set`. It takes two arguments, the content path to operate on, and the new data. It returns a promise.

```js
Simpla.set('/content/path', {
  type: '...',
  data: { ... }
}).then(() => {
  // Data set to buffer
});
```

> Remember, these changes won’t be persisted until you call `Simpla.save`

All custom data (ie: user content) must be set inside the `data` object, and most paths should also set a `type`. The `type` property is an arbitrary hint (_not_ a JavaScript type) of what kind of content the path contains. The `data` object is immutable and gets overwritten with every set.

All properties outside of `data` and `type` are protected, and trying to set to them will fail.

You can set an entire new path at once, any nonexistant intermediary paths will be implicitly created by Simpla

```js
Simpla.set('/path/to/item', { ... });

// Path /path/to/item created with data
// Empty paths /path and /path/to implicitly created by Simpla
```

### Remove

Delete a path with `Simpla.remove`. It takes the path to remove as a single argument, and returns a promise.

```js
Simpla.remove('/content/path')
  .then(() => {
    // Data deleted
  });
```

> Remember, these changes wont be persisted until `Simpla.save` is called

Removing a path will remove all of its data and all the data in all the paths below it. Given the paths

```text
/foo/bar/baz
/foo/bar/qux
```

Calling `Simpla.remove('/foo/bar')` will result in the removal of all the following:

```text
/foo/bar/baz
/foo/bar/qux
/foo/bar
```

### Save

The `Simpla.save` method performs a diff between the local buffer and remote data, and commits all changes to your GitHub repo. It takes an optional path to save, and returns a promise.

```js
// Save all changes
Simpla.save().then(() => console.log('all changes saved'));

// Save changes at specific path
Simpla.save('/home/title').then(() => console.log('home title changes saved'));
```

To call save, you must have first authenticated with `Simpla.login` and initialized Simpla.

## States

Simpla uses a state tree to coordinate elements and data on a page. These are the public states currently managed by Simpla

| State | Type | Description |
| --- | --- | --- |
| `authenticated` | `Boolean` | Whether there is an authenticated user |
| `editable` | `Boolean` | Whether Simpla is in edit mode |
| `buffer` | `Object` | Metadata about the items currently in Simpla’s data buffer |
| `config` | `Object` | The parameters that Simpla was initialized with |

### Get a state

Get the current value of a state with `Simpla.getState`. It takes the state to fetch as a single argument.

```js
Simpla.getState('authenticated'); // Returns true/false
```

### Control the editable state

Enter and exit edit mode globally using the `Simpla.editable` method. It takes a single Boolean argument.

```js
// Enter edit mode
Simpla.editable(true);

// Exit edit mode
Simpla.editable(false);
```

## Observers

Simpla provides observers to react to changes in state and content

### Observing changes to data

Observe changes to data with the `Simpla.observe` method. It takes two arguments, the content path to observe, and a callback to execute when the data in that path changes. It returns an object containing an `unobserve()` method used to destroy the observer.

```js
// Create observer
let observer = Simpla.observe('/content/path', (data) => {});

// Destroy observer
observer.unobserve();
```

### Observing changes to state

Observe changes to a part of Simpa’s state with `Simpla.observeState`. It has the same syntax as `observe`.

```js
// Create observer
let stateObserver = Simpla.observeState('editable', (value) => {});

// Destroy observer
stateObserver.unobserve();
```