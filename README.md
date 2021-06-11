# vue-cli-plugin-unit-karmajscli3

Run unit tests in a @vue/cli project with Karma and Mocha

## Install

```bash
$ vue add vue-cli-plugin-unit-karmajscli3
```

If you want to install it using npm:

```bash
$ npm install vue-cli-plugin-unit-karmajscli3
$ vue invoke vue-cli-plugin-unit-karmajscli3
```

If you don't want to use `vue invoke`, install required devDependencies which are present in `generator/index.js`.

## Usage

`vue-cli-service test:unit [options] [...files]`

### Injected commands:
* `vue-cli-service test:unit`

  Default files matches are: any files in tests/unit that end in `.spec.js`.

  Command line options: 

  * `--watch, -w`: run in watch mode

NOTE: If you want to override default karma settings, you can use the `pluginOptions.karma` key in `vue.config.js` like this:

```javascript
pluginOptions: {
  karma: {
    files: [ 'tests/**/*.spec.js' ]
  }
}
```

This module is inspired from [vue-cli-plugin-unit-karmajs] which has some sourcemap bugs when get coverage report with vue file,when use [istanbul-instrumenter-loader] , the way to fix it is set sourcemap false.

### Vue cli3 vue.config.js 
```javascript
module.exports = {
  chainWebpack: config => {
  
    if (process.env.NODE_ENV === 'test') {
      config.devtool(false)/* muset set devtool false ,if not will get error file path with vue file*/
      config.module.rule('js')
        .test(/\.js$/)
        .exclude
        .add(/node_modules/)
        .add(/tests/)
        .end()
        .use('istanbul-instrumenter-loader')
        .loader('istanbul-instrumenter-loader')
        .options({
          esModules: true,
        });
    }
  },
  pluginOptions:{
    karma:{
      browsers: ['Chrome'],
      reporters: ['mocha', 'coverage'],
      files: ['tests/unit/**/*.spec.js']
    }
  }
}

```

[vue-cli-plugin-unit-karma]: https://github.com/davidwallacejackson/vue-cli-plugin-unit-karma
[vue-cli-plugin-unit-karmajs]: https://github.com/tushararora/vue-cli-plugin-unit-karmajs