---
outline: deep
---

### ExportExcel-导出Excel表格

#### 说明
  作用：封装导出数据到excel功能
#### 基础用法

```html{2}
<!-- 组件使用 -->
    <ExportExcel
       :eParams="mandatory 协议请求参数"
       :eHeaders="mandatory excel导出文件头"
       :eTotalNumber="mandatory 总的条数"
       :eFileName="optional 文件名称"
       :ePageSize="optional 每页大小"
       :sendRequest="mandatory 发送请求函数"
       :convertData="optional 请求数据转换函数"
       @onStart="()=>{}"
       @onProgress="()=>{}"
       @onFinish="()=>{}"
    ></ExportExcel>
```

```js{2,7}
// 引入组件
import ExportExcel from '@/components/ExportExcel/index.vue';

export default {
  // 组件注册
  components: {
    ExportExcel
  },
}
```

#### 参数说明
| 参数名            | 类型      | 是否必填  | 默认值 | 说明 | 
| ---- | ---- | ---- |  ---- | ---- |
| eParams | string   | 是      | 无 | mandatory 协议请求参数 | 
| eHeaders | string   | 是       | 无 | mandatory excel导出文件头 | 
| eTotalNumber | Number  | 是      | 无 | mandatory 总的条数 | 
| eFileName | string   | 是       | 无 | optional 文件名称 | 
| ePageSize | Number   | 是       | 无 | optional 每页大小 | 
| sendRequest | Function  | 是       | 无 | mandatory 发送请求函数 | 
| convertData | Function   | 是      | 无 | optional 请求数据转换函数 | 
| onStart | Function   | 否       | 无 | 开始导出Excel | 
| onProgress | Function  | 否       | 无 | 正在导出Excel | 
| onFinish | Function   | 否       | 无 | 结束导出Excel | 

#### 效果展示







