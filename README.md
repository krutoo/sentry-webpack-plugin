<p align="center">
    <a href="https://sentry.io" target="_blank" align="center">
        <img src="https://sentry-brand.storage.googleapis.com/sentry-logo-black.png" width="280">
    </a>
<br/>
    <h1>Sentry Webpack Plugin</h1>
</p>

[![Travis](https://img.shields.io/travis/getsentry/sentry-webpack-plugin.svg?maxAge=2592000)](https://travis-ci.org/getsentry/sentry-webpack-plugin)
[![codecov](https://codecov.io/gh/getsentry/sentry-webpack-plugin/branch/master/graph/badge.svg)](https://codecov.io/gh/getsentry/sentry-webpack-plugin)
[![npm version](https://img.shields.io/npm/v/@sentry/webpack-plugin.svg)](https://www.npmjs.com/package/@sentry/webpack-plugin)
[![npm dm](https://img.shields.io/npm/dm/@sentry/webpack-plugin.svg)](https://www.npmjs.com/package/@sentry/webpack-plugin)
[![npm dt](https://img.shields.io/npm/dt/@sentry/webpack-plugin.svg)](https://www.npmjs.com/package/@sentry/webpack-plugin)

[![deps](https://david-dm.org/getsentry/sentry-webpack-plugin/status.svg)](https://david-dm.org/getsentry/sentry-webpack-plugin?view=list)
[![deps dev](https://david-dm.org/getsentry/sentry-webpack-plugin/dev-status.svg)](https://david-dm.org/getsentry/sentry-webpack-plugin?type=dev&view=list)
[![deps peer](https://david-dm.org/getsentry/sentry-webpack-plugin/peer-status.svg)](https://david-dm.org/getsentry/sentry-webpack-plugin?type=peer&view=list)

A webpack plugin acting as an interface to [Sentry CLI](https://docs.sentry.io/learn/cli/).

### Installation

Using npm:

```
$ npm install @sentry/webpack-plugin
```

Using yarn:

```
$ yarn add @sentry/webpack-plugin
```

### CLI Configuration

You can use either `.sentryclirc` file or ENV variables described here https://docs.sentry.io/learn/cli/configuration/

### Usage

```js
const SentryCliPlugin = require('@sentry/webpack-plugin');

const config = {
  plugins: [
    new SentryCliPlugin({
      include: '.',
      ignoreFile: '.sentrycliignore',
      ignore: ['node_modules', 'webpack.config.js'],
      configFile: 'sentry.properties'
    })
  ]
};
```

#### Options

* `release [optional]` - unique name of a release, can be either a `string` or a `function` which will expose you a compilation hash as it's first argument, which is 20-char long string, unique for a given codebase, defaults to `sentry-cli releases propose-version` command which should always return the correct version
* `include [required]` - `string` or `array`, one or more paths that Sentry CLI should scan recursively for sources. It will upload all `.map` files and match associated `.js` files
* `ignoreFile [optional]` - `string`, path to a file containing list of files/directories to ignore. Can point to `.gitignore` or anything with same format
* `ignore [optional]` - `string` or `array`, one or more paths to ignore during upload. Overrides entries in `ignoreFile` file. If neither `ignoreFile` or `ignore` are present, defaults to `['node_modules']`
* `configFile [optional]` - `string`, path to Sentry CLI config properties, as described in https://docs.sentry.io/learn/cli/configuration/#properties-files. By default, the config file is looked for upwards from the current path and defaults from `~/.sentryclirc` are always loaded
* `ext [optional]` - `string`, adds an additional file extension to be considered. By default the following file extensions are processed: js, map, jsbundle and bundle.
* `urlPrefix [optional]` - `string`, this sets an URL prefix in front of all files. This defaults to ~/ but you might want to set this to the full URL. This is also useful if your files are stored in a sub folder. eg: --url-prefix '~/static/js'
* `validate [optional]` - `boolean`, this attempts sourcemap validation before upload when rewriting is not enabled. It will spot a variety of issues with source maps and cancel the upload if any are found. This is not the default as this can cause false positives.
* `stripPrefix | stripCommonPrefix [optional]` - `string`, when paired with `--rewrite` this will chop-off a prefix from uploaded files. For instance you can use this to remove a path that is build machine specific.
* `sourceMapReference [optional]` - `boolean`, this prevents the automatic detection of sourcemap references.
* `rewrite [optional]` - `boolean`, enables rewriting of matching sourcemaps so that indexed maps are flattened and missing sources are inlined if possible., defaults to `true`

You can find more information about these options in our official docs: https://docs.sentry.io/learn/cli/releases/#upload-source-maps
