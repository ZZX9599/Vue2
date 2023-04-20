# Vue2

# 0:环境准备

## 1:安装 nvm

nvm 即 (node version manager)，好处是方便切换 node.js 版本

安装注意事项

1：要卸载掉现有的 nodejs

2：提示选择 nvm 和 nodejs 目录时，一定要避免目录中出现空格

3：选用【以管理员身份运行】cmd 程序来执行 nvm 命令

4：首次运行前设置好国内镜像地址

```
nvm node_mirror http://npm.taobao.org/mirrors/node/
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```

首先查看有哪些可用版本

```
nvm list available
```

输出

```
|   CURRENT    |     LTS      |  OLD STABLE  | OLD UNSTABLE |
|--------------|--------------|--------------|--------------|
|    18.7.0    |   16.16.0    |   0.12.18    |   0.11.16    |
|    18.6.0    |   16.15.1    |   0.12.17    |   0.11.15    |
|    18.5.0    |   16.15.0    |   0.12.16    |   0.11.14    |
|    18.4.0    |   16.14.2    |   0.12.15    |   0.11.13    |
|    18.3.0    |   16.14.1    |   0.12.14    |   0.11.12    |
|    18.2.0    |   16.14.0    |   0.12.13    |   0.11.11    |
|    18.1.0    |   16.13.2    |   0.12.12    |   0.11.10    |
|    18.0.0    |   16.13.1    |   0.12.11    |    0.11.9    |
|    17.9.1    |   16.13.0    |   0.12.10    |    0.11.8    |
|    17.9.0    |   14.20.0    |    0.12.9    |    0.11.7    |
|    17.8.0    |   14.19.3    |    0.12.8    |    0.11.6    |
|    17.7.2    |   14.19.2    |    0.12.7    |    0.11.5    |
|    17.7.1    |   14.19.1    |    0.12.6    |    0.11.4    |
|    17.7.0    |   14.19.0    |    0.12.5    |    0.11.3    |
|    17.6.0    |   14.18.3    |    0.12.4    |    0.11.2    |
|    17.5.0    |   14.18.2    |    0.12.3    |    0.11.1    |
|    17.4.0    |   14.18.1    |    0.12.2    |    0.11.0    |
|    17.3.1    |   14.18.0    |    0.12.1    |    0.9.12    |
|    17.3.0    |   14.17.6    |    0.12.0    |    0.9.11    |
|    17.2.0    |   14.17.5    |   0.10.48    |    0.9.10    |
```

建议安装 LTS（长期支持版）

```
nvm install 16.16.0
nvm install 14.20.0
```

执行 `nvm list` 会列出已安装版本

切换到 16.16.0

```
nvm use 16.16.0
```

切换到 14.20.0

```
nvm use 14.20.0
```

查看当前的Node.js版本

```
nvm -v
```

查看安装的所有Node.js版本

```
nvm ls
```

安装后 nvm 自己的环境变量会自动添加

但可能需要手工添加 nodejs 的 PATH 环境变量



新建文件夹，配置这个，把node_global加到path里面

```
npm config set prefix "E:\Tools\NVM\node_global"
npm config set cache "E:\Tools\NVM\node_cache"
```



## 2:检查 npm

npm 是 js 的包管理器，就类似于 java 界的 maven，要确保它使用的是国内镜像

检查镜像

```
npm get registry
```

如果返回的不是 `https://registry.npm.taobao.org/`，需要做如下设置

```
npm config set registry https://registry.npm.taobao.org/
```



## 3:搭建前端服务器

新建一个保存项目的 client 文件夹，进入文件夹执行

```
npm install express --save-dev
```

修改 package.json 文件

```json
{
  "type": "module",
  "devDependencies": {
    "express": "^4.18.1"
  }
}
```

其中 devDependencies 是 npm install --save-dev 添加的

编写 main.js 代码

```js
import express from 'express'
const app = express()

app.use(express.static('./'))
app.listen(7070)
```

执行 js 代码（运行前端服务器）

```
node main.js
```

# 1:FetchAPI

## 1.1:介绍

使用Vue之前，先学习一下Fetch API ，便于后面学习`Vue`的`Axios`组件

Fetch API 可以用来获取远程数据，它有两种方式接收结果：同步方式与异步方式

## 1.2:语法

```js
// 返回的是Promise对象，但是Promise对象不是最终结果
let promise = fetch(url, options) 
```

## 1.3:同步方式

```js
// 在Promise前加上await，就代表是同步执行
const 结果 = await Promise
```

await 关键字必须在一个标记了 async 的 function 内来使用

后续代码不会在结果返回前执行

## 1.4:异步方式

```js
//使用Promise.then就是异步方式
Promise.then(结果 => { ... })         
```

后续代码不必等待结果返回就可以执行

## 1.5:同步实例

假设服务器上有 students.json 文件

现在用 fetch api 获取这些数据，使用同步方式获取数据

```js
<script>
    async function findStudents() {
        try {
        	//参数二是一些发送的配置信息，请求头等
            //不写的话默认是get请求
        	//使用同步方式，得到的结果是响应对象【Promise对象】
            //后续代码不会执行
            
        	const promise = await fetch('student.json')
        	//使用响应对象的.json来获取响应体，按json格式转换为js数组
        	//注意：返回的还不是js数组，目前还是一个Promise，所以还要加await
            
        	const array = await promise.json();
        }catch (e) {
            console.log(e);
        }
</script>
```

fetch('students.json') 内部会发送请求，但响应结果不能立刻返回

因此 await 就是等待响应结果返回

其中 resp.json() 解析数据也不是立刻能返回结果，它返回的也是 Promise 对象

也要配合 await 取结果

以上的思想理解即可，并不会要求Java程序员在前端代码写

## 1.6:异步实例

```js
<script>
    fetch('students.json')
        .then( resp => resp.json() )
        .then( array => {
        	//后续操作
        })
        .catch( e => console.log(e) )
</script>
```

上面的 resp 就是返回的 Promise 对象，意思就是返回的对象只要是Promise对象

就可以使用 then 或者 wait

## 1.7:跨域问题

目前我这里前端服务器是 `localhost:7070`

![image-20220814105448882](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20220814105448882.png)

浏览器会针对`Feach` 的请求做出一种策略，叫做同源检查

发送到前端服务器的时候，浏览器发现发送方和响应方都是 `http://localhost:7070`属于同源，就会允许

发送到后端的服务器的时候，浏览器发现发送方是`http://localhost:7070`，但是响应属于

`http://localhost:8080`，后端返回数据到浏览器【注意：还未到达网页】，浏览器针对Feach做出检查

发现不同源，就`没有通过浏览器的同源检查策略`，浏览器就会对这个响应弃之不用



实际上，发送Fetch请求的时候，发现不同源，就会在`请求头包含一个Origin`的头，包含发送源的信息，申

请获得响应，意思是告诉Tomcat，我是7070端口的，我想要你8080的数据，服务器接收到请求之后，可以

做出选择，`允许访问的话，相应的数据头应该包含一个头`，`不允许的话，就不做任何处理`，也就是不加头，这

个头是 `Access-Control-Allow-Origin`，这个头的值代表哪些请求可以使用这个响应



只要协议、主机、端口之一不同，就不同源，例如

http://localhost:7070/a 和 https://localhost:7070/b 就不同源，协议不同



同源检查是`浏览器客户端的特殊行为`，而且只针对 fetch、XMLHttpRequest【AJAX】请求

如果是其它客户端，例如 java http client，postman，它们是不做同源检查的

通过表单提交、浏览器直接输入 url 地址这些方式发送的请求，也不会做同源检查

更多相关知识请参考：[跨源资源共享（CORS） - HTTP | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)



## 1.8:请求响应头解决

![image-20220814144040703](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20220814144040703.png)

fetch 发现请求是跨域的话，会携带一个 Origin 头，代表【发请求的资源源自何处】，目标通过它就能辨别

是否发生跨域，我们的例子中：student.html 发送 fetch 请求，告诉 tomcat，我源自 localhost:7070

目标Tomcat资源通过返回 Access-Control-Allow-Origin 头，告诉浏览器【允许哪些源使用此响应】

我们的例子中：tomcat 返回 fetch 响应，告诉浏览器，这个响应只允许源自 localhost:7070 的资源使用

浏览器发现存在这个头的话，就不会把Tomcat的响应弃之不用，会允许localhost:7070的资源使用



服务端可以在接口上使用@CrossOrigin("http://localhost:7070")

也可以配置SpringBoot全局的跨域

实现WebMvcConfigure接口，实现addCorsMappings方法

