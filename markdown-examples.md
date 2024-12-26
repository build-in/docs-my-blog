# OMC微应用开发规范

## 前言

编写 Vue.js 开发规范的目的是确保团队成员在开发过程中遵循一致的编码风格、结构和最佳实践，以提高代码的可维护性和可读性以及减少错误等。以下是 Vue.js 开发规范的示例，涵盖了常见的代码风格、文件结构、命名约定等方面。

原文地址：[传送门](https://ucnrydq2bh3h.feishu.cn/wiki/PMgQwBRqTiYV3ukJQdncZHKxnjf)

## 一、Vue项目结构规范

**目录结构**

```plaintext
src/
├── api/                ---------- 请求接口
├── assets/             ---------- 资源
├── components/         ---------- 公共组件
├── config/             ---------- 配置文件（站点配置/灰度key配置/常量配置等）
├── directive/          ---------- 自定义指令
├── const/              ---------- 通用变量
├── layout/             ---------- 布局
├── filters/            ---------- 过滤方法
├── lang                ---------- 语言包
├── mock                ---------- mock数据
├── icons               ---------- 图标
├── store/              ---------- 状态管理
├── router/             ---------- 路由配置
├── utils/              ---------- 工具类
├── store/              ---------- 前端数据管理
├── App.vue             ---------- 根组件
├── main.js             ---------- 应用入口脚本
```

---

**命名规范**

组件名为多个单词  🌟🌟🌟

组件名应该始终是多个单词的，根组件 `App` 以及 `<transition>`、`<component>` 之类的 Vue 内置组件除外。

这样做可以避免跟现有的以及未来的 HTML 元素相冲突，因为所有的 HTML 元素名称都是单个单词的。

🙅不推荐
```js
  Vue.component('todo', {

    // ...

  })

  export default {

    name: 'Todo',

    // ...

  }
```


👍推荐
```js
Vue.component('todo-item', {

  // ...

})

export default {

  name: 'TodoItem',

  // ...

}
```

---

### 1、基础组件名  🌟🌟

应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 `Base`、`App` 或 `V`

#### 🙅不推荐
```plaintext
components/

|- MyButton.vue

|- VueTable.vue

|- Icon.vue
```



#### 👍推荐

```plaintext
components/

|- BaseButton.vue

|- BaseTable.vue

|- BaseIcon.vue

components/

|- AppButton.vue

|- AppTable.vue

|- AppIcon.vue

components/

|- VButton.vue

|- VTable.vue

|- VIcon.vue
```

##### 详解

这些组件为你的应用奠定了一致的基础样式和行为。它们可能只包括：

- HTML 元素

- 其它基础组件

- 第三方 UI 组件库

但是它们绝不会包括全局状态 (比如来自 Vuex store)。

它们的名字通常包含所包裹元素的名字 (比如 BaseButton、BaseTable)，除非没有现成的对应功能的元素 (比如 BaseIcon)。如果你为特定的上下文构建类似的组件，那它们几乎总会消费这些组件 (比如 BaseButton 可能会用在 ButtonSubmit 上)。

这样做的几个好处：

- 当你在编辑器中以字母顺序排序时，你的应用的基础组件会全部列在一起，这样更容易识别。

- 因为组件名应该始终是多个单词，所以这样做可以避免你在包裹简单组件时随意选择前缀 (比如 MyButton、VueButton)。


---

###  2、紧密耦合的组件名  🌟🌟

和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

#### 🙅不推荐
```plaintext
components/

|- TodoList.vue

|- TodoItem.vue

|- TodoButton.vue

components/

|- SearchSidebar.vue

|- NavigationForSearchSidebar.vue
```

#### 👍推荐

```plaintext
components/

|- TodoList.vue

|- TodoListItem.vue

|- TodoListItemButton.vue

components/

|- SearchSidebar.vue

|- SearchSidebarNavigation.vue
```

---

### 3、单文件组件文件名的大小写  🌟🌟

单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。

#### 🙅不推荐

```plaintext
components/

|- mycomponent.vue

components/

|- myComponent.vue
```

#### 👍推荐

```plaintext
components/

|- MyComponent.vue

components/

|- my-component.vue
```

### 4、组件文件 🌟🌟

当你需要编辑一个组件或查阅一个组件的用法时，可以更快速的找到它。

#### 🙅不推荐

```js
Vue.component('TodoList', {

  // ...

})

Vue.component('TodoItem', {

  // ...

})
```


#### 👍推荐

```plaintext
components/

|- TodoList.js

|- TodoItem.js

components/

|- TodoList.vue

|- TodoItem.vue
```

### 5、Prop 大小写  🌟🌟

**在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板和** [**JSX**](https://v2.cn.vuejs.org/v2/guide/render-function.html#JSX) **中应该始终使用 kebab-case。**

我们单纯的遵循每个语言的约定。在 JavaScript 中更自然的是 camelCase。而在 HTML 中则是 kebab-case。

#### 🙅不推荐

```plaintext
props: {

  'greeting-text': String

}

<WelcomeMessage greetingText="hi"/>
```

#### 👍推荐

```plaintext
props: {

  greetingText: String

}

<WelcomeMessage greeting-text="hi"/>
```

## 二、脚本部分

:::
组件属性顺序（如 name, props, data, computed, methods, watch, lifecycle hooks 等）。

避免在 data 中定义复杂对象，推荐使用函数返回初始状态。

脚本命名方法

驼峰命名

命名建议

|  动词  |  含义  |  🌰  |
| --- | --- | --- |
|  can  |  判断是否可执行某个动作(权限)  |  canNext  |
|  has  |  判断是否含有某个值  |  hasPermission  |
|  is  |  判断是否为某个值  |  isProd  |
|  get  |  获取某个值  |  getAllOptionsName  |
|  set  |  设置某个值  |  setDynamicRules  |
|  load  |  加载某些数据  |  load  |
|  on/handle  |  事件操作/业务操作  |  onChange  |
|  queryXXXXService  |  api 请求数据  |  queryCurrencyService  |
:::

**Vue 组件规范**

### 1、多个attribute的元素  🌟🌟

**多个 attribute 的元素应该分多行撰写，每个 attribute 一行。**

在 JavaScript 中，用多行分隔对象的多个 property 是很常见的最佳实践，因为这样更易读。模板和 [**JSX**](https://v2.cn.vuejs.org/v2/guide/render-function.html#JSX) 值得我们做相同的考虑。

#### 🙅不推荐

```html

<img src="[https://vuejs.org/images/logo.png](https://vuejs.org/images/logo.png)" alt="Vue Logo" />

<MyComponent foo="a" bar="b" baz="c"/>
```

#### 👍推荐
```html

<img src="[https://vuejs.org/images/logo.png](https://vuejs.org/images/logo.png)" alt="Vue Logo" />

<MyComponent

  foo="a"

  bar="b"

  baz="c"

/>
```


### 2、模版中简单的表达式  🌟🌟

**组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。**

复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的_是什么_，而非_如何_计算那个值。而且计算属性和方法使得代码可以重用。

#### 🙅不推荐

```html

<img src="[https://vuejs.org/images/logo.png](https://vuejs.org/images/logo.png)" alt="Vue Logo" />

<MyComponent foo="a" bar="b" baz="c" />

```

#### 👍推荐

```html
<img src="[https://vuejs.org/images/logo.png](https://vuejs.org/images/logo.png)" alt="Vue Logo" />

<MyComponent foo="a" bar="b" baz="c" />
```


#### 👍推荐
```html
<!-- 在模板中 -->
{{ normalizedFullName }}

// 复杂表达式已经移入一个计算属性

computed: {

  normalizedFullName: function () {

    return this.fullName.split(' ').map(function (word) {

      return word\[0\].toUpperCase() + word.slice(1)

    }).join(' ')

  }

}
```

#### 3、简单的计算属性

**应该把复杂计算属性分割为尽可能多的更简单的 property。**

#### 🙅不推荐

```html
computed: {

  price: function () {

    var basePrice = this.manufactureCost / (1 - this.profitMargin)

    return (

      basePrice -

      basePrice \* (this.discountPercent || 0)

    )

  }

}
```

#### 👍推荐
```html
computed: {

  basePrice: function () {

    return this.manufactureCost / (1 - this.profitMargin)

  },

  discount: function () {

    return this.basePrice \* (this.discountPercent || 0)

  },

  finalPrice: function () {

    return this.basePrice - this.discount

  }

}
```

:::
更简单、命名得当的计算属性是这样的：

**易于测试**

当每个计算属性都包含一个非常简单且很少依赖的表达式时，撰写测试以确保其正确工作就会更加容易。

**易于阅读**

简化计算属性要求你为每一个值都起一个描述性的名称，即便它不可复用。这使得其他开发者 (以及未来的你) 更容易专注在他们关心的代码上并搞清楚发生了什么。

**更好的“拥抱变化”**

任何能够命名的值都可能用在视图上。举个例子，我们可能打算展示一个信息，告诉用户他们存了多少钱；也可能打算计算税费，但是可能会分开展现，而不是作为总价的一部分。
:::

## 三、常见开发注意点和实践点

### 1、对象字段的防空校验

代码中获取某一个对象的值时，如果对象为 null 或者 undefind，则会抛出异常，导致代码错误

:::
![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/80647945-b1d0-4bac-935c-9c58e5ee0d87.png)

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/f3896ddf-ba40-4862-b2e9-34058aae451d.png)
:::

#### 🙅不推荐

```js
const obj = null;

console.log(obj.a) // Cannot read properties of null (reading 'a')

let obj;

console.log(obj.name);  // Cannot read properties of undefined (reading 'name')
```

#### 👍推荐

##### 方法一 
[可选链运算符（?.）](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Optional_chaining) 

```js
const adventurer = {

  name: 'Alice',

  cat: {

    name: 'Dinah',

  },

};

const catName = adventurer?.cat?.name;

console.log(dogName); // Dinah

const dogName = adventurer?.dog?.name;

console.log(dogName); // undefined 
```

##### 方法二 
通过if判断

```js

const obj = null;

if(obj){

  // do somethig....

  const a = obj.a

}else {

  // TODO ...

}
```

### 2、打包动态参数的注入规范

~~如果通过 devlops 平台注入或者手动添加环境变量参数，vue/webpack.definePlugin 项目对命名有规范要求~~

~~需要以 VUE\_APP 开头才可以被注入使用~~

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/3d9d7f71-7e1c-4f2b-bc14-bc4ab2fb5b96.png)

[https://blog.csdn.net/weixin\_45811256/article/details/117415565](https://blog.csdn.net/weixin_45811256/article/details/117415565)

### 3、接口请求或者Pormise链式调用完善catch()方法

在使用 JavaScript 的 Promise 时，规范的写法可以提高代码的可读性和可维护性。特别是使用 .catch 方法来处理错误，有助于集中管理错误，提升代码的健壮性。以下是详细解释和示例。

**规范写法示例**

```javascript
fetchData()
  .then(response => {
    console.log(response);
    // 处理成功的结果
    return response;
  })
  .catch(error => {
    console.error('捕获到的错误:', error);
    // 处理错误
  });

```

**.catch 方法的好处**

*   **集中管理错误处理**：将所有错误处理逻辑集中在 .catch 方法中，提高代码的可读性和维护性。
    
*   **简化代码**：避免在每个 then 方法中编写重复的错误处理逻辑，使代码更加简洁清晰。
    
*   **捕获链式调用中的所有错误**：确保在链式调用中任何一个步骤出现错误时，都能被 .catch 方法捕获并处理。
    

++另外由于项目中引用 Sentry ，Sentry++ ++会自动上报未被捕获的错误，所以都 Promise 中都需增加上.catch()++

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/b6e48b30-5774-4b17-bfc8-cae881c46392.png)

### 4、解构 Array.prototype.find 响应数据

[解构 Array.prototype.find 响应数据](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/find)

find 方法遍历数组，查找到匹配对象，找到返回对象，未找到返回 undefined，这里因为未找到，并且进行解构，导致代码异常

#### 示例 🙅不推荐

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/889639f9-1b93-4c30-8feb-66fec7acc566.png)


