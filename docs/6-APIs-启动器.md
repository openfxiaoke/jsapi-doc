## 启动器

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>launcher.checkAppInstalled</td>
            <td>检测应用是否安装</td>
        </tr>
        <tr>
            <td>launcher.launchApp</td>
            <td>启动第三方应用</td>
        </tr>
    </tbody>
</table>

#### 检测应用是否安装
 
代码样例
```javascript
FSOpen.launcher.checkAppInstalled({
    scheme: 'taobao',
    pkgName: 'com.alibaba.taobao',
    onSuccess: function(resp) {
        // installed: true
        if (resp.installed) {
            alert('本机安装了应用');
        } else {
            alert('本机未安装应用');
        }
    }
});
``` 

方法名：FSOpen.launcher.checkAppInstalled   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| scheme    | String      | 是   | iOS使用，应用scheme。 |
| pkgName   | String      | 是   | Android使用，应用包名。 |

成功回调返回参数：     

| 参数      | 类型            | 说明                |
| ----------| ----------------| --------------------|
| installed | Boolean         | 检测结果 |


#### 启动第三方应用    

代码样例
```javascript
FSOpen.launcher.launchApp({
    scheme: 'taobao',
    pkgName: 'com.alibaba.taobao',
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        if (error.errorCode === 40010) {
            alert('未安装该应用');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.launcher.launchApp   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| scheme    | String      | 是   | iOS使用，应用scheme。 |
| pkgName   | String      | 是   | Android使用，应用包名。 |
| activityName | String   | 是   | Android使用。 |