```java
public void addCorsMappings(CorsRegistry registry) {
   
    registry.addMapping("/**")
            .allowedOriginPatterns("*")
            .allowedMethods("POST", "GET", "PUT", "OPTIONS", "DELETE")
            .allowCredentials(true)
            .allowedHeaders("*")
            .maxAge(3600);
}
```

这样的话接口返回的数据就可以被加上Access-Control-Allow-Origin，内容是允许源自 localhost:7070 的资源

使用，浏览器拿到数据之后发现存在Access-Control-Allow-Origin并且允许源自 localhost:7070 的资源使用，

浏览器就允许通过了，就可以解析渲染了

## 1.9:代理解决

![image-20220814161532141](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20220814161532141.png)

- 上图粉红色的是我们在`前端的服务器`加的一个代理插件
- 之前的浏览器发送的请求，部分请求7070端口，部分请求8080端口
- 加入了插件之后，就好像`生成一个新的约定，请求涉及到跨域，就从我7070的代理访问`
- 由我`7070这个服务器去找8080`拿，8080返回给7070【由于`不是浏览器的行为，就不存在跨域`】
- 再由`7070返回给浏览器`，浏览器这个时候就不涉及跨域了
- 浏览器都不知道7070的请求去了8080，就好像骗了浏览器
- 但是`前端服务器怎么区分`是走7070还是8080呢
- 可以通过浏览器前缀路径
- 例如找Tomcat的都加上api开头
- 带上了api的，就代表要走代理
- 加入插件就不记录了



这个时候`fetch`代码可以改为

```js
const resp = await fetch('http://localhost:7070/api/students')
```

因为现在浏览器以为是访问的是本地的了，不会跨域

后端的代码也可以去掉@CrossOrigin了



# 2:模块化和解构

## 2.1:单个导出

```js
export const a = 10;
export let b = 20;
export function c() {
    console.log('c');
}
```

## 2.2:一起导出

```js
const a = 10;
let b = 20;
function c() {
    console.log('c')
}
export {a,b,c}
```

## 2.3:默认导出

**导出 default，只能有一个**

```js
export const a = 10;
export let b = 20;
export function c() {
    console.log('c')
}
export default b;
```

## 2.4:单个导入

**注意：要使用导入的语法，type必须为module**

```js
<script type="module">
    import {a} from '/1.js'
</script>
```

## 2.5:一起导入

```js
import * as module from '/1.js'
console.log(module.a)		// 输出10
console.log(module.b)		// 输出20
module.c()	
```

## 2.6:默认导入

**注意：导入默认不用加{}**

```js
//这个x代表1.js默认导出的，而且不用加{}
import x from '/1.js'
console.log(x)			
```



## 2.7:[] 解构

注意：`[]`一般是用于数组

1：用在声明变量时

```js
let arr = [1,2,3];

let [a, b, c] = arr;	// 结果 a=1, b=2, c=3
```

2：用在声明参数时

```js
let arr = [1,2,3];

function test([a,b,c]) {
    console.log(a,b,c) 	// 结果 a=1, b=2, c=3
}

test(arr);				
```

## 2.8:{} 解构

注意：`[]`一般是用于对象

1：用在声明变量时

```js
let obj = {name:"张三", age:18};

let {name,age} = obj;	// 结果 name=张三, age=18
```

2：用在声明参数时

```js
let obj = {name:"张三", age:18};

function test({name, age}) {
    console.log(name, age); // 结果 name=张三, age=18
}

test(obj)
```

# 3:Vue基础

## 3.1:创建项目和准备

确保已经安装了Node.js 和 Vue 脚手架

安装脚手架：npm install -g @vue/cli

使用 -g 参数表示全局安装，这样在任意目录都可以使用 vue 脚本创建项目

使用 `Vue UI`图形化创建项目

devtools 插件网址：https://devtools.vuejs.org/guide/installation.html

使用 npm run serve 即可启动项目

### 1:修改端口

前端服务器默认占用了 8080 端口，需要修改一下

