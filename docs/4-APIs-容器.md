## 容器

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>runtime.getVersion</td>
            <td>获取终端版本号</td>
        </tr>
        <tr>
            <td>runtime.getCurrentUser</td>
            <td>获取当前用户信息</td>
        </tr>
        <tr>
            <td>runtime.requestAuthCode</td>
            <td>获取临时授权码用于免登业务</td>
        </tr>
        <tr>
            <td>runtime.showUpdate</td>
            <td>提示版本升级</td>
        </tr>
        <tr>
            <td>runtime.getPhoneInfo</td>
            <td>获取手机信息</td>
        </tr>
    </tbody>
</table>

#### 获取终端版本号   

代码样例
```javascript
FSOpen.runtime.getVersion({
    onSuccess: function(resp){
        alert('当前终端版本号是：' + resp.ver);
    },
    onFail: function(error){
        alert('接口调用失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.runtime.getVersion  
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：    

| 参数      | 类型        | 说明                |
| ----------| ------------| --------------------|
| ver       | String      | 版本号信息，如'5.3.0'。 |

#### 获取当前用户信息   
  
代码样例
```javascript
FSOpen.runtime.getCurrentUser({
    onSuccess: function(resp){
        alert('当前用户信息：' + JSON.stringify(resp));
    },
    onFail: function(error){
        alert('接口调用失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.runtime.getCurrentUser    
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：    

| 参数      | 类型        | 说明                |
| ----------| ------------| --------------------|
| user      | Object      | 返回当前用户的具体信息，详细字段如下表所示。 |

`user`对象参数说明：  

| 参数      | 类型          | 说明         |
| ----------| --------------| -------------|
| userId    | String        | 用户ID  |
| nickname  | String        | 用户昵称     |
| email     | String        | 用户电子邮箱 |
| avatarUrl | String        | 用户头像     |
| position  | String        | 用户职位信息 |
| corpId    | String        | 企业号ID |
| corpName  | String        | 企业号中文名称 |
| corpAccount | String        | 企业号帐号名 |

#### 获取免登授权码
免登授权码主要用于通过服务器接口换取用户身份(用户ID)，参考[身份验证接口](http://open.fxiaoke.com/wiki.html#artiId=19) 。

代码样例  
```javascript
FSOpen.runtime.requestAuthCode({
    onSuccess: function(resp) {
        alert('免登授权码信息：' + JSON.stringify(resp));
    },
    onFail: function(error) {
        alert('接口调用失败，错误码：' + error.errorCode);
    }
});
```  

方法名：FSOpen.runtime.requestAuthCode   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：     

| 参数       | 类型        | 说明                |
| -----------| ------------| --------------------|
| code       | String      | 免登授权码，主要用于通过服务器接口换取用户身份。 |

客户端同时提供一个更为便捷的JS API，可直接获取到当前用户信息，请参考接口**获取当前用户信息** `FSOpen.runtime.getCurrentUser  `。

#### 提示版本升级    
弹出升级提示，在Android上用户点击确定后可跳转到应用市场直接升级。  

代码样例
```javascript
FSOpen.runtime.showUpdate({
    message: '当前版本有更新。'
});
``` 

方法名：FSOpen.runtime.showUpdate  
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明                |
| ----------| ------------| -----| --------------------|
| message   | String      | 是   | 升级提示文本 |


方法名：FSOpen.runtime.getVersion  
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：    

| 参数      | 类型        | 说明                |
| ----------| ------------| --------------------|
| ver       | String      | 版本号信息，如'5.3.0'。 |

#### 获取当前手机信息   
  
代码样例
```javascript
FSOpen.runtime.getPhoneInfo ({
    onSuccess: function(resp){
        alert('当前手机信息：' + JSON.stringify(resp));
    },
    onFail: function(error){
        alert('接口调用失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.runtime.getPhoneInfo    
JS版本：2.2  
客户端支持版本：6.7.3及以上  

成功回调返回参数：    

| 参数      | 类型          | 说明         |
| ----------| --------------| -------------|
| system    | String        | 手机系统，比如android,ios  |
| brand    | String        | 手机品牌，比如xiaomi  |
| model  | String        | 手机型号，比如mi9     |
| version     | String        | 手机系统版本 |
| netInfo | String        | 手机网络信息，比如wifi,4g,3g等     |
| webViewCore  | String        | webview内核 |