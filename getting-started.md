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

const fileDir = path.join(__dirname, '../files')
const storageBackend = fileSystem(fileDir)

backstage({ storageBackend }).listen(3000)
```

(Note: this example can be cloned from the [`backstage-example` repo](https://github.com/BackstageJS/backstage-example).)

In this example, we import `backstage` from `backstage-server`. `backstage()` simply returns an instance of `Express`, which you can use just the same as if you'd imported it from Express via `const app = require('express')()`. However, it's been configured in `backstage-server` to include `cookie-parser`, as well as middleware for the storage backend that you pass it.

Run your server via `node app.js`, and it's ready to start receiving deployments and serving your files!

## The CLI client

Use the CLI client to deploy your front-end apps to your Backstage server instance. First, install `backstage-cli` globally:

```bash
$ npm install -g backstage-cli
```

Next, `cd` into the directory of a front-end app you'd like to deploy:

```bash
$ cd ~/projects/my-react-app
```

Run `backstage init` to go through the configuration wizard. At the end, it will create a `.backstagerc` file in the root of your repo, which contains configuration for future deploys.

You're now ready to deploy! Build your app using whatever command you normally use (e.g., `npm run build`), then deploy your app to Backstage:

```bash
$ backstage deploy
```

By default, this will deploy to the app you named when you ran `backstage init`, and to a key equal to your current Git branch name. If you'd rather use a different key than your Git branch name, use the `--key` option (`-k` for short):

```bash
$ backstage deploy --key myKey
```

Once the deploy has completed, Backstage will display a link you can use to access this specific deployment. Open it up, and your front-end app is ready to test!
