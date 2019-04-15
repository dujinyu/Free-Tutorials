[TOC]

# axios的使用

[axios中文说明](<https://www.kancloud.cn/yunye/axios/234845>)

在实际的项目中使用时，有一些写法，可以把请求的URL集中在一个文件中，方便统一管理。

## 安装

> npm install axios

## 简单配置

案例代码如下：

```javascript
import Axios from "axios"
import qs from "qs"

const baseURL = "http://xxx.com"

// 创建一个axios实例
const service = Axios.create({
    baseURL: "",
    timeout: 1000,
    headers: {}
})

// 封装get请求
const get = (url) => service({
    method: "get",
    url: url
})

// 封装post请求
const post = (url, data) => service({
    method: "post",
    url: url,
    data: qs.stringify(data)
})

// 获取轮播图图片
export const getSwiperImg = () => get("/getSwiperImg")

// 获取研究方向信息
export const getResearchInfo = () => get("/getResearchInfo")

// 获取研究生信息
export const getStdInfo = () => get("/getStdInfo")

// 获取设备器材信息
export const getEquipmentInfo = () => get("/getEquipmentInfo")

// 获取tradition list信息
export const getTraditionList = () => get("/getTraditionList")

// 获取近期项目数据
export const getProjectList = () => get("/getProjectList")

// 获取教师信息
export const getTeacherInfo = () => get("/getTeacherInfo")

// signup 加入我们
export const signUp = (data) => post("/sign", data)

```

上述代码中，先创建一个`service`服务，然后封装`get`和`post`请求，代码就可以大量复用，统一管理。还可以添加请求拦截器和响应拦截器等。当URL发生改变时，不用去组件中寻找，直接在`api`文件中找到对应的请求更改即可。

## 全局配置

全局配置，即将`axios`挂在到`vue`对象上，成为默认的方法。

案例代码如下：

```javascript
// 全局引入axios并挂载在vue上
import Axios from "axios"
Vue.prototype.$axios = Axios


// 单个vue文件中的使用方法
this.$axios.get(url)
this.$axios.post(url, data)
```

或者在特定的js文件中封装`get`和`post`请求，并添加拦截器、`default config`等，代码如下：

```javascript
import Axios from 'axios'
import { querystring } from "vux"

Axios.defaults.timeout = 10000;
// Axios.defaults.baseURL = "http://www.xxx.com";
// Axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';

// 请求拦截器
Axios.interceptors.request.use(
    function (config) {
        // 在发送请求之前干点什么
        return config
    },
    function (error) {
        // 对错误请求做点什么
        console.log("请求错误，被拦截！")
        return Promise.reject(error)
    }
)
// 响应拦截器
Axios.interceptors.response.use(
    function (response) {
        // 对响应数据做点什么
        return response
    },
    function (error) {
        // 对响应错误做点什么
        return Promise.reject(error)
    }
)

// 封装get请求
export function get(url) {
    return new Promise((resolve, reject) => {
        Axios.get("/api" + url)
        .then(res => {
            resolve(res)
        })
        .catch(err => {
            reject(err)
        })
    })
}

// 封装post请求
export function post(url, data={}) {
    return new Promise((resolve, reject) => {
        Axios.post("/api" + url, querystring.stringify(data))
        .then(res => {
            resolve(res)
        })
        .catch(err => {
            reject(err)
        })
    })
}
// 此处的api是处理跨域问题是，目标地址rewrite的部分
```

经过上述封装，然后在`main.js`中引入，代码如下：

```javascript
//全局挂载axios，不用挨个引入
import { get, post } from '@/common/axios'
Vue.prototype.$get = get
Vue.prototype.$post = post

// 单个vue文件中的使用方法
this.$get(url)
this.$post(url, data)
```

全局配置的优点就是不用再每个`vue`文件中`import`一下需要的请求接口，可以直接写。

缺点也很明显，不能统一管理`url`。

# mockjs的使用

[mock官网](<http://mockjs.com/>)

mock用来拦截所有的`http`请求，不论是否使用`Mock.mock()`。

## 安装

> npm install mockjs --save-dev

## 引入mock

首先在`main.js`中引入我们的`mock`的文件，代码如下：

```javascript
// 引入mockjs,require的路径就是mock的js文件
require('./mock/index.js')
```

## 简单配置

案例代码如下：

```js
// 引入mock
const Mock = require("mockjs");
// 引入mock中的random方法
const Random = Mock.Random;

const graphicVerificationImage = function() {
    let img = Random.dataImage("70x40", "mock的图片！")
    return {img: img}
}

const loginWithPasswd = function(data) {
    console.log(data)
    return {
        status: "success"
    }
}

const loginWithSMSCode = function(data) {
    console.log(data)
    return {
        status: "success"
    }
}
const getSMSCode = function() {
    return {
        status: "success"
    }
}

const signUp = function(data) {
    return {
        status: "success"
    }
}

const forgetPwd = function(data) {
    return {
        status: "success"
    }
}

const rename = function(data) {
    return {
        status: "success"
    }
}

const repwd = function(data) {
    return {
        status: "success"
    }
}

const step1 = function() {
    return {
        status: "success"
    }
}

const step2 = function(data) {
    return {
        status: "success"
    }
}

const step3 = function(data) {
    return {
        status: "success"
    }
}

const contractNum = function() {
    return {
        "contractNum": [10001, 10002, 10003, 10004, 10005, 10006]
    }
}

const details = function() {
    return {
        "status": "success",
        "contract_num": "111",
        "tax_payer": "222",
        "company_name": "333",
        "legal_person": "444",
        "principal_person": "555",
        "principal_person_phone": "666",
        "company_addr": "777",
        "contract_date": "888",
        "expiry_time": "999",
        "purcha_num": "101010",
        "legal_person_phone": "111111"
    }
}

const addContract = function(data) {
    return {
        status: "success"
    }
}

const Logout = function() {
    return {
        status: "success"
    }
}

// 发起拦截相应路由的请求
// 写法为Mock.mock(url, method, function)或者Mock.mock(url, method, object)

Mock.mock("/getVerificationCode", "get", graphicVerificationImage)
Mock.mock("/purchase/login/0", "post", loginWithPasswd)
Mock.mock("/purchase/login/1", "post", loginWithSMSCode)
Mock.mock(RegExp("/purchase/getSMSCode/" + ".*"), "get", getSMSCode)
Mock.mock("/purchase/signUp", "post", signUp)
Mock.mock("/purchase/forgetpasswd", "post", forgetPwd)
Mock.mock("/purchase/modifyusername", "post", rename)
Mock.mock("/purchase/modifypasswd", "post", repwd)
Mock.mock("/purchase/modifyuserid/step1", "get", step1)
Mock.mock("/purchase/modifyuserid/step2", "post", step2)
Mock.mock("/purchase/modifyuserid/step3", "post", step3)
Mock.mock("/purchase/getContractNum", "get", contractNum)
Mock.mock(RegExp("/purchase/getContract/" + ".*"), "get", details)
Mock.mock("/purchase/addContract", "post", addContract)
Mock.mock("/purchase/logout", "get", Logout)
```

## 存在的问题

1. mock要么就拦截全部请求，要么就不拦截，不能做到只拦截特定的请求。我们在实际开发中，前端会预留一些接口，后端只写了其中的一部分，另外一部分我们可以用mock拦截，返回一些静态的假数据。这样，我们就既可以访问真实的接口，请求到后端的数据，又可以拦截部分请求，返回假数据。

2. 一旦使用了mock，就算将`main.js`中对mock的引入给注销了，也不能发起正常的http请求。这个很奇怪。

# vuex的使用

## 安装

> npm install vuex

## 引入vuex

在`main.js`中引入，代码如下：

```js
// 引入路由和store，并挂载到new Vue上
import router from "./router"
import store from "./store"  
// 这里的store的名字要和“store/index.js中export default的名字相同，这是import的问题”

new Vue({
  el: '#app',
  router,   
  store,  // 要挂载在原型上
  render: h => h(App)
})
```

## 配置store

代码如下：

```js
// store/index.js
import Vue from "vue"
import Vuex from "vuex"

Vue.use(Vuex)

import mutations from "./mutations"
import actions from "./actions"

const state = {
    swiperImg: [],      // 首页轮播图信息
    researchInfo: [],   // 研究方向信息
    project: [],        // 近期项目信息
    tradition: [],      // 实验室传统信息
    teacher: [],        // 导师风采信息
    student: [],        // 学生信息
    equipment: [],      // 实验室器材信息
    signUpForm: [],      // 报名表单信息
    tabbarIndex: 0
}

const store = new Vuex.Store({
    // strict: true,  // 严格模式，只允许mutation改变state数据
    state,
    mutations,
    actions
})

export default store

// store/mutaions.js
const mutations = {
    "RECORD_TABBARINDEX" (state, index) {
        state.tabbarIndex = index
    },
    "RECORD_SWIPERIMAGE" (state, imgList) {
        state.swiperImg = imgList
    },
    "SET_STDLIST" (state, stdList) {
        state.student = stdList
    },
    "SET_EQUIPMENTLIST" (state, equipmentList) {
        state.equipment = equipmentList
    },
    "SET_TRADITIONLIST" (state, traditionList) {
        state.tradition = traditionList
    },
    "SET_PROJECTLIST" (state, projectList) {
        state.project = projectList
    },
    "SET_TEACHERLIST" (state, teacherList) {
        state.teacher = teacherList
    }
}

export default mutations

// store/actions.js
const actions = {
    RECORD_TABBARINDEX(context, index) {
        context.commit("RECORD_TABBARINDEX", index)
    },
    RECORD_SWIPERIMAGE({ commit }, imgList) {
        commit("RECORD_SWIPERIMAGE", imgList)
    },
    // { commit }是es6的语法，叫做解构。 
    SET_STDLIST({ commit }, stdList) {
        commit("SET_STDLIST", stdList)
    },
    SET_EQUIPMENTLIST({ commit }, equipmentList) {
        commit("SET_EQUIPMENTLIST", equipmentList)
    },
    SET_TRADITIONLIST({ commit }, traditionList) {
        commit("SET_TRADITIONLIST", traditionList)
    },
    SET_PROJECTLIST({ commit }, projectList) {
        commit("SET_PROJECTLIST", projectList)
    },
    SET_TEACHERLIST({ commit }, teacherList) {
        commit("SET_TEACHERLIST", teacherList)
    }
}

export default actions

```

`state`, `mutations`, `actions`写在一个文件中显得臃肿，就分成多个文件，方便管理。

想要改变`state`的数据，需要`mutations`来改变，使用`actions`是允许异步操作。操作代码入下：

```js
// 在vue文件中，操作mutations
this.$store.commit("RECORD_TABBARINDEX", index)

// 在vue文件中，操作actions
this.$store.dispatch("RECORD_TABBARINDEX", index)
```

## 刷新浏览器，vuex数据丢失问题

**解决方案：**监听浏览器刷新事件，在刷新前将`store.state`中的数据存入`sessionStorage`中，在刷新完毕，再在页面加载时，将数据取出来，放入`store.state`

方案代码如下：

```js
// 写在App.vue的script中
import { getSessionStore, setSessionStore } from "@/common/js/localOrSessionStorage.js"
export default {
    name: 'App',
    components: {},
    methods: {
        getDataBeforeLoading() {
            // 在页面加载时读取sessionStorage里的状态信息， 因为页面加载时，会自动执行created中的内容
            let tmpStore = getSessionStore("store")
            if (tmpStore) {
                this.$store.replaceState(Object.assign({}, this.$store.state, tmpStore))
            }
        },
        storeDataWhenRefresh() {
            // 在页面刷新时将vuex里的信息保存到sessionStorage里
            window.addEventListener("beforeunload", ()=>{
                setSessionStore("store", this.$store.state)
            })
        }
    },
    created() {
        this.getDataBeforeLoading()
        this.storeDataWhenRefresh()
    }
}

// @/common/js/localOrSessionStorage.js中的文件
// 存入sessionstorage
export const setSessionStore = (name, content) => {
    if (!name) return
    if (typeof content !== "string") {
        content = JSON.stringify(content)
    }
    window.sessionStorage.setItem(name, content)
}

// 获取sessionstorage数据
export const getSessionStore = (name) => {
    if (!name) return
    try {
        return JSON.parse(window.sessionStorage.getItem(name))
    } catch (err) {
        return window.sessionStorage.getItem(name)
    }
    // return window.sessionStorage.getItem(name)
}
```

# 样式初始化和scss的使用

## 样式初始化

为了解决HTML标签自带样式的问题，我们需要进行样式初始化。

**初始化方法：**在`main.js`中引入样式初始化的`.css`文件。

代码如下：

```js
//引入初始化样式和防止边框在高清手机显示问题的边框样式,初始化样式，直接百度
import '@/common/css/reset.css'
import '@/common/css/border.css'
```

## scss的使用

scss的好处是可以抽象出公共样式文件，从而大量复用。例如：背景色，字体样式，布局样式等。通过定义变量和`mixin`的方式实现。

# 移动端处理点击延时

在`main.js`中添加如下代码：需要`npm install fastclick`

```js
//fastclick：处理移动端click事件300毫秒延迟
import FastClick from 'fastclick'
FastClick.attach(document.body)

```

# 浏览器tab栏图标设置

**方法：**

1. 在`index.html`文件同级下放置一个名为`favicon.ico`的文件

2. 在`index.html`的`head`标签下添加

   ```html
   <link rel="shortcut icon" href="./favicon.ico">
   ```

3. 在`build/webpack.dev.conf.js`文件中的`plugins`下添加代码，如下：

   ```js
   plugins: [
       new webpack.DefinePlugin({
         'process.env': require('../config/dev.env')
       }),
       new webpack.HotModuleReplacementPlugin(),
       new webpack.NamedModulesPlugin(), // HMR shows correct file names in console on update.
       new webpack.NoEmitOnErrorsPlugin(),
       // https://github.com/ampedandwired/html-webpack-plugin
       new HtmlWebpackPlugin({
         filename: 'index.html',
         template: 'index.html',
         inject: true,
         favicon: './favicon.ico'  // 这里就是增加的代码
       }),
       // copy custom static assets
       new CopyWebpackPlugin([
         {
           from: path.resolve(__dirname, '../static'),
           to: config.dev.assetsSubDirectory,
           ignore: ['.*']
         }
       ])
     ]
   })
   ```

4. `npm run dev`一遍即可

# build打包后空白页面和图片找不到的问题

**原因：**打包后路径发生了变化，和原本js代码中的路径不同了。

**解决方案：**

## 1. 针对打包后空白页面的问题

在打包前，找到`config/index.js`文件，更改其中`build`下面的`assetPublicPath`。更改后代码如下：

```js
build: {
    // Template for index.html
    index: path.resolve(__dirname, '../dist/index.html'),

    // Paths
    assetsRoot: path.resolve(__dirname, '../dist'),
    assetsSubDirectory: 'static',
    assetsPublicPath: './',    // 更改了这里，比原本多一个点
	// 省略以下代码。。。。。。
}
```



## 2. 针对图片找不到的问题

打包后的静态文件统统放在`static`的文件夹中，与其搞来搞去相对路径，`../../`的搞来搞去，我们直接将图片等放在`src`同级的`static/images`下。在引入图片时，我们先给`static`设置别名alias，然后引用`"static/images/xx.jpg"`，这样打包后路径一定是对的。别名设置参见：[别名设置](#别名设置)

# 别名设置

在`build/webpack.base.conf.js`中，修改别名alias，代码如下：

```js
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
      'static': resolve("static")  
      // 给static添加了别名，写路径是直接static开头，就能找到它了。其他常用的路径也可以自行配置。
    }
  },
```



