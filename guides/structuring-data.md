# Structuring data

> This guide requires the [`simpla-paths`](https://github.com/SimplaElements/simpla-paths) component

Simpla organizes content into paths, which map roughly to folders and JSON files stored in your GitHub repo (under `_content/data/`).

```text
/path/to/item
```

You can use paths to model your content. For example, you could organise your project into sections, pages, and individual content fields

```text
/[section]/[page]/[field]
```

This kind of flexible heirachy is the same way that HTML works. You could represent the above path in abstract DOM terms like this:

```text
[page]
  [section]
    [field][/field]
  [/section]
[/page]
```

The `simpla-paths` component maps paths to the DOM via new HTML attributes. This allows you to structure content declaratively in code, and have it react directly to changes in your app.

Get simpla-paths from Bower or NPM, then link it into the `<head>` of your page

```sh
bower i simpla-paths --save
```

```html
<link rel="import" href="/bower_components/simpla-paths/simpla-paths.html">
```

## Path IDs

Simpla-paths constructs paths by stringing together IDs used in new HTML attributes. For example, this markup

```html
<div sid="page">
  <div sid="section">
    <simpla-text sid="content"></simpla-text>
  </div>
</div>
```

Creates the path `/page/section/content` for the `<simpla-text>` element.

Simpla-paths exposes two new HTML attributes:

*   `sid`: Scoped ID
*   `gid`: Global ID

Every element with either of these attributes gets a `path` property set on it by simpla-paths.

### Scoped IDs

Scoped IDs (the `sid` attribute) are namespaced to any parent element with a content ID. To created nested paths, just nest HTML elements with `sid` attributes

```html
<div sid="nested">
  <span sid="path"></span>
</div>

<p sid="path"></p>

<script>
  document.querySelector('div').path // '/nested'
  document.querySelector('span').path // '/nested/path'
  document.querySelector('p').path // '/path'
</script>
```

### Global IDs

Global IDs (the `gid` attribute) always create new paths regardless of where they are used. When applied to Simpla elements, they are equivelant to `path="/[gid]"`.

They are useful for creating reusable chunks of content that always have consistent data, regardless of where they appear on your site.

```html
<div sid="page">

  <!-- path = /page/title -->
  <simpla-text sid="title"></simpla-text>

  <div gid="footer">

    <!-- path = /footer/company -->
    <simpla-text sid="company"></simpla-text>

  </div>

</div>
```

## Dynamic content

Changing any ID in a chain of IDs recalculates the entire path. This means your content can easily react to changes in your DOM.

```html
<div id="page" sid="page">
  <div sid="section">
    <simpla-text sid="title"></simpla-text>
  </div>
</div>

<!-- simpla-text path = /page/section/title -->

<script>
  document.querySelector('#page').setAttribute('sid', 'about');
</script>

<!-- simpla-text path = /about/section/title -->
```

### Example: Reusable post template

Create an HTML template you would like to re-use for several pages of content, for example a post template for a blog. Wrap your template in an `sid`, set with JavaScript based on the current URL.

For example, you could redirect all URLs in a ‘blog’ section of your site to your post template (ie: `mysite.com/blog/[post]`). Get the URL slug following `/blog/` and set it to the `sid` of your post.

```html
<article id="post" sid="">
  <h1><simpla-text sid="title"></simpla-text></h1>
  <simpla-markdown sid="content"></simpla-markdown>
</article>

<script>
  var post = document.querySelector('#post'),
      postSlug = window.location.split('blog').pop();

  post.setAttribute('sid', postSlug);
</script>
```

Now all posts under `mysite.com/blog/` are automatically fetched with Simpla. Visiting `mysite.com/blog/my-post` would result in a path of `/my-post` for the template.

You could go one step further by wrapping your post template in another element with a `gid` of `'blog'`. This will give all posts a path of `/blog/[post-slug]`, instantly creating a blog collection you can query later for an index page.

```html
<div gid="blog">
  <article id="post" sid="">
    <h1><simpla-text sid="title"></simpla-text></h1>
    <simpla-text sid="content"></simpla-text>
  </article>
</div>
```

To create a new post/page under this setup, just visit a unique URL with `#edit` appended to the end

```text
mysite.com/blog/some-new-post#edit
```

### Example: Localizing content

You can also use paths to organize sets of content for your whole project. For example, you could have these sets of content for translations of your site:

```text
/en/[all other paths]
/es/[all other paths]
```

To achieve this with simpla-paths, just wrap the section of content you want to localize in an element with a `gid` set to a default locale string (eg: `'en'`). Then, use JavaScript to check the Browser language and update your localization wrapper appropriately.

```html
<div id="localize" gid="en">
  <!-- Rest of content here -->
</div>

<script>
  var localize = document.querySelector('#localize'),
      browserLocale = window.navigator.language;

  // In reality we'd limit browserLocale to a set of supported langauges
  localize.setAttribute('gid', browserLocale);
</script>
```

This will preface any path with a locale based on the user’s preferred language, instantly creating unique sets of content for your whole site.

### Going futher

There are many more ways you can take advantage of paths mapped to your DOM. A few examples:

*   Personalised content
*   Serving different content on desktop and mobile
*   Hooking your content into the logic of frontend frameworks and platforms

Of course you don’t need to use `simpla-paths` to acheive this, you can also just set the `path` property directly on Simpla elements, or use your templating library of choice to construct them yourself. The main point is that by conceptualizing content as a flexible heirachy, you can mirror your content models directly in code.

> See `simpla-paths` full documentation and API reference on its [GitHub repository](https://github.com/SimplaElements/simpla-paths)