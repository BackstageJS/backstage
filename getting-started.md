# Getting started

There are two components to Backstage: an Express server that hosts your branches, and a CLI client to deploy your apps. Let's start by setting up the server.

## The server

A Backstage server instance is responsible for two things: handling deployments, and serving deployed packages. Both of these functions are managed by a storage backend -- an Express middleware that saves deployed packages to a specific location, and then serves them from that location when requested.

At the moment, Backstage ships with only one storage backed: a file system backend, which simply writes deployed packages to disk on the same machine as the Backstage server instance. In the future, additional storage backends (e.g., for integrating with Amazon S3) may be added.

To set up your Backstage server instance, you'll need to create a simple repo that imports and configures the Backstage app with the storage backend of your choice. First, install `backstage-server` as a dependency:

```bash
$ yarn add backstage-server
```

Or:

```bash
$ npm install --save backstage-server
```

Then, create an `app.js` file which will run your server instance. In this example, we'll use the file system backend:

```JavaScript
// app.js
const path = require('path')
const { backstage } = require('backstage-server')
const { fileSystem } = require('backstage-server/dist/storage-backends/file-system')

const app = backstage(fileSystem(path.join(__dirname, '../files')))

app.listen(3000)
```

(Note: this example can be cloned from the [`backstage-example` repo](https://github.com/jessepinho/backstage-example).)

In this example, we import `backstage` from `backstage-server`. `backstage()` simply returns an instance of `Express`, which you can use just the same as if you'd imported it from Express via `const app = require('express')()`. However, it's been configured in `backstage-server` to include `cookie-parser`, as well as middleware for the storage backend that you pass it.

Run your server via `node app.js`, and it's ready to start receiving deployments and serving your files!
