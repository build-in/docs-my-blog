# 风控开发规范

## 前言

这里主要针对风控项目的代码，有些需要优化的地方改进说明。

## 一、目录结构

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

## 二、代码编写规范

### 1、v-for一定要增加key

* 错误写法1

_完全没有配置key_

```html
 <template v-for="item in $$t('tradeInfoOption')">
    <el-descriptions-item :label="item.label">
    {{  getTradeInfo(item.prop)}}
    </el-descriptions-item>
 </template>
```

* 错误写法2

_设置index，但没有配置key_

```html
 <template v-for="(item,index) in $$t('basicInfoOption')">
      <el-descriptions-item  :label="item.label" :span="1">
        {{getBaseInfo(item)}}
      </el-descriptions-item>
    </template>

```

* 错误写法3

_设置index，但没有配置key_
```html
<div v-for="(it,subIndex) in item.nameTableFieldList">
                  <el-form-item :label="it.fieldDesc" :prop="'nameTableList.'+index+'.tableFieldParams.'+it.fieldName"
                                :rules="{required: it.isPrimary === 1 ? true : false, trigger: 'blur'}" v-if="hasPermission">
                    <el-input v-model.trim="item.tableFieldParams[it.fieldName]"
                              v-if="it.fieldType==='varchar'"
                              clearable style="width: 220px;"/>
                    <el-date-picker
                      v-else-if="it.fieldType === 'bigint'"
                      class="mediumFormItem"
                      v-model="item.tableFieldParams[it.fieldName]"
                      type="datetime"
                    />
                    <el-input-number v-model="item.tableFieldParams[it.fieldName]"
                                     :precision="getFieldLengthbyType(it.fieldType,it.decimalLength)"
                                     :controls="false"
                                     v-else-if="[' bigint','decimal'].includes(it.fieldType)"
                                     style="width: 220px;"/>
                    <el-select v-model="item.tableFieldParams[it.fieldName]"
                               v-else-if="it.fieldType === 'char'"
                               style="width: 220px;">
                      <el-option
                        v-for="(option, opIndex) in $$t('booleanOption')"
                        :key="opIndex"
                        :label="option.label"
                        :value="option.value"
                      />
                    </el-select>
                  </el-form-item>
                  <el-form-item :label="it.fieldDesc" v-else>
                    <span>{{it.fieldType === 'bigint' ? $util.formatTime(item.tableFieldParams[it.fieldName],null,null) : item.tableFieldParams[it.fieldName]}}</span>
                  </el-form-item>
                </div>
```

template上不止直接写key，所有有两种写法解决问题。

* 正确写法1

_将key加在遍历的子项上_

```html
 <template v-for="(item,index) in $$t('disputeInfoOption')">
    <el-descriptions-item  :key="index" :label="item.label">
      {{  getTradeInfo(item.prop)}}
    </el-descriptions-item>
  </template>

```
* 正确写法2

_在子项上进行循环_

```html
  <template>
    <el-descriptions-item v-for="(item,index) in $$t('disputeInfoOption')" :key="index" :label="item.label">
      {{  getTradeInfo(item.prop)}}
    </el-descriptions-item>
  </template>

```

### 2、快捷名引入文件

目前支持引入src文件下的所有文件

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

案例：在一个VUE页面引入src下的文件

```vue
<template>
  <div>测试文件引入</div>
</template>

<script>
import { getPageList } from "@/api/nameApproval";
import { uploadApi } from 'api/common';
import SiPreview from 'components/si-preview';
import { tableConst } from "const/index";
 import { getName } from 'utils/user';

export default {
  name: "MyComponent",
  components: {},
  props: {},
  data() {
    return {};
  },
  computed: {},
  watch: {},
  methods: {},
  mounted() {},
};
</script>

<style scoped lang="scss">

</style>
```

这里的对应规则如下：

```plaintext
      '@'          ===    src
      'api'        ===    src/api
      'assets'     ===    src/assets
      'components' ===    src/components
      'config'     ===    src/config
      'const'      ===    src/const
      'directive'  ===    src/directive
      'filters'    ===    src/filters
      'icons'      ===    src/icons
      'lang'       ===    src/lang
      'mock'       ===    src/mock
      'router'     ===    src/router
      'store'      ===    src/store
      'styles'     ===    src/styles
      'utils'      ===    src/utils
      'views'      ===    src/views
```

### 3、处理通用数据

 通用的数据维护在src --> const 文件中

 目前const文件中的内容如下：

