# vue2+vue-router2+iview2实战

### 初始化vue配置

vue-cli初始化

### 安装vue-router

`npm install vue-router`



### 安装ivew

`npm install ivew --save`

### 如果有异步加载模块需要安装style-loader

`npm install style-loader -D`



### 配置web-config.js

```javascript
  {
    test: /iview.src.*?js$/,
      loader: 'babel'
  },
  {
      test: /\.css$/,
        loader: 'style-loader!css-loader'
  },
  {
    test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
      loader: 'file-loader'
  },
```

### 使用iview和vue-router

```javascript
import Vue from 'vue'
import App from './App.vue'
import VueRouter from 'vue-router';
import routeConfig from './router.js'; // 路由列表
import iView from 'iview';
import 'iview/dist/styles/iview.css'; // 使用 CSS
Vue.use(VueRouter);
Vue.use(iView);


const router = new VueRouter(routeConfig);

new Vue({
	el: '#app',
	router: router,
	render: h => h(App)
});
```