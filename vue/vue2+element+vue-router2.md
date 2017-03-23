vue全家桶安装指导

### 安装vue-cli

```
npm install -g vue-cli
```

### 1通过vue-cli模版创建vue项目

```
vue init webpack-simple 你的项目名
```

### 2进入项目安装依赖

```
cd 你的项目文件夹 
npm install 
```

### 3运行项目

```
npm run dev
```

现在可以查看结果

浏览器输入

```
localhost:8080    
```

查看结果

------

# 整合elementui

### 4整合安装elementui

```
npm install element-ui -S
```

### 5安装style-loader

```
npm install style-loader -S
```

### 6在webpack.config中配置匹配字体的loader

```javascript
   module: {
       rules: [{
            test: /\.vue$/,
            loader: 'vue-loader',
            options: {
                loaders: {}
                // other vue-loader options go here
            }
        }, {
            test: /\.js$/,
            loader: 'babel-loader',
            exclude: /node_modules/
        }, {
            test: /\.css$/,
            loader: 'style-loader!css-loader'
        }, {
            test: /\.(eot|svg|ttf|woff|woff2)(\?\S*)?$/,
            loader: 'file-loader'
        }, {
            test: /\.(png|jpg|gif|svg)$/,
            loader: 'file-loader',
            options: {
                name: '[name].[ext]?[hash]'
            }
        }]
},
```
### 和elementui的整合完成

使用elemntUI

```javascript
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-default/index.css'

Vue.use(ElementUI);
```

------

# 整合vue-router

- ### 安装

  ```
  npm install vue-router -S
  ```

  ### 使用vue-router

  ```javascript
  import VueRouter from 'vue-router';
  Vue.use(VueRouter);
  //经过测试这里不能写成require（./router）的形式，只能这么写才能生效，暂时不知道为啥
  const router = new VueRouter(routes);
  new Vue({
  	router,
  	render: h => h(App)
  }).$mount('#app');
  ```

  ### router.js中配置如下

  ```javascript
  //这个路由表是从app.vue开始算起的，如果加载后的vue文件包含子路由，都需要配置在children中
  export default {
  	routes: [{
  		path: '/index',
  		component: resolve => require(['./views/index.vue'], resolve),
  		children: [{
  			path: 'content',
  			
  			components: {
  			//这个是因为
  				leftNav: resolve => require(['./views/left-nav.vue'], resolve),
  				mainContent: resolve => require(['./views/main-content.vue'], resolve),
  				breadCrumb: resolve => require(['./views/bread-crumb.vue'], resolve)
  			}
  		}]
  	}, {
  		path: '/job',
  		component: resolve => require(['./views/job/job-list.vue'], resolve)
  	}, {
  		path: '/menu',
  		component: resolve => require(['./views/left-nav.vue'], resolve),
  	}]
  }
  ```

  **解释：**

  routes 相当于匹配的是第一个router-view

  如果第一个router-view中的页面中还有嵌套router-view则需要配置在children中

  经过测试**component和components二者只能存在一个**

  components表示的是一个页面存在多个router-view，通过name值进行指定

  例如

  ```vue
  leftNav: resolve => require(['./views/left-nav.vue'], resolve),
  对应页面中的
  <router-view name="leftNav"></router-view>
  ```

  其中：

  ```javascript
   resolve => require(['./views/left-nav.vue'], resolve),
  ```

  这个是懒加载页面，优化页面性能，**es6语法，**

  简单的理解为就是类似

  ```javascript
  function(resolve){
  	return require(['./views/left-nav.vue'], resolve);
  }
  ```

  ​

  ### app.vue如下

  ```vue
  <template>
  	<div id="app">
  		{{welcome}}
  		<p>
  			<router-link to="/index/content">跳转首页</router-link>
  			<router-link to="/job">跳转列表</router-link>
  			<router-link to="/menu">caidan</router-link>
  		</p>

  		<!-- 路由出口 -->
  		<!-- 路由匹配到的组件将渲染在这里 -->
  		<router-view></router-view>

  	</div>
  </template>
  <script>
  	export default {
  		data() {
  			return {
  				welcome: '欢迎光临，这是app'
  			};
  		}
  	};
  </script>
  ```

  ### index.vue如下

  ```vue
  <template>
  	<div>
  		<span>{{msg}}</span>
  		<router-view name="leftNav"></router-view>
  		<router-view name="mainContent"></router-view>
  		<router-view name="breadCrumb"></router-view>
  	</div>

  </template>

  <script>
  	export default {
  		data() {
  			return {
  				msg: '5555'
  			}
  		}
  	}
  </script>
  ```