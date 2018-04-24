# blog-vue

> A blog by vuejs

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For detailed explanation on how things work, checkout the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

> 功能

图片上传  相册  收藏分享点赞评论  音乐列表  文章  排行榜

header footer  功能区

> 第三方依赖

基础: Vue
组件库: mintui
图标: fontawesome
语法: scss, es6
路由: vue-router
Ajax: axios
状态管理: Vuex
Mock服务: mock.js, json-server
工具: moment


## Vuex的使用

> 保存登录信息

登录成功存入state,并缓存至storage
登出清楚state和storage
使用获取state,state为空则获取storage

## axios的使用

> 全局配置

axios.default