### 规范写法示例 🙅不推荐

```javascript
const list = [
  {name: 'tom', age: 18},
  {name: 'jerry', age: 16}
]
const _name = 'dog'

// 解法-1
let { name } = (list || []).find(item => item.name === _name) || {};

// 解法2
const result = (list || []).find(item => item.name === _name) || {};

// do something
```

[**引用vue-i18n中配置数组类型，加载异常时的处理**](https://uovol.com/how-to-check-whether-the-key-exists-in-vue-i18n)

##### 场景

在多语言中配置![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/e80ffebf-ede8-40bf-8246-5ec691ffb044.png) 并且在项目中进行业务处理，常见的处理，针对orderTypeOptions进行数组方法的处理，但有时候，vue-i18n可能会加载异常，导致使用this.$t('orderSearch.orderTypeOptions') || \[\]).filter() 解析成'orderSearch.orderTypeOptions'.filter 并报错

![image.png](https://alidocs.oss-cn-zhangjiakou.aliyuncs.com/res/YvenvK56BgRAqoyZ/img/97bb75f0-8f4c-4a2f-8b57-5ceeb064f419.png)

**规范写法示例** **👍推荐**

```javascript

// 多语言未找到对应key返回为空数组
const i18nArr = function (key) {
  return Window.vue.$te(key) ? Window.vue.$t(key) : [];
};

export default i18nArr;


// 使用
this.$tArr('accountConversion.statusMap')
    .find((item) => item.value === this.detailInfo.status) || {}
```

参考文档：

[三方应用: https://v2.cn.vuejs.org/v2/style-guide/index.html#%E8%A7%84%E5%88%99%E5%BD%92%E7%B1%BB](https://v2.cn.vuejs.org/v2/style-guide/index.html#%E8%A7%84%E5%88%99%E5%BD%92%E7%B1%BB)

[三方应用: https://airbnb.io/javascript/](https://airbnb.io/javascript/)