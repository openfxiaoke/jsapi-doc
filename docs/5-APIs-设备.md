## 设备类

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>device.authenticateUser</td>
            <td>对当前用户鉴权，支持指纹和纷享密码两种方式</td>
        </tr>
        <tr>
            <td>device.getAP</td>
            <td>获取接入点标识</td>
        </tr>
        <tr>
            <td>device.getNetworkType</td>
            <td>获取当前接入的网络类型：WiFi、2/3/4G</td>
        </tr>
        <tr>
            <td>device.getUUID</td>
            <td>获取设备唯一编码</td>
        </tr>
        <tr>
            <td>device.scan</td>
            <td>调用扫码</td>
        </tr>
        <tr>
            <td>device.vibrate</td>
            <td>手机震动</td>
        </tr>
        <tr>
            <td>device.selectWifi</td>
            <td>从当前的wifi列表选择某wifi</td>
        </tr>
        <tr>
            <td>device.scanCard</td>
            <td>扫名片</td>
        </tr>
        <tr>
            <td>device.call</td>
            <td>打电话</td>
        </tr>
    </tbody>
</table>

#### 对当前用户鉴权
调用该接口会弹出一个鉴权页面，用户需要验证指纹（支持Touch-ID并打开了指纹登录）、或输入当前登录用户的纷享密码才能通过鉴权。       

代码样例  
```javascript
FSOpen.device.authenticateUser({
    appName: '工资助手',
    onSuccess: function(resp){
        alert('认证成功');
    },
    onFail: function(error){
        if (error.errorCode === 40050) {
            alert('取消了认证');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.device.authenticateUser   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：   

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| appName   | String      | 是   | 当前应用名字 |


#### 获取接入点标识    

代码样例
```javascript
FSOpen.device.getAP({
    onSuccess: function(resp) {
        // ssid: 'FSDevLan'
        // macAddress: '3c:12:aa:09'
        alert('ssid = ' + resp.ssid + '\n' + 'macAddress = ' + resp.macAddress);
    }
});
``` 

方法名：FSOpen.device.getAP   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：     

| 参数       | 类型        | 说明                |
| -----------| ------------| --------------------|
| ssid       | String      | 热点SSID            |
| macAddress | String      | 热点MAC地址         |


#### 获取网络类型      

代码样例
```javascript
FSOpen.device.getNetworkType({
    onSuccess: function(resp) {
        // network: '3g'
        alert('network = ' + resp.network);
    }
});
``` 

方法名：FSOpen.device.getNetworkType    
JS版本：2.0.0   
客户端支持版本：5.4.0及以上  

成功回调返回参数：     

| 参数      | 类型        | 说明                |
| ----------| ------------| --------------------|
| network   | String      | 网络类型，取值可能为：`2g``3g``4g``wifi``unknown``none`，`none`表示离线。|


#### 获取设备唯一编码     

代码样例
```javascript
FSOpen.device.getUUID({
    onSuccess: function(resp) {
        // uuid: 'FD71A168-1CAD-4EF1-BECC-52997124207A'
        alert('uuid = ' + resp.uuid);
    }
});
``` 

方法名：FSOpen.device.getUUID   
JS版本：2.0.0   
客户端支持版本：5.4.0及以上    

成功回调返回参数：     

| 参数      | 类型        | 说明                |
| ----------| ------------| --------------------|
| uuid      | String      | 本机唯一识别码          |


#### 调用扫码     

代码样例
```javascript
FSOpen.device.scan({
    onSuccess: function(resp) {
        // text: 'https://www.fxiaoke.com/'
        alert('扫码内容：' + resp.text);
    }
});
``` 

方法名：FSOpen.device.scan   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

成功回调返回参数：     

| 参数        | 类型        | 说明                |
| ------------| ------------| --------------------|
| text        | String      | 扫码内容            |


#### 手机震动     

代码样例
```javascript
FSOpen.device.vibrate({
    duration: 3000
});
``` 

方法名：FSOpen.device.vibrate    
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| duration  | Number      | 否   | 震动时间，只对Android有效。单位毫秒，默认3秒。 |


#### 从当前的wifi列表选择某wifi       

代码样例
```javascript
FSOpen.device.selectWifi();
``` 

方法名：FSOpen.device.selectWifi    
JS版本：2.1.5   
客户端支持版本：5.7.0及以上   

成功回调返回参数：     

| 参数        | 类型        | 说明                |
| ------------| ------------| --------------------|
| name        | String      | 当前wifi名字        |


#### 扫名片       
调用扫名片JSAPI，会跳转到扫名片页面，扫描完名片后，会跳转到扫描结果页面，在这个页面可以新建联系人、新建客户、新建销售线索等，和CRM里面的扫名片功能

代码样例
```javascript
FSOpen.device.scanCard();
``` 

方法名：FSOpen.device.scanCard    
JS版本：2.2   
客户端支持版本：6.5.3及以上

#### 打电话       
打开系统的默认打电话APP，并传入默认的电话号码

代码样例
```javascript
FSOpen.device.call();
``` 

方法名：FSOpen.device.call()    
JS版本：2.2   
客户端支持版本：6.7及以上

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| tel  | String      | 是   | 电话号码 |
