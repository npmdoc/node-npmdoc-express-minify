# api documentation for  [express-minify (v0.2.0)](https://github.com/SummerWish/express-minify)  [![npm package](https://img.shields.io/npm/v/npmdoc-express-minify.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-express-minify) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-express-minify.svg)](https://travis-ci.org/npmdoc/node-npmdoc-express-minify)
#### Automatically minify and cache your javascript and css files.

[![NPM](https://nodei.co/npm/express-minify.png?downloads=true)](https://www.npmjs.com/package/express-minify)

[![apidoc](https://npmdoc.github.io/node-npmdoc-express-minify/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-express-minify_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-express-minify/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-express-minify/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-express-minify/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Breezewish"
    },
    "bugs": {
        "url": "https://github.com/SummerWish/express-minify/issues"
    },
    "dependencies": {
        "cssmin": "^0.4.3",
        "on-headers": "^1.0.1",
        "uglify-js": "^2.6.1"
    },
    "description": "Automatically minify and cache your javascript and css files.",
    "devDependencies": {
        "async": "^1.5.2",
        "coffee-script": "^1.10.0",
        "compression": "^1.6.0",
        "less": "^2.5.3",
        "mocha": "^2.3.4",
        "node-sass": "^3.4.2",
        "should": "^8.1.1",
        "stylus": "^0.53.0",
        "supertest": "^1.1.0"
    },
    "directories": {},
    "dist": {
        "shasum": "69e74bc40d832e0a97c8472a3bb1f08e1b1c6245",
        "tarball": "https://registry.npmjs.org/express-minify/-/express-minify-0.2.0.tgz"
    },
    "engines": {
        "node": ">= 0.12.0"
    },
    "gitHead": "dc6cf75d150a8c2cae6ad8c2c0ec7648e9484151",
    "homepage": "https://github.com/SummerWish/express-minify",
    "keywords": [
        "uglify",
        "minify",
        "express"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "breezewish",
            "email": "me@breeswish.org"
        }
    ],
    "name": "express-minify",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/SummerWish/express-minify.git"
    },
    "scripts": {
        "test": "mocha --check-leaks --reporter dot"
    },
    "version": "0.2.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module express-minify](#apidoc.module.express-minify)
1.  [function <span class="apidocSignatureSpan">express-minify.</span>Cache (cacheDirectory)](#apidoc.element.express-minify.Cache)
1.  [function <span class="apidocSignatureSpan">express-minify.</span>Minifier (uglifyJS, cssmin, errorHandler)](#apidoc.element.express-minify.Minifier)
1.  object <span class="apidocSignatureSpan">express-minify.</span>Minifier.prototype

#### [module express-minify.Minifier](#apidoc.module.express-minify.Minifier)
1.  [function <span class="apidocSignatureSpan">express-minify.</span>Minifier (uglifyJS, cssmin, errorHandler)](#apidoc.element.express-minify.Minifier.Minifier)
1.  [function <span class="apidocSignatureSpan">express-minify.Minifier.</span>defaultErrorHandler (err, stage, assetType, minifyOptions, body, callback)](#apidoc.element.express-minify.Minifier.defaultErrorHandler)
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_COFFEE
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_CSS
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_JS
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_JSON
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_LESS
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_SASS
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_STYLUS
1.  number <span class="apidocSignatureSpan">express-minify.Minifier.</span>TYPE_TEXT

#### [module express-minify.Minifier.prototype](#apidoc.module.express-minify.Minifier.prototype)
1.  [function <span class="apidocSignatureSpan">express-minify.Minifier.prototype.</span>compileAndMinify (assetType, minifyOptions, body, callback)](#apidoc.element.express-minify.Minifier.prototype.compileAndMinify)



# <a name="apidoc.module.express-minify"></a>[module express-minify](#apidoc.module.express-minify)

#### <a name="apidoc.element.express-minify.Cache"></a>[function <span class="apidocSignatureSpan">express-minify.</span>Cache (cacheDirectory)](#apidoc.element.express-minify.Cache)
- description and source-code
```javascript
Cache = function (cacheDirectory) {
  this.isFileCache = (cacheDirectory !== false);
  if (this.isFileCache) {
    // whether the directory is writeable
    cacheDirectory = path.normalize(cacheDirectory + '/').toString();
    try {
      fs.accessSync(cacheDirectory, fs.W_OK);
    } catch (ignore) {
      console.log('WARNING: express-minify cache directory is not writeable, fallback to memory cache.');
      this.isFileCache = false;
    }
  }
  if (this.isFileCache) {
    this.layer = new FileCache(cacheDirectory);
  } else {
    this.layer = new MemoryCache();
  }
}
```
- example usage
```shell
...
  var css_match = options.css_match || /css/;
  var sass_match = options.sass_match || /scss/;
  var less_match = options.less_match || /less/;
  var stylus_match = options.stylus_match || /stylus/;
  var coffee_match = options.coffee_match || /coffeescript/;
  var json_match = options.json_match || /json/;

  var cache = new Factory.Cache(options.cache || false);
  var minifier = new Factory.Minifier(options.uglifyJS, options.cssmin, options.onerror);

  return function express_minify_middleware(req, res, next) {
var write = res.write;
var end = res.end;

var buf = null;
...
```

#### <a name="apidoc.element.express-minify.Minifier"></a>[function <span class="apidocSignatureSpan">express-minify.</span>Minifier (uglifyJS, cssmin, errorHandler)](#apidoc.element.express-minify.Minifier)
- description and source-code
```javascript
Minifier = function (uglifyJS, cssmin, errorHandler) {
  this.handleError = errorHandler || Minifier.defaultErrorHandler;
  this.uglifyJS = uglifyJS || require('uglify-js');
  this.cssmin = cssmin || require('cssmin');
  this.sass = testModule('node-sass');
  this.less = testModule('less');
  this.stylus = testModule('stylus');
  this.coffee = testModule('coffee-script');
}
```
- example usage
```shell
...
  var sass_match = options.sass_match || /scss/;
  var less_match = options.less_match || /less/;
  var stylus_match = options.stylus_match || /stylus/;
  var coffee_match = options.coffee_match || /coffeescript/;
  var json_match = options.json_match || /json/;

  var cache = new Factory.Cache(options.cache || false);
  var minifier = new Factory.Minifier(options.uglifyJS, options.cssmin, options.onerror);

  return function express_minify_middleware(req, res, next) {
var write = res.write;
var end = res.end;

var buf = null;
var type = Factory.Minifier.TYPE_TEXT;
...
```



# <a name="apidoc.module.express-minify.Minifier"></a>[module express-minify.Minifier](#apidoc.module.express-minify.Minifier)

#### <a name="apidoc.element.express-minify.Minifier.Minifier"></a>[function <span class="apidocSignatureSpan">express-minify.</span>Minifier (uglifyJS, cssmin, errorHandler)](#apidoc.element.express-minify.Minifier.Minifier)
- description and source-code
```javascript
Minifier = function (uglifyJS, cssmin, errorHandler) {
  this.handleError = errorHandler || Minifier.defaultErrorHandler;
  this.uglifyJS = uglifyJS || require('uglify-js');
  this.cssmin = cssmin || require('cssmin');
  this.sass = testModule('node-sass');
  this.less = testModule('less');
  this.stylus = testModule('stylus');
  this.coffee = testModule('coffee-script');
}
```
- example usage
```shell
...
  var sass_match = options.sass_match || /scss/;
  var less_match = options.less_match || /less/;
  var stylus_match = options.stylus_match || /stylus/;
  var coffee_match = options.coffee_match || /coffeescript/;
  var json_match = options.json_match || /json/;

  var cache = new Factory.Cache(options.cache || false);
  var minifier = new Factory.Minifier(options.uglifyJS, options.cssmin, options.onerror);

  return function express_minify_middleware(req, res, next) {
var write = res.write;
var end = res.end;

var buf = null;
var type = Factory.Minifier.TYPE_TEXT;
...
```

#### <a name="apidoc.element.express-minify.Minifier.defaultErrorHandler"></a>[function <span class="apidocSignatureSpan">express-minify.Minifier.</span>defaultErrorHandler (err, stage, assetType, minifyOptions, body, callback)](#apidoc.element.express-minify.Minifier.defaultErrorHandler)
- description and source-code
```javascript
defaultErrorHandler = function (err, stage, assetType, minifyOptions, body, callback) {
  if (stage === 'compile') {
    callback(err, JSON.stringify(err));
    return;
  }
  callback(err, body);
}
```
- example usage
```shell
n/a
```



# <a name="apidoc.module.express-minify.Minifier.prototype"></a>[module express-minify.Minifier.prototype](#apidoc.module.express-minify.Minifier.prototype)

#### <a name="apidoc.element.express-minify.Minifier.prototype.compileAndMinify"></a>[function <span class="apidocSignatureSpan">express-minify.Minifier.prototype.</span>compileAndMinify (assetType, minifyOptions, body, callback)](#apidoc.element.express-minify.Minifier.prototype.compileAndMinify)
- description and source-code
```javascript
compileAndMinify = function (assetType, minifyOptions, body, callback) {
  if (typeof callback !== 'function') {
    return;
  }

  var self = this;
  var result, opt;

  switch (assetType) {
  case Minifier.TYPE_JS:
    result = body;
    try {
      if (!minifyOptions.noMinify) {
        opt = extend({fromString: true}, minifyOptions.uglifyOpt);
        result = self.uglifyJS.minify(result, opt).code;
      }
    } catch (err) {
      self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
      return;
    }
    callback(null, result);
    break;
  case Minifier.TYPE_CSS:
    result = body;
    try {
      if (!minifyOptions.noMinify) {
        result = self.cssmin(result);
      }
    } catch (err) {
      self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
      return;
    }
    callback(null, result);
    break;
  case Minifier.TYPE_SASS:
    if (!self.sass) {
      self.sass = require('node-sass');
    }
    result = body;
    try {
      result = self.sass.renderSync({
        data: result
      }).css.toString();
    } catch (err) {
      self.handleError(err, 'compile', assetType, minifyOptions, result, callback);
      return;
    }
    try {
      if (!minifyOptions.noMinify) {
        result = self.cssmin(result);
      }
    } catch (err) {
      self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
      return;
    }
    callback(null, result);
    break;
  case Minifier.TYPE_LESS:
    if (!self.less) {
      self.less = require('less');
    }
    result = body;
    self.less.render(result, function (err, output) {
      if (err) {
        self.handleError(err, 'compile', assetType, minifyOptions, result, callback);
        return;
      }
      result = output.css;
      try {
        if (!minifyOptions.noMinify) {
          result = self.cssmin(result);
        }
      } catch (err) {
        self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
        return;
      }
      callback(null, result);
    });
    break;
  case Minifier.TYPE_STYLUS:
    if (!self.stylus) {
      self.stylus = require('stylus');
    }
    result = body;
    self.stylus.render(result, function (err, css) {
      if (err) {
        self.handleError(err, 'compile', assetType, minifyOptions, result, callback);
        return;
      }
      result = css;
      try {
        if (!minifyOptions.noMinify) {
          result = self.cssmin(result);
        }
      } catch (err) {
        self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
        return;
      }
      callback(null, result);
    });
    break;
  case Minifier.TYPE_COFFEE:
    if (!self.coffee) {
      self.coffee = require('coffee-script');
    }
    result = body;
    try {
      result = self.coffee.compile(result);
    } catch (err) {
      self.handleError(err, 'compile', assetType, minifyOptions, result, callback);
      return;
    }
    try {
      if (!minifyOptions.noMinify) {
        opt = extend({fromString: true}, minifyOptions.uglifyOpt);
        result = self.uglifyJS.minify(result, opt).code;
      }
    } catch (err) {
      self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
      return;
    }
    callback(null, result);
    break;
  case Minifier.TYPE_JSON:
    result = body;
    try {
      if (!minifyOptions.noMinify) {
        result = JSON.stringify(JSON.parse(result));
      }
    } catch (err) {
      self.handleError(err, 'minify', assetType, minifyOptions, result, callback);
      return;
    }
    callback(null, result);
    break;
  default:
    callback(null, body);
    break;
  }
}
```
- example usage
```shell
...

var cacheKey = crypto.createHash('sha1').update(JSON.stringify(minifyOptions) + buffer).digest('hex').toString();
var self = this;

cache.layer.get(cacheKey, function (err, minized) {
  if (err) {
    // cache miss
    minifier.compileAndMinify(type, minifyOptions, buffer.toString(encoding), function (err, minized) {
      if (self._no_cache || err) {
        // do not cache the response body
        write.call(self, minized, 'utf8');
        end.call(self);
      } else {
        cache.layer.put(cacheKey, minized, function () {
          write.call(self, minized, 'utf8');
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
