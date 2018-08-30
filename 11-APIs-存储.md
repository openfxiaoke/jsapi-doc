## 媒体

### 文件 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>storage.getItem</td>
            <td>获取缓存数据</td>
        </tr>
        <tr>
            <td>storage.setItem</td>
            <td>设置缓存数据</td>
        </tr>
        <tr>
            <td>storage.removeItem</td>
            <td>删除缓存数据</td>
        </tr>
        <tr>
            <td>storage.clearItems</td>
            <td>清空缓存数据</td>
        </tr>
    </tbody>
</table>

#### 获取缓存数据

代码样例
```javascript
FSOpen.storage.getItem({
    name: 'test',
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.storage.getItem     
JS版本：2.1.5  
客户端支持版本：5.7.0及以上  

调用参数说明：    

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| name      | String    | 是   | 缓存Key  |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| value          | String | 缓存的对象字符串，需要业务方自己序列化 |


#### 设置缓存数据

代码样例
```javascript
FSOpen.storage.setItem({
    name: 'test',
    value: JSON.stringify({a:1,b:2}),
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.storage.setItem     
JS版本：2.1.5  
客户端支持版本：5.7.0及以上  

调用参数说明：    

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| name      | String    | 是   | 缓存Key  |
| value     | String    | 是   | 缓存Value  |


#### 删除缓存数据

代码样例
```javascript
FSOpen.storage.removeItem({
    name: 'test',
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.storage.removeItem     
JS版本：2.1.5  
客户端支持版本：5.7.0及以上  

调用参数说明：    

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| name      | String    | 是   | 缓存Key  |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| value          | String | 缓存的对象字符串，需要业务方自己序列化 |


#### 清空缓存数据

代码样例
```javascript
FSOpen.storage.clearItems({
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.storage.clearItems     
JS版本：2.1.5  
客户端支持版本：5.7.0及以上  




