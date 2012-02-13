Heroku buildpack: Node.js
=========================

This is a [Heroku buildpack](http://devcenter.heroku.com/articles/buildpacks) for Node.js apps.
It uses [NPM](http://npmjs.org/) and [SCons](http://www.scons.org/).

Usage
-----

Example usage:

    $ ls
    Procfile  package.json  web.js

    $ heroku create --stack cedar --buildpack 'https://github.com/hakobera/heroku-buildpack-nodejs.git#versions'

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Node.js app detected
    -----> Vendoring node 0.6.10
    -----> installing dependencies with npm 1.1.0-3
      ...

The buildpack will detect your app as Node.js if it has the file `package.json` in the root.  It will use NPM to install your dependencies, and vendors a version of the Node.js runtime into your slug.  The `node_modules` directory will be cached between builds to allow for faster NPM install time.

Node.js and npm versions
------------------------

You can specify the versions of Node.js and npm your application requires using `package.json`

    {
      "name": "myapp",
      "engines": {
        "node": ">=0.4.7 <0.7.0",
        "npm": ">=1.0.0"
      }
    }

To list the available versions of Node.js and npm, see these manifests:

- [Node.js](http://hakobera-heroku-node.s3.amazonaws.com/manifest.nodejs)
- [NPMm](http://hakobera-heroku-node.s3.amazonaws.com/manifest.npm)

Hacking
-------

To use this buildpack, fork it on Github.  Push up changes to your fork, then create a test app with `--buildpack <your-github-url>` and push to it.

To change the vendored binaries for Node.js, NPM, and SCons, use the helper scripts in the `support/` subdirectory.  You'll need an S3-enabled AWS account and a bucket to store your binaries in.

For example, you can change the default version of Node.js to v0.6.10.

First you'll need to build a Heroku-compatible version of Node.js:

    $ gem install aws-sdk
    $ export AWS_ID=xxx AWS_SECRET=yyy S3_BUCKET=zzz
    $ support/aws/s3 create $S3_BUCKET
    $ support/package_nodejs 0.6.10

Open `bin/compile` in your editor, and change the following lines:

    DEFAULT_NODE_VERSION="0.6.10"
    S3_BUCKET=zzz

Commit and push the changes to your buildpack to your Github fork, then push your sample app to Heroku to test.  You should see:

    -----> Vendoring node 0.6.10
