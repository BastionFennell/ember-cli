---
layout: post
title: "Managing Dependencies"
permalink: managing-dependencies
github: "https://github.com/stefanpenner/ember-cli/blob/gh-pages/_posts/2013-04-08-managing-dependencies.md"
---

Ember CLI uses [Bower](http://bower.io/) for dependency management.

### Bower Configuration

The Bower configuration file, `bower.json`, is located at the root of your Ember
CLI project, and lists the dependencies for your project. Changes to your
dependencies should be managed through this file, rather than manually
installing packages individually.

Executing `bower install` will install all of the dependencies listed in
`bower.json` in one step.

Ember CLI is configured to have git ignore your `vendor` directory by default.
Using the Bower configuration file allows collaborators to fork your repo and get
their dependencies installed locally by executing `bower install` themselves.

Ember CLI watches `bower.json` for changes. Thus it reloads your app if you
install new dependencies via `bower install --save <dependencies>`.

Further documentation about Bower is available at their
[official documentation page](http://bower.io/).

### Compiling Bower Assets

Some bower packages include static assets that will not, by default, get merged
into your final output tree. In this case, you will need to extend your default
`Brocfile` to merge in your vendored assets. For example:

```js
// ...existing Brocfile...

var pickFiles = require('broccoli-static-compiler');
var mergeTrees  = require('broccoli-merge-trees');

// get a hold of the tree in question
var pikaday = pickFiles('vendor', {
  srcDir: '/pikaday/css',
  files: [
    'pikaday.css'
  ],
  destDir: '/assets/'
});

// default ember app source tree
var emberApp = app.toTree();

// shim in custom assets
var appAndCustomDependencies = mergeTrees([emberApp, pikaday], {
  overwrite: true
});

module.exports = appAndCustomDependencies;
```