文档地址：[DevServer | webpack](https://webpack.js.org/configuration/dev-server/#devserverport)

打开 vue.config.js 添加

![image-20221023161431169](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221023161431169.png)



### 2:添加代理

为了避免前后端服务器联调时， fetch、xhr 请求产生跨域问题，需要配置代理

文档地址：[DevServer | webpack](https://webpack.js.org/configuration/dev-server/#devserverport)

打开 vue.config.js 添加

![image-20221024170551789](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024170551789.png)

这时候访问 http://localhost:7070/api/students  

实际上响应的是后台的http://localhost:8080/api/students

### 3:项目结构

![image-20221023162059842](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221023162059842.png)

main.js：Vue项目的入口

App.vue：根组件

components：可重用组件

router：路由跳转

store：Vuex数据共享的

views：视图组件，学生的，教师的，主要是分类



以后还会添加

api - 跟后台交互，发送 fetch、xhr 请求，接收响应



## 3.2:Vue基本知识

### 1:Vue组件

Vue组件文件以 .vue 结尾，每个组件由三部分组成

```vue
<template></template>

<script></script>

<style></style>
```

template 模板部分，由它生成 html 代码

script 代码部分，控制template模板的数据来源和行为

style 样式部分，一般不关心，写上scope代表只作用于当前组件



入口组件是 App.vue

先删除原有代码，来个 Hello  World 例子

```vue
<template>
  <div>
    <h1>{{msg}}</h1>
  </div>
</template>

//约定，Vue的组件的Script必须默认导出一个 options对象【约定】
//options就可以给上面的模板提供数据和方法
<script>
export default {
  data(){
    return{
      msg:'你好'
    }
  }
}
</script>
```

解释

export default 导出组件对象，供 `main.js` 导入使用

这个对象有一个 data 方法，返回一个**对象**，给 template 提供数据

{{}}  在 Vue 里称之为插值表达式，用来**绑定** data 方法返回的**对象**属性

**绑定**的含义是数据发生变化时，页面显示会同步变化，{{}} 放在标签之间

使用`this.$refs`可以拿到所有取了名字【ref属性】的组件

使用`this.$refs.组件name`可以拿到所有该名字的组件



### 2:文本插值

```vue
<template>
    <div>
        <h1>{{ name }}</h1>
        <h1>{{ age > 60 ? '老年' : '青年' }}</h1>
    </div>
</template>
<script>
const options = {
    data() {
        return { name: '张三', age: 70 };
    }
};
export default options;
</script>
```

{{}}  里只能绑定一个属性，绑定多个属性需要用多个 `{{}}` 分别绑定

template 内只能有一个根元素

插值内可以进行简单的表达式计算



### 3:属性绑定

```vue
<template>
    <div>
        <div><input type="text" v-bind:value="name"></div>
        <div><input type="date" v-bind:value="birthday"></div>
        <div><input type="text" :value="age"></div>
    </div>
</template>
<script>
const options = {
    data() {
        return { name: '王五', birthday: '1995-05-01', age: 20 };
    }
};
export default options;
</script>
```

简写方式：可以省略 v-bind 只保留冒号

v-bind 是用在给属性赋值的，而 {{}} 是用在标签之间的



### 4:事件绑定

```vue
<!-- 事件绑定 -->
<template>
    <div>
        <div><input type="button" value="点我执行m1" v-on:click="m1"></div>
        <div><input type="button" value="点我执行m2" @click="m2"></div>
        <div>{{count}}</div>
    </div>
</template>
<script>
const options = {
    data:() {
        return { count: 0 };
    },
    methods: {
        m1() {
            this.count ++;
            console.log("m1")
        },
        m2() {
            this.count --;
            console.log("m2")
        }
    }
};
export default options;
</script>
```

简写方式：可以把 v-on: 替换为 @

在 methods 方法中的 this 代表的是 data 函数返回的数据对象



### 5:双向绑定

```vue
<template>
    <div>
        <div>
            <label for="">请输入姓名</label>
            <input type="text" v-model="name">
        </div>
        <div>
            <label for="">请输入年龄</label>
            <input type="text" v-model="age">
        </div>
        <div>
            <label for="">请选择性别</label>
            男 <input type="radio" value="男" v-model="sex">
            女 <input type="radio" value="女" v-model="sex">
        </div>
        <div>
            <label for="">请选择爱好</label>
            游泳 <input type="checkbox" value="游泳" v-model="fav">
            打球 <input type="checkbox" value="打球" v-model="fav">
            健身 <input type="checkbox" value="健身" v-model="fav">
        </div>
    </div>
</template>
<script>
const options = {
    data: function () {
        return { name: '', age: null, sex:'男' , fav:['打球']};
    },
    methods: {
    }
};
export default options;
</script>
```

用 v-model 实现双向绑定，即 

* javascript 数据可以同步到表单标签
* 反过来用户在表单标签输入的新值也会同步到 javascript 这边

双向绑定只适用于表单这种带【输入】功能的标签，其它标签的数据绑定，单向就足够了

复选框这种标签，双向绑定的 javascript 数据类型一般用数组



### 6:计算属性

```vue
<!-- 计算属性 -->
<template>
    <div>
        <h2>{{fullName}}</h2>
        <h2>{{fullName}}</h2>
        <h2>{{fullName}}</h2>
    </div>
</template>
<script>
const options = {
    data() {
        return { firstName: '三', lastName: '张' };
    },
    computed: {
        fullName() {
            console.log('进入了 fullName')
            return this.lastName + this.firstName;
        }
    }
};
export default options;
```

普通方法调用必须加 ()，没有缓存功能

计算属性使用时就把它当属性来用，不加 ()，有缓存功能

一次计算后，会将结果缓存，下次再计算时，只要数据没有变化，不会重新计算，直接返回缓存结果



### 7:条件渲染

```vue
<template>
    <div>
        <input type="button" value="获取远程数据" @click="sendReq()">
        <div class="title">学生列表</div>
        <div class="thead">
            <div class="row bold">
                <div class="col">编号</div>
                <div class="col">姓名</div>
                <div class="col">性别</div>
                <div class="col">年龄</div>
            </div>
        </div>
        <div class="tbody">
            <div class="row" v-if="students.length > 0">显示学生数据</div>
            <div class="row" v-else>暂无学生数据</div>
        </div>
    </div>
</template>
<script>
import axios from '../util/myaxios'
const options = {
    data: function() {
        return {
            students: []
        };
    },
    methods : {
        async sendReq() {
            const resp = await axios.get("/api/students");
            console.log(resp.data.data)
            this.students = resp.data.data;
        }
    }
};
export default options;
</script>
<style scoped>
</style>
```



### 8:列表渲染

```vue
<template>
    <div>
        <!-- <input type="button" value="获取远程数据" @click="sendReq()"> -->
        <div class="title">学生列表</div>
        <div class="thead">
            <div class="row bold">
                <div class="col">编号</div>
                <div class="col">姓名</div>
                <div class="col">性别</div>
                <div class="col">年龄</div>
            </div>
        </div>
        <div class="tbody">
            <div v-if="students.length > 0">
                <div class="row" v-for="s of students" :key="s.id">
                    <div class="col">{{s.id}}</div>
                    <div class="col">{{s.name}}</div>
                    <div class="col">{{s.sex}}</div>
                    <div class="col">{{s.age}}</div>
                </div>
            </div>
            <div class="row" v-else>暂无学生数据</div>
        </div>
    </div>
</template>
<script>
import axios from '../util/myaxios'
const options = {
    mounted: function(){
        this.sendReq()
    },
    data: function() {
        return {
            students: []
        };
    },
    methods : {
        async sendReq() {
            const resp = await axios.get("/api/students");
            console.log(resp.data.data)
            this.students = resp.data.data;
        }
    }
};
export default options;
</script>
<style scoped>
</style>
```

v-if 和 v-for 不能用于同一个标签

v-for 需要配合特殊的标签属性 key 一起使用【一般是一个唯一性的ID】

并且 key 属性要绑定到一个能起到唯一标识作用的数据上，本例绑定到了学生编号上

options 的 mounted 属性对应一个函数，此函数会在组件挂载后（准备就绪）被调用

可以在它内部发起请求，去获取学生数据



### 9:重用组件

重用组件一般会放在components里面

一般被重用的组件叫做子组件，引用这个组件的叫做父组件

子组件定义如下：

```vue
<template>
	<!-- class="button"，这个是style的样式，基于笔记篇幅没写出来 -->
	<!--:class="[type,size]" 这个type,size就会读取引用它的组件传递-->
    <div class="button" :class="[type,size]">
        <!--子组件内的slot就是一个插槽，便于放父组件内的元素填充-->
        a<slot></slot>b
    </div>
</template>
<script>
const options = {
    //自定义属性的名称，给父组件来自定义选择填充我这个组件已有的样式
    props: ["type", "size"]
};
export default options;
</script>
```

注意：省略了样式部分



在一个组件里使用其它的组件【重用组件】

```vue
<template>
    <div>
        <h1>重用组件</h1>
        <!--使用子组件-->
        <my-button type="primary" size="small">1</my-button>
        <my-button type="danger" size="middle">2</my-button>
        <my-button type="success" size="large">3</my-button>
    </div>
</template>
<script>
import MyButton from '../components/MyButton.vue'
const options = {
    //表示我这个组件要使用哪些子组件
    components: {
        //这里是ES6新特性，实际上是"MyButton":MyButton
        MyButton
    }
};
export default options;
</script>
```

# 4:Axios



## 4.1:axios介绍

Axios 它的底层是用了 XMLHttpRequest（xhr）方式发送请求和接收响应

xhr 相对于之前讲过的 fetch api 来说功能更强大，但由于XML是比较老的 api

不支持 Promise，不支持同步异步，只能支持异步，所以axios 对 xhr 进行了封装

使之支持 Promise，也就是同步异步都支持了，并提供了对请求、响应的统一拦截功能

类似java的拦截器，可以在发送前，接收响应的时候做出处理

因为是XMLHttpRequest，所以会涉及到跨域



安装

```sh
npm install axios -S
```

-S是 --save 的简写，并将您正在安装的软件包添加到 package.json 中的依赖项中

如果您使用的是 npm 5 或更高版本，则完全没有必要，因为它是默认完成的



导入

```js
import axios from 'axios'
```



axios 源码默认导出一个对象，这里的 import 导入的就是它默认导出的对象

```js
import axios from 'axios'
```



## 4.2:axios发送各种请求

**基本方法**

| 请求                                                 | 备注   |
| ---------------------------------------------------- | ------ |
| axios.get(url，[config] )                            | :star: |
| axios.delete(url，[config] )                         |        |
| axios.head(url，[config] )                           |        |
| axios.options(url，[config] )                        |        |
| axios.post(url，[data]，[ config] )   data代表请求体 | :star: |
| axios.put(url，[data]，[config] )                    |        |
| axios.patch(url，[data]，[config] )                  |        |

config - 可选的参数，不需要则不写，选项对象、例如查询参数、请求头

data - 请求体数据、最常见的是 json 格式数据

注意：get、head请求无法携带请求体，这是浏览器的限制所致（xhr、fetch api 均有限制）

但是：options、delete 请求可以通过 config 中的 data 携带请求体



常见的 config 项有

| 名称            | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| baseURL         | 将自动加在 url 前面                                          |
| headers         | 请求头，类型为简单对象                                       |
| params          | 跟在 URL 后的请求参数，类型为简单对象或 URLSearchParams      |
| data            | 请求体，参数类型常用的这几种             1：简单对象     2：FormData【表单类型】3：URLSearchParams【URL参数】         4：File【文件上传】等 |
| withCredentials | 跨域时是否携带 Cookie 等凭证，默认为 false，跨域的时候默认是不会存储服务器返回的Cookie的，自然发送的时候也就不会发送了，服务器那边也要添加allowCredentials(true)，不然服务器就算返回，浏览器也不会使用 |
| responseType    | 响应类型，默认为 json                                        |

以上的配置可以配置在发请求的config里面，也可以自己配置自己的axios作为对象属性传递



### 1:基本get请求

后端服务器接口：@GetMapping("/api/a1")

```js
const resp = await axios.get('/api/a1');
```

![image-20221024173333357](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024173333357.png)



### 2:基本post请求

服务器接口：@PostMapping("/api/a2")

```js
const resp = await axios.post('/api/a2');
```

![image-20221024173545049](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024173545049.png)



### 3:post请求带请求头

请求头是在Config里面配置的

语法：axios.post(url，[data]，[ config] )   data代表请求体

```js
const resp = await axios.post('/api/a3', {} ,{
            	headers:{
                	Authorization:'abc'
                }
			});
```

![image-20221024174003820](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024174003820.png)



![image-20221024174113265](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024174113265.png)



### 4:post请求拼接URL

语法：axios.post(url，[data]，[ config] )   data代表请求体

```js
// 发送请求时携带查询参数拼接URL ?name=xxx&age=xxx
// 拼接字符串，特殊字符例如 &或者中文 会经过URL编码，不然不能发送
// 特殊字符需要自己处理
const name = encodeURIComponent('&&&');
const address = "北京";
const age = 18;
const resp = await axios.post(`/api/a4?name=${name}&address=${address}&age=${age}`);
```

![image-20221024174752247](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024174752247.png)



### 5:post携带param

配置 param 也是在 config 里面进行配置的【data代表的是请求体】

语法：axios.post(url，[data]，[ config] )   data代表请求体

```js
// 不想自己拼串、处理特殊字符、就用下面的办法
const resp = await axios.post('/api/a4', {}, {
            	 params: {
                     name:'&&&&',
            		age: 20
            	 }
             });
```



### 6:post用请求体发url参数

使用post的请求头发送数据，默认就是json参数

如果要使用拼接URL的参数，需要使用URLSearchParams对象的数据

如果要使用表单数据，需要使用FormData对象的数据

如果要使用JSON数据，无需处理，直接发送即可

```js
// 用请求体发数据，格式为 urlencoded【跟拼接字符串的方式一样】
// 默认传递的是JSON类型，我们要传递URL类型的话，要使用URLSearchParams();
const params = new URLSearchParams();
params.append("name", "张三");
params.append("age", 24)

const resp = await axios.post('/api/a4', params);
```

![image-20221024174920479](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024174920479.png)



### 7:post用请求体发表单数据

```js
// 用请求体发数据，格式为 multipart
const params = new FormData();
params.append("name", "李四");
params.append("age", 30);
onst resp = await axios.post('/api/a5', params);
```

![image-20221024175042152](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024175042152.png)



发现在请求体里面

![image-20221024175113647](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024175113647.png)



### 8:post用请求体发json数据

```js
 // 6. 用请求体发数据，默认格式为 json
const resp = await axios.post('/api/a5json', {
                name: '王五',
                age: 50
             });
```

![image-20221024175430296](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024175430296.png)

![image-20221024175454714](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024175454714.png)



除了json类型的需要加上@RequestBody

其他的请求参数类型都可以使用对应的参数或者实体类来接收



## 4.3:创建Axios实例

### 1:默认实例

```js
const _axios = axios.create(config);
```

axios 对象可以直接使用，但使用的是默认的设置

用 axios.create 创建的对象，可以覆盖默认设置，config 见下面说明

每次发送都是使用的axios的默认配置，我们可以配置axios的配置，不然每个请求都要改配置

我们可以修改配置，自己创建axios的对象，这样生成的对象就是我们自定义的配置了

语法：拿到原来的axios对象，调用create方法，返回新的axios对象  axios.create( {配置})



常见的 config 项有

| 名称            | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| baseURL         | 将自动加在 url 前面                                          |
| headers         | 请求头，类型为简单对象                                       |
| params          | 跟在 URL 后的请求参数，类型为简单对象或 URLSearchParams      |
| data            | 请求体，参数类型常用的这几种             1：简单对象     2：FormData【表单类型】3：URLSearchParams【URL参数】         4：File【文件上传】等 |
| withCredentials | 跨域时是否携带 Cookie 等凭证，默认为 false，跨域的时候默认是不会存储服务器返回的Cookie的，自然发送的时候也就不会发送了，服务器那边也要添加allowCredentials(true)，不然服务器就算返回，浏览器也不会使用 |
| responseType    | 响应类型，默认为 json                                        |

以上的配置可以配置在发请求的config里面，也可以自己配置自己的axios作为对象属性传递



### 2:创建实例

```js
const _axios = axios.create({ 
    baseURL: 'http://localhost:8080',
    withCredentials: true
});

await _axios.post('/api/a6set')
await _axios.post('/api/a6get')
```

事实上希望 xhr 请求不走代理，可以用 baseURL 统一修改，因为代理多了一些环节，效率降低

希望跨域请求携带 cookie，需要配置 axios 的 config 里面的  withCredentials: true

服务器也要配置 allowCredentials = true，否则浏览器获取跨域返回的 cookie 还是不会存储到浏览器



### 3:响应格式

| 名称    | 含义              |
| ------- | ----------------- |
| data    | 响应体数据 :star: |
| status  | 状态码 :star:     |
| headers | 响应头            |

axios默认会认为200及其以下的是成功，200以上的是失败

* 200 表示响应成功
* 400 请求数据不正确【比如前端给age传了个abc】
* 401 身份验证没通过【比如密码错误】
* 403 没有权限
* 404 资源不存在
* 405 不支持请求方式 post
* 500 服务器内部错误



### 3:请求拦截器

```js
_axios.interceptors.request.use(
  //请求成功的操作
  function(config) {
    // 比如在这里添加统一的 headers
    config.headers = {
             Authorization:'jyadgdukgfkjghdfsaghukghf'
    }
    return config;
  },
    
  //请求出错的操作
  function(error) {
    return Promise.reject(error);
  }
);
```



### 4:响应拦截器

```js
_axios.interceptors.response.use(
  function(response) {
    // 2xx 范围内走这里
    return response;
  },
  function(error) {
    // 超出 2xx, 比如 4xx, 5xx 走这里
    if(error.response.status===404){
        alert("不存在")
    }
    //抛出异常信息
    return Promise.reject(error);
    //不抛出，代表已经处理
    return Promise.resolve(200);
  }
);
```



### 5:封装axios

上面这些常用的，我们可以自己写一个js文件，配置好默认的配置，请求响应拦截器等等

然后导出axios对象，其他的组件直接导入使用即可，就可以不用每个组件都配置axios对象了

```js
import axios from 'axios'
const api = axios.create({
    baseURL: 'http://localhost:8080',
    withCredentials: true
});

api.interceptors.request.use(
    function (config) {
        //比如在这里添加统一的 headers
        config.headers = {
            Authorization: 'aaa.bbb.ccc'
        }
        return config;
    },
    function (error) {
        return Promise.reject(error);
    }
);

api.interceptors.response.use(
    function (response) {
        // 2xx 范围内走这里
        return response;
    },
    function (error) {
        if (error?.response?.status === 400) {
            console.log('请求参数不正确');
            return Promise.resolve(400);
        } else if (error?.response?.status === 401) {
            console.log('跳转至登录页面');
            return Promise.resolve(401);
        } else if (error?.response?.status === 404) {
            console.log('资源未找到');
            return Promise.resolve(404);
        }
        // 超出 2xx, 比如 4xx, 5xx 走这里
        return Promise.reject(error);
    }
);

export default api;
```



# 5:ElementUI

注意：`Vue属于前端人员的学习范畴，我们只需要简单学习一下表单，分页等功能的使用`

## 5.1:安装和基本使用

1：安装ElementUI组件到Vue项目里面

```sh
npm install element-ui -S
```



2：引入组件到main.js

```js
import Element from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

//下面这个放在new Vue前面
Vue.use(Element)
```



3：在自己的组件中使用 ElementUI 的组件

```vue
<el-button>按钮</el-button>
```

## 5.2:表格组件

ElementUI的表格组件是  <el-table>

ElementUI表格的一列是  <el-table-column>

表格组件的最常用属性就是`:data`

每一列的常用属性是

label：作为每列数据的开头的提示

prop：取得的data数据的什么属性

```vue
<template>
    <div>
        <!-- :data用来绑定Vue的data里面的数据 -->
        <el-table :data="students">
            <!-- prop来显示属性 -->
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="姓名" prop="name"></el-table-column>
            <el-table-column label="性别" prop="sex"></el-table-column>
            <el-table-column label="年龄" prop="age"></el-table-column>
        </el-table>
    </div>
</template>

<script>
import axios from '../util/myaxios'
const options = {
    async mounted() {
        const resp = await axios.get('/api/students');
        this.students = resp.data.data
    },
    data() {
        return {
            students: []
        }
    }
}
export default options;
</script>
```

## 5.3:分页组件

分页组件的标签是  <el-pagination>



常用的属性是

- : total：显示数据的总数，由后台返回
- : page-size：每页显示几条，可以直接定义，也可以写在data里面，一般在data获取
- : current-page：当前是第几页，一般也是data获取
- layout：用来控制分页组件显示哪些内容



常用事件是

-  @current-change：当前页改变触发的事件，注意调用的时候不加`()`
-  @size-change：当前查询的每页的大小改变的时候调用的时间，也不加`()`

```vue
<template>
    <div>
        <!--表格组件-->
        <el-table :data="students">
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="姓名" prop="name"></el-table-column>
            <el-table-column label="性别" prop="sex"></el-table-column>
            <el-table-column label="年龄" prop="age"></el-table-column>
        </el-table>
        
        <!--分页组件-->
        <el-pagination 
            :total="total"  #总数
            :page-size="queryDto.size"  #每页几条
            :current-page="queryDto.page"  #当前页
            layout="prev,pager,next,jumper,sizes,->,total"
            
            #layout用来控制组件是否显示
            ---------------------------------           
            #total 没有向右对齐的话，在前面加上->
            #prev 向前翻页
            #pager 页码
            #next 下一页
            #jumper 跳转到某一页
            #sizes 分页显示选择框
            ---------------------------------
            
            :page-sizes="[5,10,15,20]"
            @current-change="currentChange"
            @size-change="sizeChange"
        ></el-pagination>
    </div>
</template>
<script>
import axios from '../util/myaxios'
const options = {
    mounted() {
        this.query();
    },
    methods: {
        currentChange(page) {
            this.queryDto.page = page;
            this.query();
        },
        sizeChange(size){
            this.queryDto.size = size;
            this.query();
        },
        async query() {
            const resp = await axios.get('/api/students/queryList', {
                params: this.queryDto
            });
            this.students = resp.data.data.list;
            this.total = resp.data.data.total;
        }
    },
    data() {
        return {
            students: [],
            total: 0,
            queryDto: {
                page: 1,
                size: 5
            }
        }
    }
}
export default options;
</script>
```

注意：为什么有的属性加了:  有的不加 :

解释：

加了 :  的实际上就是  v:bind  也就是 加了: 的来自于 Javascript 里面来找数据

没加 : 的，那么后面的就是它的值



比如 <el-table-column label="编号" prop="id">

则就是读取的 students.id，如果我们在data里面定义了 id 的属性

上面的 prop 改为了 :id，则显示的就不是  students.id 了



但是 : total="50" 该怎么理解呢？并没有data为50的数据

实际上，就会先找javascript的值，找不到的话，就把后面的作为表达式来执行



触发查询的三种条件

1：mounted 组件挂载完成后

2：页号变化时

3：页大小变化时

查询传参应该根据后台需求，灵活采用不同方式

本例中因为使用了 GET 请求，所以没有采用请求体，只能用 params 方式传参

返回响应的格式也许会很复杂，需要掌握【根据返回的响应结构，获取数据】的能力



## 5.4:搜索和下拉框组件

搜索框的标签是  <el-input>



搜索框常用的属性是

- placeholder：提示输入框应该输入的信息
- size：大小
- v-model：输入框所绑定的数据
- clearable：是否显示清除按钮



下拉选择框是使用`<el-select>`加上`<el-option>`来实现的

el-select的常用属性和el-input的常用属性一样的

el-option的常用属性如下：

- value：选中这个的时候，对应`el-select`标签的 `v-model`绑定的值
- label：作为用户选择的提示

```vue
<template>
    <div>
        <el-input placeholder="姓名" size="mini" v-model="queryDto.name"></el-input>
        
        <el-select placeholder="性别" size="mini" v-model="queryDto.sex" clearable>
            <el-option value="男"></el-option>
            <el-option value="女"></el-option>
        </el-select>
        
        <el-select placeholder="年龄" size="mini" v-model="queryDto.age" clearable>
            <el-option value="0,20" label="0到20岁"></el-option>
            <el-option value="21,30" label="21到30岁"></el-option>
            <el-option value="31,40" label="31到40岁"></el-option>
            <el-option value="41,120" label="41到120岁"></el-option>
        </el-select>
        
        <el-button type="primary" size="mini" @click="search()">搜索</el-button>
        
        <!-- 分割线组件 -->
        <el-divider></el-divider>
        
        <el-table v-bind:data="students">
            <el-table-column label="编号" prop="id"></el-table-column>
            <el-table-column label="姓名" prop="name"></el-table-column>
            <el-table-column label="性别" prop="sex"></el-table-column>
            <el-table-column label="年龄" prop="age"></el-table-column>
        </el-table>
        
    </div>
</template>
<script>
import axios from '../util/myaxios'
const options = {
    mounted() {
        this.query();
    },
    methods: {
        async query() {
            const resp = await axios.get('/api/students/query', {
                params: this.queryDto
            });
            this.students = resp.data.data.list;
            this.total = resp.data.data.total;
        },
        search() {
            this.query();
        }
    },
    data() {
        return {
            students: [],
            queryDto: {
                name: '',
                sex: '',
                age: '',  
            }
        }
    }
}
export default options;
</script>
```

## 5.5:消息提示

完整语法：使用方式就是使用

```js
this.$message({
	配置1,
	配置2,
	配置3,
	.....
});
```

常用的属性其实就是  

- message：消息的内容
- type：消息的类型【 success/warning/info/error 】
- center：提示的信息是否居中



一般不需要过多的配置，可以这样写

```js
this.$message.消息提示的类型('消息内容')
```



案例演示：

```vue
<template>
  <el-button :plain="true" @click="open1">消息</el-button>
  <el-button :plain="true" @click="open2">成功</el-button>
  <el-button :plain="true" @click="open3">警告</el-button>
  <el-button :plain="true" @click="open4">错误</el-button>
</template>

<script>
  export default {
    methods: {
      open1() {
        this.$message('这是一条消息提示');
      },
      open2() {
        this.$message({
          message: '恭喜你，这是一条成功消息',
          type: 'success'
        });
      },

      open3() {
        this.$message({
          message: '警告哦，这是一条警告消息',
          type: 'warning'
        });
      },

      open4() {
        this.$message.error('错了哦，这是一条错误消息');
      }
    }
  }
</script>
```

## 5.6:级联选择

级联选择器中选项的数据结构为一个数组

```js
[
    {value:100, label:'主页1',children:[
        {value:101, label:'菜单1', children:[
            {value:103, label:'子项1'},
            {value:104, label:'子项2'}
        ]},
        {value:102, label:'菜单2', children:[
            {value:105, label:'子项3'},
            {value:106, label:'子项4'},
        ]}
    ]},
    
    {value:110, label:'主页2',children:[
        {value:111, label:'菜单1', children:[
            {value:113, label:'子项1'},
            {value:114, label:'子项2'}
        ]},
        {value:112, label:'菜单2', children:[
            {value:115, label:'子项3'},
            {value:116, label:'子项4'},
        ]}
    ]}
]
```

value是选中这个选项之后的标识，要求唯一



下面的例子是将后端返回的一维数组【树化】

```vue
<template>
    <el-cascader :options="ops"></el-cascader>
</template>
<script>
import axios from '../util/myaxios'
const options = {
    data(){
        return {
            ops: [
                {value:100, label:'主页1',children:[
                    {value:101, label:'菜单1', children:[
                        {value:103, label:'子项1'},
                        {value:104, label:'子项2'}
                    ]},
                    {value:102, label:'菜单2', children:[
                        {value:105, label:'子项3'},
                        {value:106, label:'子项4'},
                    ]}
                ]},

                {value:110, label:'主页2',children:[
                    {value:111, label:'菜单1', children:[
                        {value:113, label:'子项1'},
                        {value:114, label:'子项2'}
                    ]},
                    {value:112, label:'菜单2', children:[
                        {value:115, label:'子项3'},
                        {value:116, label:'子项4'},
                    ]}
                ]}
			]
        }
    }
};
export default options;
</script>
```

# 6:Vue-Router

Vue 属于单页面应用，所谓的路由，就是根据浏览器路径不同

用不同的**视图组件**替换这个页面内容展示

## 6.1:配置路由

删除router目录下本来的`index.js`，新建一个路由的 js 文件，例如 src/router/myIndex.js，内容如下

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import ExampleOne from '@/views/router/ExampleViewOne.vue'
import ExampleTwo from '@/views/router/ExampleViewTwo.vue'
import ExampleThree from '@/views/router/ExampleViewThree.vue'

Vue.use(VueRouter)

const routes = [
    {
      path:'/one',
      component: ExampleOne
    },
    {
      path:'/two',
      component: ExampleTwo
    },
    {
      path:'/three',
      component: ExampleThree
    }
  ]
  
  const router = new VueRouter({
    routes
  })
  
  export default router
```

最重要的就是建立了【路径】与【视图组件】之间的映射关系

本例中映射了 3 个路径与对应的视图组件

目录结构图示

![image-20221024221033345](https://zzx-note.oss-cn-beijing.aliyuncs.com/vue2/image-20221024221033345.png)

在 main.js 中采用我们的路由 js

```js
import Vue from 'vue'
//导入路由的js
import router from './router/myIndex'
import store from './store'

//主页面的Vue组件
import main from '@/views/MyMainView.vue'

Vue.config.productionTip = false

new Vue({
  router,
  store,
  render: h => h(main)
}).$mount('#app')

```

重启发现路由并没有生效，也就是路径改变了，路由并没有跳转

原因：所谓的路由，就是根据浏览器路径不同，用不同的**视图组件**替换这个页面内容展示

但是Vue就只有一个根组件【main.js里面导入的那个才会显示】，要在组件里面来使用才会显示

就要加上  <router-view>



现在我们的根组件是 MyMainView.vue，内容为：

```vue
<template>
    <div>
        <h1>main</h1>
    </div>
</template>

<script>
export default {
    
}
</script>
```

修改为下图，也就是加上一个  <router-view>

加上之后，路由的内容就会显示在  <router-view>  标签里面

```vue
<template>
    <div>
        <h1>main</h1>
        <router-view></router-view>
    </div>
</template>

<script>
export default {
    
}
</script>
```

其中 <router-view> 起到占位作用

改变路径后，这个路径对应的视图组件就会占据 <router-view> 的位置，替换掉它之前的内容



## 6.2:动态导入

动态导入就是不先导入所有的Vue组件，而是写在对应的路径那里，等到访问的时候再加载

动态导入必须写在箭头函数内部

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = [
    {
      path:'/one',
      component:()=>import('@/views/router/ExampleViewOne.vue')
    },
    {
      path:'/two',
      component:()=>import('@/views/router/ExampleViewTwo.vue')
    },
    {
      path:'/three',
      component:()=>import('@/views/router/ExampleViewThree.vue')
    }
  ]
  
  const router = new VueRouter({
    routes
  })
  
  export default router
```

静态导入将所有组件的 JS 代码打包到一起

如果组件非常多，打包后的 js 文件会很大影响页面加载速度

动态导入是将组件的 JS 代码放入独立的文件，等到使用到的时候才会加载，推荐使用



## 6.3:嵌套路由

组件内再要切换内容，就需要用到嵌套路由（子路由）

下面的例子是在【ExampleViewOne组件】内定义了 3 个子路由

完整的路径名称就是 父组件的路由地址加上子组件的路由地址【子路由不需要加 / 】

例如父路由是 /student，子路由是 add，则访问 add 组件的地址是 /student/add

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)

const routes = [
    {
      path:'/',
      redirect:'/one/c/p1'
    },
    {
      path:'/one',
      component:()=>import('@/views/router/ExampleViewOne.vue'),
      children:[
        {
          path:'c/p1',
          component:()=>import('@/views/router/oneViews/OneP1View.vue')
        },
        {
          path:'c/p2',
          component:()=>import('@/views/router/oneViews/OneP2View.vue')
        },
        {
          path:'c/p3',
          component:()=>import('@/views/router/oneViews/OneP3View.vue')
        }
      ]
    },
    {
      path:'/two',
      component:()=>import('@/views/router/ExampleViewTwo.vue')
    },
    {
      path:'/three',
      component:()=>import('@/views/router/ExampleViewThree.vue')
    },
    {
      path:'*',
      component:()=>import('@/views/404View.vue')
    }
  ]
  
  const router = new VueRouter({
    routes
  })
  
  export default router
```

子路由变化，切换的是父组件中  <router-view></router-view>  的内容

因为 c/p1，c/p2，c/p3是ExampleOneView的子路由

所以变化的是ExampleOneView下面的 <router-view></router-view>



redirect 可以用来默认重定向（跳转）到一个新的地址

path 的取值为 * 表示匹配不到其它 path 时，就会匹配它，一般用来处理404的资源

```js
{
  path:'*',
  redirect: '/404'
}
```



## 6.4:ElementUI 布局

主页要做布局，一般使用 ElementUI 提供的【上-【左-右】】布局

总的容器布局是：` <el-container>`

头部是：`<el-header>`

侧边栏是：`<el-aside>`

主页面是：`<el-main>`



内容展示在 main 部分

左侧侧边栏提供超链接



一般都是左边写路由跳转。main里面展现真正的路由数据，使用的是`<router-view>`

```vue
<template>
    <div class="container">
        <el-container>
            <el-header></el-header>
            <el-container>
                <el-aside width="200px"></el-aside>
                <el-main>
                    <!-- 路由跳转的时候，变化展示在 main 区域 -->
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
```



## 6.5:路由跳转

上面说了左侧的侧边栏一般都是提供链接进行路由跳转，我们怎么加链接呢？

一般都不会使用 <a> 标签，一般使用下面的方式

### 1:标签式

标签式一般是用于在Vue的ElementUI布局的时候进行路由跳转

一般都是在 <el-aside> 里面嵌入 <router-link>



注意：使用<router-link>的跳转，需要使用`to`属性

to属性的内容就是路由的路径，路由的路径就会解析到 <el-main> 里面的<router-view>去

至于解析的路由样式怎么样，就看定义的路由的关系了，进行渲染

```vue
<el-aside width="200px">
    <router-link to="/c1/p1">P1</router-link>
    <router-link to="/c1/p2">P2</router-link>
    <router-link to="/c1/p3">P3</router-link>
</el-aside>
```



### 2:编程式

注意：编程式一般使用在按钮上，也就是绑定事件的时候

我这个按钮使用 <el-button> 的按钮，可以自己随意选择

具体的方法里面就可以写跳转的代码，怎么进行路由跳转呢？

可以使用当前的路由对象来进行跳转，具体怎么跳转呢？

使用this.$router来拿到当前的路由对象，然后调用`push`方法来进行跳转

整体的使用也就是   this.$router.push(url) 

```vue
<el-header>
    <el-button type="primary" icon="el-icon-edit" 
               circle size="mini" @click="jump('/c1/p1')"></el-button>
    <el-button type="success" icon="el-icon-check" 
               circle size="mini" @click="jump('/c1/p2')"></el-button>
    <el-button type="warning" icon="el-icon-star-off" 
               circle size="mini" @click="jump('/c1/p3')"></el-button>
</el-header>
```

```vue
<script>
const options = {
    methods : {
        jump(url) {
            this.$router.push(url);
        }
    }
}
export default options;
</script>
```

其中 this.$router 是拿到路由对象

其中 push 方法根据 url 进行跳转



## 6.6:导航菜单

导航菜单组件跟路由跳转也存在一定的关系，显得更加的专业一些

一般也是用在 <el-aside>里面

ElementUI的导航菜单的标签是 <el-menu>，代表这个标签里面的是一个大的导航栏

ElementUI的子菜单的标签是 <el-menu-item>，这就是每一个单独的菜单

每一个菜单都可以对应上面我们写的一个路由跳转链接

注意：这个标签并不能添加子菜单，是最小的单位了

如果想在一个菜单下面，还想拥有多个子菜单，需要使用  <el-submenu>

这个标签下面还可以继续添加 <el-menu-item>

如果想给菜单项的文字前面加上图标的话，需要使用<i>标签，class属性就是icon的值

这个可以自己去官网找属性：https://element.eleme.cn/#/zh-CN/component/icon

```vue
<el-menu>
    <el-menu-item>
        <span slot="title">
            <i class="el-icon-phone"></i>
            菜单1
        </span>
    </el-menu-item>
    <el-menu-item>
        <span slot="title">
            <i class="el-icon-star-on"></i>
            菜单2
        </span>
    </el-menu-item>
</el-menu>
```

这样只是一级的菜单，怎么做出多级菜单的效果呢？

存在子项的话，要使用 el-submenu，在 el-submenu下面嵌套 el-menu-item

```vue
<el-menu>
    <!-- 菜单一下面存在三个子项 -->
    <el-submenu>
        <span slot="title">
            <i class="el-icon-platform-eleme"></i>
            菜单1
        </span>
        <el-menu-item index="/c1/p1">子项1</el-menu-item>
        <el-menu-item index="/c1/p2">子项2</el-menu-item>
        <el-menu-item index="/c1/p3">子项3</el-menu-item>
    </el-submenu>
    <el-menu-item>
        <span slot="title">
            <i class="el-icon-phone"></i>
            菜单2
        </span>
    </el-menu-item>
    <el-menu-item>
        <span slot="title">
            <i class="el-icon-star-on"></i>
            菜单3
        </span>
    </el-menu-item>
</el-menu>
```

但是如果就这样写了之后，是不能够进行路由的跳转的

我们并没有指定每个菜单需要跳转的位置，使用导航菜单进行路由跳转的话

1：在最开始的菜单项<el-menu>加上 router，代表菜单会使用路由

2：在每个菜单项加上 index 属性，属性的值写上路由的地址，这样点击就会自动进行路由跳转了

```vue
<el-menu router>
    <el-submenu index="/c1">
        <span slot="title">
            <i class="el-icon-platform-eleme"></i>
            菜单1
        </span>
        <el-menu-item index="/c1/p1">子项1</el-menu-item>
        <el-menu-item index="/c1/p2">子项2</el-menu-item>
        <el-menu-item index="/c1/p3">子项3</el-menu-item>
    </el-submenu>
    <el-menu-item index="/c2">
        <span slot="title">
            <i class="el-icon-phone"></i>
            菜单2
        </span>
    </el-menu-item>
    <el-menu-item index="/c3">
        <span slot="title">
            <i class="el-icon-star-on"></i>
            菜单3
        </span>
    </el-menu-item>
</el-menu>
```



小结导航菜单：

图标和菜单项文字建议用 <span slot='title'></span> 包裹起来，不然多级菜单会错位

对 el-menu 标签上加上 router 属性，表示结合导航菜单与路由对象，不然是没有路由功能的

此时使用el-menu-item就可以利用菜单项的 index 属性来指定路由跳转的地址

没有子项的话，使用el-menu-item

存在子项的，使用el-submenu



## 6.7:动态路由

### 1:概述

因为用户存在的权限是不一样的，所以后台数据返回的数据包含的路由信息是不一样的

我们先将菜单、路由信息（仅主页的）存入数据库中



m1 对应两个子项，m2 对应三个子项，m3 对应两个子项，m4 没有子项

```sql
insert into menu(id, name, pid, path, component, icon) values
    (101, '菜单1', 0,   '/m1',    null,         'el-icon-platform-eleme'),
    (102, '菜单2', 0,   '/m2',    null,         'el-icon-delete-solid'),
    (103, '菜单3', 0,   '/m3',    null,         'el-icon-s-tools'),
    (104, '菜单4', 0,   '/m4',    'M4View.vue', 'el-icon-user-solid'),
    (105, '子项1', 101, '/m1/c1', 'C1View.vue', 'el-icon-s-goods'),
    (106, '子项2', 101, '/m1/c2', 'C2View.vue', 'el-icon-menu'),
    (107, '子项3', 102, '/m2/c3', 'C3View.vue', 'el-icon-s-marketing'),
    (108, '子项4', 102, '/m2/c4', 'C4View.vue', 'el-icon-s-platform'),
    (109, '子项5', 102, '/m2/c5', 'C5View.vue', 'el-icon-picture'),
    (110, '子项6', 103, '/m3/c6', 'C6View.vue', 'el-icon-upload'),
    (111, '子项7', 103, '/m3/c7', 'C7View.vue', 'el-icon-s-promotion');
```

不同的用户查询的的菜单、路由信息是不一样的

例如：访问  /api/menu/admin  返回所有的数据

```json
[
    {
        "id": 102,
        "name": "菜单2",
        "icon": "el-icon-delete-solid",
        "path": "/m2",
        "pid": 0,
        "component": null
    },
    {
        "id": 107,
        "name": "子项3",
        "icon": "el-icon-s-marketing",
        "path": "/m2/c3",
        "pid": 102,
        "component": "C3View.vue"
    },
    {
        "id": 108,
        "name": "子项4",
        "icon": "el-icon-s-platform",
        "path": "/m2/c4",
        "pid": 102,
        "component": "C4View.vue"
    },
    {
        "id": 109,
        "name": "子项5",
        "icon": "el-icon-picture",
        "path": "/m2/c5",
        "pid": 102,
        "component": "C5View.vue"
    }
]
```

访问  /api/menu/wang  返回

```json
[
    {
        "id": 103,
        "name": "菜单3",
        "icon": "el-icon-s-tools",
        "path": "/m3",
        "pid": 0,
        "component": null
    },
    {
        "id": 110,
        "name": "子项6",
        "icon": "el-icon-upload",
        "path": "/m3/c6",
        "pid": 103,
        "component": "C6View.vue"
    },
    {
        "id": 111,
        "name": "子项7",
        "icon": "el-icon-s-promotion",
        "path": "/m3/c7",
        "pid": 103,
        "component": "C7View.vue"
    }
]
```

本质就是前端根据他们身份不同，访问数据库拿到的路由组件就不相同

我们动态的把数据库返回的信息添加到路由的配置里面去，这样各个用户显示菜单也就不同了

### 2:动态路由

我们可以这样做：

在路由的 JS 文件里面添加一个方法，参数是我们所得到的用户登录之后返回的路由和菜单数据

执行这个方法的时候就能获得所有的路由数据并添加到路由的配置里面

如何动态的添加路由呢？

使用当前的路由对象 this.$router 的 addRouter('' , {}) 方法

参数一：父路由的name，注意，并不是父路由的其他属性，而是name属性，唯一即可

参数二：路由的配置，就跟之前我们定义的路由配置项一模一样

```js
export function addServerRoutes(array) {
  for (const { id, path, component } of array) {
    if (component !== null) {
      // 动态添加路由
      // 参数1：父路由名称，index.js添加路由可以指定name属性
      // 参数2：路由信息对象
      // 假设我们的 / 路由的name就叫做 c
      router.addRoute('c', {
        path: path,
        name: id,
        component: () => import(`@/views/router/container/${component}`)
      });
    }
  }
}
```

原来的路由的 JS 这边只保留几个固定路由，如主页、404 和 login

我们在组件内引入 路由JS 的 addServerRoutes() 方法

先发起登录请求，查询到路由数据之后，传给 addServerRoutes() 方法

这样就能够把当前用户在服务器的路由信息和组件关系对应到 路由JS 的配置里面去了



这里要注意组件路径，前面 @/views 是必须在 JS 这边完成拼接的，否则 import 函数会失效

使用  this.$router.getRouters()  可以拿到路由信息

可以在发送请求前使用和发送之后使用进行对比



### 3:重置路由

在用户注销时应当重置路由，例如管理员有所以权限，张三只有三个路由的权限，但是先admin登录

再张三登录，张三会发现自己存在所有的路由，在路由的JS里面写上以下代码

注意：Vue2并不能直接清空动态添加的路由信息，Vue2 只能添加路由，Vue3才有清空路由



所以只能创建一个新的路由对象【读取的是路由.js】，重新赋值给我的路由对象

这样就是一个新的原本的路由对象了

```js
export function resetRouter() {
  // 创建一个新的默认的路由对象
  router.matcher = new VueRouter({ routes }).matcher
}

#其他的组件里导入函数 import {resetRouter} from '@/.....'
#其他的组件里使用函数 resetRouter()
```

这样组件每次登陆的时候，先重置路由，再写入新的路由配置！



### 4:页面刷新

因为 Vue 是单页面应用，刷新之后所有的内容都会重置

页面刷新后，会导致动态添加的路由失效，解决方法是将路由数据存入 sessionStorage

比如路由配置默认没有配置/order页面，登录之后动态添加了order组件

但是我点了刷新之后，重新创建了路由对象，路由对象就只存在写死的路由信息【并没有/order】

访问order不存在了，突然变为/404页面，本来有，突然又没了，用户体验不好



注意：sessionStorage 的 key 和 value 都是String类型，并不可以存储其他的类型

localStorage：浏览器关闭，数据还在

sessionStorage：以标签页为单位，标签页关闭数据就不在了

JS对象转JSON字符串：JSON.stringify(JS数据)

JSON字符串转化为JS对象：JSON.parse(JSON数据)



思路就是：

1：服务器返回数据，把数据转化为字符串存入sessionStorage

2：页面加载的时候，从sessionStorage读取数据，转化为JS数据，传入动态添加路由的方法内



```vue
<script>
import axios from '@/util/myaxios'
import {resetRouter, addServerRoutes} from '@/router/exampleRouter'
const options = {
    data() {
        return {
            username: 'admin'
        }
    },
    methods: {
        async login() {       
            resetRouter(); // 登陆之前先重置路由     
            const resp = await axios.get(`/api/menu/${this.username}`)
            // 传递数组
            const array = resp.data.data;
            sessionStorage.setItem('serverRoutes', JSON.stringify(array))
            addServerRoutes(array); // 动态添加路由
            this.$router.push('/');
        }
    }
}
export default options;
</script>
```

页面刷新，重新创建路由对象时，从 sessionStorage 里恢复路由数据

```js
const router = new VueRouter({
  routes
})

const serverRoutes = sessionStorage.getItem('serverRoutes');
if(serverRoutes) {
  const array = JSON.parse(serverRoutes);
  addServerRoutes(array) // 动态添加路由
}
```

localStorage 浏览器关闭，存储的数据还在

sessionStorage 以标签页为单位，标签不关闭数据就在，关闭标签之后数据被清除

注意存储的key和value都是String类型，对于对象来说，我们都是存储 JSON 字符串



# 7:Vuex

## 7.1:介绍

Vuex 可以在多个组件之间共享数据，并且共享的数据是【响应式】的

响应式：即数据的变更能及时渲染到模板，能够自动监测数据并展现



与之对比 localStorage 与 sessionStorage 也能共享数据，但缺点是数据并非【响应式】

Vuex首先需要定义 state 与 mutations，一个用来读取共享数据，一个用来修改共享数据

Vuex的默认创建是 store目录下的 index.js

我们定义需要读取的共享数据，加在index.js内的 state 里面

修改数据的话，使用 mutations 的方法



先查看一下默认的Vuex的定义：也就是store下的index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})

```

## 7.2:基本使用

读取数据，走 state 或者 getters

修改数据，走 mutations 或者 actions



我们先使用 state 和 mutations 来演示，后面再记录区别

假设我们需要在多个组件之间共享一个数据叫做`name`，我们就在`state`下面定义一个name

修改共享数据的话，需要在`mutations`里面定义方法，定义的方法需要两个参数

参数一：就是Vuex.Store的 `state `，这样就可以使用`state.属性`拿到我们的共享数据了

参数二：就是要修改的新值，如果从服务器来的数据，这里就不需要，也就是一个参数即可



修改store下的index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    name: '',
  },
  getters: {
  },
  mutations: {
    // 参数一：state
    // 参数二：需要修改的新值
    updateName(state, name) {
      state.name = name;
    }
  },
  actions: {
  },
  modules: {
  }
})
```

### 1:修改共享数据

先拿到store对象：使用  this.$store  就可以拿到这个对象

调用commit来执行修改，注意不是调用updateName，commit间接来调用updateName

commit方法的第一个参数：`方法名`

commit方法的第二个参数：`修改的值`

注意 mutations 里面的方法的 state 不需要我们传【Vuex会自动传】

```vue
<template>
    <div class="p">
        <el-input placeholder="请修改用户姓名" 
            size="mini" v-model="name"></el-input>
        <el-button type="primary" size="mini" @click="update()">修改</el-button>
    </div>
