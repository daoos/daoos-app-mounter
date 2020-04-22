# daoos-app-mounter

> a mounter for daoos vue apps

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

# 初始化项目

```shell
vue init webpack-simple daoos-app-mounter
```

![初始化项目](https://oscimg.oschina.net/oscnet/up-7c1412f95198a2ba7a8ec07f2b885cf07e2.png "初始化项目")

# 编写插件

![项目结构](https://oscimg.oschina.net/oscnet/up-7b1951221048676f91e54e58a5a87fc687e.png "项目结构")

# 文件内容

-   toast.vue

```javascript
<template>
  <div class="vue-toast-wraper" v-show="isShow">
    {{msg}}
  </div>
</template>
<script>
  export default {
    name:'toast',
    props:{
      msg:{
        default:"",
        type:String
      },
      isShow:{
        default:false,
        type:Boolean
      }
    },
    mounted(){
      if(this.isShow){
        setTimeout(() => {
          this.isShow = false
        },2500);
      }
    }
  }
</script>
<style scoped>
  .vue-toast-wraper{
    background: rgba(0, 0, 0, 0.6);
    color: #fff;
    font-size: 17px;
    padding: 10px;
    border-radius:12px;
    display: -webkit-box;
    -webkit-box-pack: center;
    -webkit-box-align: center;
    position: fixed;
    top: 50%;
    left: 50%;
    z-index: 2000;
    -webkit-transform: translateY(-50%);
    transform: translateY(-50%);
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
  }
</style>

```

-   toast.js

```javascript
import VueToastPlugin from './toast.vue'
const toastPlugin = {
  install: function(Vue) {
    Vue.component(VueToastPlugin.name, VueToastPlugin)
  }
}
// global 情况下 自动安装
if (typeof window !== 'undefined' && window.Vue) {
  window.Vue.use(toastPlugin)
}
// 导出模块
export default toastPlugin

```

-   app.vue

```
<template>
  <div id="app">
    <toast :msg = "'测试'" :isShow = "true"/>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>


```

-   main.js

```
import Vue from 'vue'
import App from './App.vue'

import Toast from './lib/toast.js'
Vue.use(Toast)

new Vue({
  el: '#app',
  render: h => h(App)
})

```
- package.json
```
{
  "name": "daoos-app-mounter",
  "description": "a mounter for daoos vue apps",
  "version": "1.0.0",
  "author": "liushouquan <liushouquan@daoos.com>",
  "main": "dist/toast.js",//更改
  "license": "MIT",
  "private": false,//更改
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --open --hot",
    "build": "cross-env NODE_ENV=production webpack --progress --hide-modules"
  },
  "dependencies": {
    "vue": "^2.5.11"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ],
  "devDependencies": {
    "babel-core": "^6.26.0",
    "babel-loader": "^7.1.2",
    "babel-preset-env": "^1.6.0",
    "babel-preset-stage-3": "^6.24.1",
    "cross-env": "^5.0.5",
    "css-loader": "^0.28.7",
    "file-loader": "^1.1.4",
    "node-sass": "^4.5.3",
    "sass-loader": "^6.0.6",
    "vue-loader": "^13.0.5",
    "vue-template-compiler": "^2.4.4",
    "webpack": "^3.6.0",
    "webpack-dev-server": "^2.9.1"
  },
  "keywords": [
    "vue",
    "toast"
  ],//更改
  "repository": {
    "type": "git",
    "url": "git+https://github.com/daoos/daoos-app-mounter"
  },//更改
  "homepage": "https://github.com/daoos/daoos-app-mounter"//更改
}

```
- wenpack.config.js
```
var path = require('path')
var webpack = require('webpack')

module.exports = {
  // entry: './src/main.js',
  entry: './src/lib/toast.js',
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: '/dist/',
    // filename: 'build.js',
    filename: 'toast.js',
    library: "vueToast",
    libraryTarget: "umd",
    umdNamedDefine: true
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'vue-style-loader',
          'css-loader'
        ],
      },
      {
        test: /\.scss$/,
        use: [
          'vue-style-loader',
          'css-loader',
          'sass-loader'
        ],
      },
      {
        test: /\.sass$/,
        use: [
          'vue-style-loader',
          'css-loader',
          'sass-loader?indentedSyntax'
        ],
      },
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: {
          loaders: {
            // Since sass-loader (weirdly) has SCSS as its default parse mode, we map
            // the "scss" and "sass" values for the lang attribute to the right configs here.
            // other preprocessors should work out of the box, no loader config like this necessary.
            'scss': [
              'vue-style-loader',
              'css-loader',
              'sass-loader'
            ],
            'sass': [
              'vue-style-loader',
              'css-loader',
              'sass-loader?indentedSyntax'
            ]
          }
          // other vue-loader options go here
        }
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        exclude: /node_modules/
      },
      {
        test: /\.(png|jpg|gif|svg)$/,
        loader: 'file-loader',
        options: {
          name: '[name].[ext]?[hash]'
        }
      }
    ]
  },
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js'
    },
    extensions: ['*', '.js', '.vue', '.json']
  },
  devServer: {
    historyApiFallback: true,
    noInfo: true,
    overlay: true
  },
  performance: {
    hints: false
  },
  devtool: '#eval-source-map'
}

if (process.env.NODE_ENV === 'production') {
  module.exports.devtool = '#source-map'
  // http://vue-loader.vuejs.org/en/workflow/production.html
  module.exports.plugins = (module.exports.plugins || []).concat([
    new webpack.DefinePlugin({
      'process.env': {
        NODE_ENV: '"production"'
      }
    }),
    new webpack.optimize.UglifyJsPlugin({
      sourceMap: true,
      compress: {
        warnings: false
      }
    }),
    new webpack.LoaderOptionsPlugin({
      minimize: true
    })
  ])
}

```
- index.html
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>daoos-app-mounter</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="/dist/toast.js"></script>
  </body>
</html>
```
# 发布注意事项
如果是使用的淘宝镜像，需要更改为npm主仓库才能提交，操作结束后，再更改为淘宝镜像，具体步骤为：

1, 先修改为官方镜像
```
　 npm config set registry https://registry.npmjs.org/
```

2.添加用户
```
(base) daoos@daoos-GL62VR-7RFX:~/Documents/platform/daoos-app-mounter$ npm adduser
Username: daoos
Password: 
Email: (this IS public) liushouquan@daoos.com
Logged in as daoos on https://registry.npmjs.org/.
```
3.发布对应模块
```
(base) daoos@daoos-GL62VR-7RFX:~/Documents/platform/daoos-app-mounter$ npm publish
npm notice 
npm notice 📦  daoos-app-mounter@1.0.0
npm notice === Tarball Contents === 
npm notice 1.2kB  package.json       
npm notice 72B    .babelrc           
npm notice 147B   .editorconfig      
npm notice 211B   index.html         
npm notice 344B   README.md          
npm notice 2.6kB  webpack.config.js  
npm notice 6.3kB  dist/toast.js      
npm notice 52.1kB dist/toast.js.map  
npm notice 615B   src/App.vue        
npm notice 6.8kB  src/assets/logo.png
npm notice 310B   src/lib/toast.js   
npm notice 896B   src/lib/toast.vue  
npm notice 151B   src/main.js        
npm notice === Tarball Details === 
npm notice name:          daoos-app-mounter                       
npm notice version:       1.0.0                                   
npm notice package size:  24.0 kB                                 
npm notice unpacked size: 71.7 kB                                 
npm notice shasum:        fbc4cfab06fbf0757094871c2c65e80359bc2a6b
npm notice integrity:     sha512-9soksSBwHfi+Z[...]KqcZh5Okv+tOA==
npm notice total files:   13                                      
npm notice 
+ daoos-app-mounter@1.0.0

```
5, 修改回下载仓库为淘宝镜像（淘宝会在找不到新镜像时自动同步）
```
　 npm config set registry http://registry.npm.taobao.org/
```
# 组件使用
## 引用
![引用模块](https://oscimg.oschina.net/oscnet/up-35d9c6a0bf6ea7156026820d4a14d6fcef8.png "引用模块")
## 注册
![注册模块](https://oscimg.oschina.net/oscnet/up-4595ddeeeec3d27aa8a211659ab6d68e9cd.png "注册模块")
## 使用
![使用组件](https://oscimg.oschina.net/oscnet/up-56816e67ae9fe8644a8847911e79b798425.png "使用组件")
