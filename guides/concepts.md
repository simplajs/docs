# Concepts
This guide covers some of the fundamental concepts underlying Simpla, and usage basics for developers.

## Fundamentals

### Simpla.js

Simpla’s core [JavaScript library](/guides/simpla-js) provides a low-level API used by elements and developers to save content, manage state, coordinate changes, authenticate users, and react to content updates. It’s a tiny (~5kb) core for an expansive ecosystem.

### Elements

Simpla uses an open ecosystem of [web components](https://www.webcomponents.org) to manage content. Each element provides a different content type (text, images, videos) or management tool (collections, admin).

For example, [`<simpla-text>`](https://www.webcomponents.org/element/SimplaElements/simpla-text) is a block of editable richtext you can update inline.

```html
<!-- Block of editable richtext -->
<simpla-text path="/text"></simpla-text>
```

[Explore Simpla’s components](https://www.webcomponents.org/collection/simplaio/simpla-elements).

### Github backend

Simpla uses [Github](https://github.com) as a backend. Content is stored as a JSON file tree (under a `_content` folder), and users map to collaborators on your repo. Each time you save content on your site, Simpla commits the changes to Github. This has a number of really important benefits:

*   You can push all your content to a static-file CDN for superfast performance
*   You don’t need to run a server to add dynamic content to your site or app
*   Your content is in real version control. To roll back a change, just revert a commit. To save a draft, change the branch Simpla commits to. In future, Simpla will have APIs to manage this directly from the frontend

You can store content in its own repo or alongside the rest of your site. Simpla can read directly from Github in development, or from a CDN or flatfile host for production.

### Auth adapters

Collaborators on your content repo are able to log into Simpla on your site and update content. Simpla needs an OAuth server to authenticate these users with Github.

To achieve this, you configure an auth adapter of your choice while initializing Simpla. Currently Simpla ships with one adapter (for [Netlify](https://netlify.com)) out-of-the-box. We’ll be adding more in future, and you can also write your own (eg: custom Node server, other PaaS services, etc).

### Edit mode

Content elements (like `<simpla-text>`) both display and provide the tools to edit content. Simpla has a mechanism called edit mode, which makes all content elements on a page editable. If a user is authenticated, they can update content and save changes while in edit mode.

If you have included [`simpla-admin`](http://webcomponents.org/element/SimplaElements/simpla-admin), you can enter edit mode by adding `#edit` to the end of your URL. Or, enter edit mode directly via Simpla.js with the `editable()` method

```js
// Enter edit mode programatically
Simpla.editable(true)
```

## Adding elements

Find elements on [webcomponents.org](https://www.webcomponents.org/collection/simplaio/simpla-elements). Every component has its own documentation, demos, and API references.

Install them via [Bower](http://bower.io/) (NPM support upcoming)

```sh
bower i [element] --save
```

Then import them into the `<head>` of your page using HTML imports (ES module support upcoming). Most elements are distributed as an HTML file under the name of the element

```html
<link rel="import" href="/bower_components/[element-name]/[element-name].html">
```

## Using elements

Once you’ve imported an element you can use it on your page. Refer to the element’s documentation for full usage instructions.

All content elements require a unique `path` (string beginning with `/`), which defines where Simpla will store the data in its Github folder.

```html
<!-- Example usage of a typical Simpla element -->
<simpla-text path="/my/text"></simpla-text>
```

Element properties can be set both as HTML attributes or with JavaScript. `CamelCased` properties are serialized as `kebab-cased` attributes

```html
<simpla-text path="/text"></simpla-text>
```

```js
document.querySelector('simpla-text').path = '/text';
```