</template>
<script>
const options = {
    methods: {
        update(){
            // 参数一：mutations内的方法名称
            // 参数二：需要修改的新值
            this.$store.commit('updateName', this.name);
        }
    },
    data () {
        return {
            name:''
        }
    }
}
export default options;
</script>
```

注意：mutations 方法并不能直接调用只能通过   store.commit(方法名, 参数)  来间接调用

### 2:读取共享数据

读取数据使用  $store.state.属性

```vue
<template>
    <div class="container">
        <el-container>
            <el-header>
                <div class="t">
                    欢迎您：{{ $store.state.name }}
    			</div>
            </el-header>
            <el-container>
                <el-aside width="200px">
                </el-aside>
                <el-main>
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
```



## 7.2:mapState

每次去写 `$store.state.name`来读取数据，这样的代码显得非常繁琐，可以使用计算属性简化

```js
<template>
    <div class="container">
        <el-container>
            <el-header>
                <div class="t">欢迎您：{{ name }}, {{ age }}</div>
            </el-header>
            <el-container>
                <el-aside width="200px">
                </el-aside>
                <el-main>
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
<script>
import { mapState } from 'vuex'
const options = {
    computed: {
        //这样写一次就行了，作为计算属性
        name(){
            return this.$store.state.name;
        },
        age(){
            return this.$store.state.age;
        }
    }
}
export default options;
</script>
```

但是每次写这种方法也非常麻烦

每次都是定义计算属性，定义统一的  return this.$store.state.属性，都是重复操作

所以Vuex为我们提供了一种简化的方式  mapState() ：帮我们自动生成共享属性的计算属性



使用 mapState()，参数是一个数组，数组内部需要说明为哪些共享的属性自动生成计算属性

例如：为name和age生成计算属性  mapState(['name', 'age']) ，返回的是一个对象

这个对象内部就是我们参数提供的那些属性的计算属性



注意：使用之前记得使用   import {mapState} from 'vuex'  导入VueX的功能

```vue
<template>
    <div class="container">
        <el-container>
            <el-header>
                <div class="t">欢迎您：{{ name }}, {{ age }}</div>
            </el-header>
            <el-container>
                <el-aside width="200px">
                </el-aside>
                <el-main>
                    <router-view></router-view>
                </el-main>
            </el-container>
        </el-container>
    </div>
