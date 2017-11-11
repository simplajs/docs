# Content model

Simpla saves your content in a JSON file tree in the GitHub repo you initialized your project with (under `_content/data/`).

## Content paths

Simpla uses paths to identify content. Paths roughly map to the underlying file structure in your content repo, but it’s not 1:1 (eg: paths do not include the `_content/data/` base, or `.json` file extensions).

The path

```text
/path/to/item
```

Maps rougly to

```
/_content/data/path/to/item.json

```

Use paths to structure your content. There are no strict rules in how you model content with Simpla, every path can act as a Collection, a Post, a Field, or fulfill several roles.

## Data schema

The JSON data at each path follows a consistent schema. Every item has the following fields

| Property | Type | Description |
| --- | --- | --- |
| `path` | `String` | Content path where this data is stored |
| `type` | `String` | What kind of content is stored at this path |
| `data` | `Object` | Custom data |
| `createdAt` | `String` | Timestamp of when this path was created |
| `updatedAt` | `String` | Timestamp of when this path was last updated |

```js
{
  "path": "/path/to/item",
  "type": "Text",
  "data": {
    "html": "<p>My content</p>"
  },
  "createdAt": "2017-04-05T17:20:17.440Z",
  "updatedAt": "2017-04-06T09:31:47.499Z"
}
```

All custom data (ie: user content) for a given path is stored in the `data` object. The data inside this object can have any format, and doesn’t follow a strict schema. This is where Simpla elements store their data.

The `type` field is an arbitrary hint of what kind of content is stored at a given path. For example: `'Image'`, `'Text'`, `'Article'`. Types are not predefined or enforced by Simpla, and are used to help with content modelling a project.

All other fields are immutable and set by Simpla automatically.