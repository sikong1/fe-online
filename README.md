# Hixtrip FE Online

## 准备

- fork 此仓库
- 根据如下要求实现相关代码
- 完成要求
  - 提交`pull request`
  - 提供页面截图

## 需求

1. 目前已经有`src/api/user.ts` 以及 `src/api/org.ts` 两个 API
2. 需要实现如下效果界面：

![](./docs/preview.jpg)

> 功能要求：
- 不限 ```React``` 或者 ```Vue```
- 需要拆分 `OrgTree` 以及 `UserTable` 两个组件
  - 两个组件自己维护相关的数据。
  - 体现两个组件的互相通信。
- 组织架构根据点上级节点进行查询子级节点实现异步加载。
- 用户 ```Table ``` 数据跟据点击 ```组织架构树形节点``` 以及 ```输入搜索关键字``` 查询。
  - 需要考虑防抖节流等功能点
- 风格不限
  - 示例图仅仅是效果展示，不需要完全符合。
  - 可以使用 ```UI Framework```, 如 ```ant-design```, ```element-ui```等
  - 如果不用```UI Framework```, 可以直接用原生的```<ul> <li>```, ```<table>``` 实现，不用实现相关的CSS样式, 可以加分。

## 其他简答题

### 如何将如下的`JSON`正确解析成 `Object`
```json
{
  "userId": 111323290434354540545
}
```
```javascript
使用JSON.parse实现
```



### 前端需要*稳定*每隔`1s`向服务端请求`API`, 请问如何实现？

```javascript
function sendRequest() {
    fetch('接口url')
        .then(response => {
            // 处理响应
            if (!response.ok) {
                throw new Error('网络响应不正常');
            }
            return response.json();
        })
        .then(data => {
            // 处理数据
            console.log(data);
            
            // 在请求完成后设置下一个定时器，确保每隔1秒发送一次请求
            setTimeout(sendRequest, 1000);
        })
        .catch(error => {
            // 处理错误
            console.error(error);
            
            // 在出现错误时也设置下一个定时器，确保每隔1秒发送一次请求
            setTimeout(sendRequest, 1000);
        });
}

// 开始发送第一次请求
sendRequest();
```



### 什么情况下，你会为你的项目引入状态管理库，比如`Redux`, `Pinia`, 可以简述一下起到了什么作用么？

```
比如登录流程，很多地方用到就会使用 pinia，并且 pinia 有持久化缓存插件，只要改变 pinia 的数据，本地缓存会同步修改。
还有就是两个组件需要通信，但是层级过深（一般不是父子组件或兄弟组件），我也会选择 pinia 存储，统一管理数据，但是业务逻辑不建议在 pinia 中实现，需求变更维护起来很麻烦。

作用：
统一维护数据，不需要一层一层去寻找数据来源。
而且具有响应式，方便操作。
持久化插件、调试插件等。
```



### 为什么`ESM`与`CJS`不能兼容？

```
1.语法不同，ESM 是 export 导出，而 CJS 是 module.exports。
2.ESM 是静态加载（编译的时候就会抛出错误），CJS是动态加载的（运行的时候才会抛出错误）。
3.ESM是严格作用域，而CJS是非严格作用域
```