</template>
<script>

// 导入 mapState 的功能
import { mapState } from 'vuex'
const options = {
        computed: mapState(['name', 'age'])
    	// 或者像这样写也是可以的
        computed:{
    		...mapState(['name', 'age'])
    	}
    }
}
export default options;
</script>
```

mapState参数是一个数组，返回的是一个对象

对象内包含了 name() 和 age() 的这两个方法作为计算属性

此对象配合 `...` 展开运算符，直接生成多个方法，填充入 computed 即可使用



## 7.3:mapMutations

这个跟`mapState`类似，这个是为了简化修改共享数据的操作

因为我们在之前的修改操作，基本上也都是  this.$store.commit('方法名', 参数)

也都是一些重复的操作，可以被优化

参数也是数组，数组内的元素名称对应我们的 mutations 内部的方法名称

生成的方法名称就是 mutations 内部的方法名称，参数就是需要修改的新值，直接调用即可

```vue
<template>
    <div class="p">
        <el-input placeholder="请修改用户姓名" size="mini" v-model="name"></el-input>
        <el-button type="primary" size="mini" @click="updateName(name)">修改</el-button>
    </div>
</template>
<script>

// 导入mapMutations的功能
import {mapMutations} from 'vuex'
const options = {
    methods: mapMutations(['updateName'])
    //或者
    methods: {
        ...mapMutations(['updateName'])
    },
    data () {
        return {
            name:''
        }
    }
}
export default options;
</script>
```

类似的，调用 mutation 修改共享数据也可以简化

mapMutations 返回的对象中包含的方法，就会自动调用 store.commit() 来执行 mutation 方法

注意参数传递略有不同



## 7.4:actions

mutations 方法内不能包括修改不能立刻生效的代码【例如发送axios请求得到服务端数据的代码】

否则会造成 Vuex 调试工具工作不准确，如果需要一定的时间才能响应

针对不能立刻返回结果的代码，必须把这些代码写在 actions 方法中



使用方式：

在 actions 里面定义方法，参数是context，使用 context.commit('mutations方法名'，更改的数据)

调用 actions 里面的方法：使用 this.$store.dispatch('actions里面的方法'，参数)

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

import axios from '@/util/myaxios'
export default new Vuex.Store({
  state: {
    name: '',
    age: 18
  },
  getters: {
  },
  mutations: {
    updateName(state, name) {
      state.name = name;
    },
      
    // 错误的用法，如果在mutations方法中包含了异步操作
    // 消耗时间，会造成开发工具不准确
    async updateServerName(state) {
      const resp = await axios.get('/api/user');
      const {name, age} = resp.data.data;
      state.name = name;
      state.age = age;
    }
    
    // 我们直接把数据拿到之后再作为参数给mutations方法
    // 这样就不会出现问题了，因为是立刻完成的，无需等待
    // 那么请求在哪里发送呢？在 actions 里面发送
    updateServerName(state, data) {
      const { name, age } = data;
      state.name = name;
      state.age = age;
    }
  },
      
  actions: {
    async updateServerName(context) {
      const resp = await axios.get('/api/user');
      context.commit('updateServerName', resp.data.data)
    }
  },
  modules: {
  }
})
```

首先调用 actions 的 updateServerName 获取数据

然后再由它自己间接调用 mutations 的 updateServerName 更新共享数据

```js
const promise = this.$store.dispatch('updateServerName');
const resp = await promise 或者 promise.then();
```

## 7.5:mapActions

使用`import {mapActions} from 'vuex'`

页面使用 actions 的方法可以这么写

```vue
<template>
    <div class="p">
        <el-button type="primary" size="mini"
            @click="updateServerName()">从服务器获取数据,存入store</el-button>
    </div>
</template>
<script>
import { mapActions } from 'vuex'
const options = {
    methods: {
        ...mapActions(['updateServerName'])
    }
    
    //或者
    methods: mapActions(['updateServerName'])
}
export default options;
</script>
```

mapActions 会生成调用 actions 中方法的代码，actions调用的是mutations的方法

调用 actions 的代码返回的是 Promise 对象，可以用同步或异步方式接收结果

```js
const promise = this.$store.dispatch('action名称', 参数)
const resp = await promise 或者 promise.then()
```



