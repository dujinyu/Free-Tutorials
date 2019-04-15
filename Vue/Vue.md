[TOC]

# axios的使用

[axios中文说明](<https://www.kancloud.cn/yunye/axios/234845>)

在实际的项目中使用时，有一些写法，可以把请求的URL集中在一个文件中，方便统一管理。

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