```js
export const validatorRuleSetNum = (rule, value, callback) => {
  const reg = /^[A-Za-z][A-Za-z0-9_]*$/;
  if (reg.test(value)) {
    callback();
  } else {
    return callback(new Error('111'));
  }
};

export const validatorCustomTime = (rule, value, callback) => {
  if (this.nameTableFieldForm.timeType === 'customTime' && (value === 'effectiveTime' || value === 'expiredTime')) {
    callback(new Error('111'));
  } else {
    callback();
  }
};

export const runAllFunctions = async(val) => {
  try {
    await Promise.all([...val]);
  } catch (error) {
    console.error(error);
  }
};

export const ruleSetForm = () => {
  return {
    ruleSetNum: '',
    ruleSetName: '',
    ruleSetDesc: '',
    ruleSetCategory: null,
    ruleList: [{
      ruleId: '',
      isShortCircuit: 0,
      priority: 100,
    }],
  };
};

export const queryFormConst = () => {
  return {
    ruleSetName: '',
    createBy: '',
    ruleSetStatus: '',
    ruleSetCategory: null,
    domainCode: '',
  };
};

export const tableConst = () => {
  return {
    data: [],
    total: 0,
    columns: [],
    loading: false,
  };
};

export const FileldTableFieldForm = () => {
  return {
    fieldName: '',
    fieldType: 'varchar',
    fieldDesc: '',
    fieldDescEn: '',
    decimalLength: null
  };
};

export const DialogFormInfo = () => {
  return {
    timezoneIndex: '',
    globalTimeZone: '',
    zoneName: '',
  };
};
```
在页面中查找，发现有相同的变量，可以直接替换。

举例：定义了很多table，里面的每个参数的值都是
```js
    data: [],
    total: 0,
    columns: [],
    loading: false,
```

这时候我们就可以替换，这里重复的代码，使用const中的tableConst函数。
![alt text](../../public//img/const_use.jpg)



## 三、风控治理概述

##### 🌟 线上BUG处理
- OMC所有微应用的网站图标 `favicon `使用错误，均使用了谷歌图标。目前风控替换为`payermax`的图标，后续确定是否修改模版文件覆盖, 以及增加其他子应用；
- 规则双语显示问题；
- 页面文案写错；

----

##### 🌟 增加功能
- 流程图增加 撤销，恢复能力； 

----

##### 🌟 代码优化
- 优化无依赖接口与同步代码执行，使用并行模式；
- 优化流程图冗余创建锚点、边、卡片内容；
- 优化规则与角色创建中的大量重复代码；


----

##### 🌟  性能处理
- 处理完控制台`50`个告警；
- 增加上百个`key `增加页面渲染性能；
- 处理大量 `v-for `与 `template` 与 `v-if` 交互问题；

- 增加了 `happypack` 多线程打包，提高打包进程；
- 在`dev`环境与`product`的环境删除不存在的test文件解析，提高性能；
- 项目在非乾坤的自运行环境，加载了两个非常大的库，富文本编辑器与`PDF`预览处理。而加载过程是同步加载，会严重阻塞渲染。现改为HTML解析时异步并行下载，`HTML`解析完成后再执行。大幅提高解析进度；

----

##### 🌟  代码减少
- 替换上`321`处冗余变量；
- 替换`121`冗余校验；
- 删除`72`处使用`CSS`；

- 开启`tree-shakeing` 大幅删除冗余代码；
- 修改字体类型处理`loader`，`1KB`内打印 `base 64`，超过走`HTTP`请求，打包体积减少8.26%；`mp4|webm|ogg|mp3|wav|flac|aac` 等媒体文件，不存放在代码中，全部走请求，减少项目代码体积；
- 项目中的`Source map`使用不对，目前开发、生产、`mock`环境都使用的是`cheap-source-map`。`cheap-source-map`可以用与生产环境，第一次构建时间一搬，二次编译时间耗时较长。切生成map文件较大。目前`Source map`一共使用12种，每种方式和效率各不相同；目前对`dev`环境与生产环境做的区分。官方推荐开发环境使用`eval`模式，第一次构建速度最快，二次打包速度也是最快。不在生成`Source map`文件，打包体积减少约35%；

----

##### 🌟  关键数据 + 关键页面 
<br/>

 - ###### 项目编译提升

| 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 项目编译性能 | 26.02 S ~ 27.96 S | 16.87S  ~ 21.92 S | 

  - ###### 案件列表性能提升

| 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 页面加载时长 | 8.35S~10.53S | 3.53S ~ 6.56S | 
| 请求资源数 | 55  | 51 | 

  - ###### 流程图体积减小

| 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 流程图详情页 | 320行 | 288（环比减少10%） | 
| 节点注册页 | 750行  | 326行（环比减少56.53%） | 
| 详情节点注册页 | 142行  | 74行（环比减少47.89%） | 

  - ###### 流程图创建渲染速度提升

  | 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 第一次内容渲染 | 30.6 s | 15.5 s | 
| 最大内容渲染 | 34.2 s   | 16.6s  | 
| 总阻塞时长 | 1050 ms  | 120ms | 
| 速度指数 |  30.6s  | 10.5s | 


  - ###### 规则体积缩小

| 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 规则集页 | 805行   | 628（环比减少21.98%） | 
| 规则页   | 432行  | 381行（环比减少11.80%） | 
| 名单库主 | 363行  | 295行（环比减少18.73%） | 
| 名单库辅 | 496行  | 438行（环比减少11.69%） | 

 - ###### 规则性能提升（加载时长）
 | 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 规则列表  | 6.35S~15S      | 3.56s ~ 4.53S | 
| 规则页    | 6.76S~9.77S    | 3.25S ~ 5.53S | 
| 名单库    | 7.76S~ 14.85S  | 3.54S~ 5.45S | 

 - ###### 规则性能提升（请求资源数）
 | 说明         | 原始参数     | 现在参数     |
| ---- | ---- | ---- | 
| 规则列表  | 79     | 52 | 
| 规则页    | 57    | 51 | 
| 名单库    | 80  | 50 | 




