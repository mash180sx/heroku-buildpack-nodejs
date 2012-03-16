Heroku buildpack: Node.js
=========================

This is a Heroku buildpack for Node.js v0.6.13 apps.
It uses [NPM](http://npmjs.org/) and [SCons](http://www.scons.org/).

Usage
-----

Example usage:

    $ ls
    Procfile  package.json  web.js

    $ heroku create --stack cedar --buildpack http://github.com/hakobera/heroku-buildpack-nodejs.git

    $ git push heroku master
    ...
    -----> Heroku receiving push
    -----> Fetching custom buildpack
    -----> Node.js app detected
    -----> Vendoring node 0.6.13
    -----> installing dependencies with npm 1.1.4
           express@2.5.2 ./node_modules/express
           ├── mime@1.2.4
           ├── mkdirp@0.0.7
           ├── qs@0.4.0
           └── connect@1.8.2
           Dependencies installed

The buildpack will detect your app as Node.js if it has the file `package.json` in the root.  It will use NPM to install your dependencies, and vendors a version of the Node.js runtime into your slug.  The `node_modules` directory will be cached between builds to allow for faster NPM install time.
