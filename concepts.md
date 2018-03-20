# Concepts

## Storage backends

Backstage can use theoretically any storage mechanism to store deployed packages. A Backstage storage backend is simply an object with two properties: `get` and `deploy`.

### `get`

The `get` property is a function

At the moment, it ships with just one storage backend: the file system backend.
