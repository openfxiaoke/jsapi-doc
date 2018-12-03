## Webview  

### URL传参控制  
由于JS-API在页面加载后才会被执行，而对于Webview的一些设置需要在页面加载成功前执行，比如某些重要页面需要在用户看到之前验证其身份、或是在Webview加载网页之前即显示定制的导航栏背景色。
纷享客户端支持通过在传入的URL后添加参数的方式控制Webview的某些行为或显示效果。

示例
```
http://www.fxiaoke.com/fsask/index.html?fs_auth=true&fs_auth_appName=纷享问问&fs_nav_bgcolor=c6a60000
```

参数说明

| 参数            | 类型    | 说明                      |
| ----------------| --------| -----|
| fs_nav_title    | String  | 控制导航栏标题文字，上限30个字。 |
| fs_nav_bgcolor  | String  | 控制导航栏颜色，格式为：RRGGBBAA，如`FAFAFAFF`。 |
| fs_nav_pbcolor  | String  | 控制导航栏进度条颜色，格式为：RRGGBBAA，如`FAFAFAFF`。 |
| fs_nav_fsmenu   | Boolean | 控制是否显示纷享菜单，true为显示，false为不显示，默认显示。 |
| fs_auth         | Boolean | 控制是否需对当前用户鉴权，如果为true则需要，否则不需要，默认false。 |
| fs_auth_appname | String  | 对当前用户鉴权的应用名，只有在`fs_auth=true`时使用，非空值需要进行URL encode。 |


### Webview跳转 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.back</td>
            <td>回退页面</td>
        </tr>
        <tr>
            <td>webview.close</td>
            <td>关闭当前页面</td>
        </tr>
        <tr>
            <td>webview.open</td>
            <td>打开页面</td>
            </tr>
        <tr>
            <td>webview.onBackWebview</td>
            <td>Android物理键回退回调</td>
            </tr>
        <tr>
            <td>webview.onCloseWebview</td>
            <td>页面关闭回调</td>
        </tr>
        <tr>
            <td>webview.redirect</td>
            <td>终端级灰度重定向</td>
        </tr>
    </tbody>
</table>


#### 回退页面
回退到上一个页面。

代码样例
```javascript
FSOpen.webview.back({

});
``` 

方法名：FSOpen.webview.back      
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  


#### 关闭当前页面

代码样例
```javascript
FSOpen.webview.close({
    extras: {
        data: 1
    }
});
``` 

方法名：FSOpen.webview.close      
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| extras    | Object      | 否   | 如果当前页面是通过`webview.open`接口打开，可通过`webview.close`接口关闭页面并传递参数`extras`到`webview.open`的`onClose`回调。 |


#### 打开页面

代码样例
```javascript
FSOpen.webview.open({
    url: 'https://www.fxiaoke.com/jsapi-demo',
    onClose: function(extras) {
        alert('open new window');
    }
});
``` 

方法名：FSOpen.webview.open      
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| url       | String      | 是   | 需要打开的页面链接地址 |
| onClose   | Function    | 是   | 打开的页面关闭后的回调。如果通过`webview.open`打开页面，可通过`webview.close`接口关闭页面并传递参数`extras`到`webview.open`的`onClose`回调。 |


#### Android物理键回退回调
仅Android适用。Android物理返回键返回上个页面时被调用。  

代码样例
```javascript
FSOpen.webview.onBackWebview({
    onBack: function() {
        alert('back to previous page！');
        FSOpen.webview.back();
    },
    onSuccess: function() {
        alert('现在请点击物理返回键');
    }
});
``` 

方法名：FSOpen.webview.onBackWebview      
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| onBack    | Function    | 是   | 物理键点击事件回调。如开发者声明了`onBack`，则需注意在最后根据需要调用`FSOpen.webview.back()`实现返回上一页，否则Webview不会自动返回。如不声明`onBack`，Webview会自动返回上一页。 |


#### 页面关闭回调
Webview窗口被关闭时回调，包括顶部栏关闭按钮及物Android物理返回键关闭。

代码样例
```javascript
FSOpen.webview.onCloseWebview({
    onClose: function() {
        alert('我要关闭了！');
        FSOpen.webview.close();
    },
    onSuccess: function() {
        alert('现在请点击回退按钮退出当前页面');
    }
});
``` 

方法名：FSOpen.webview.onCloseWebview       
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| onClose    | Function    | 是   | 物理键点击事件回调。如开发者声明了`onClose`，则需注意在最后根据需要调用`FSOpen.webview.close()`实现关闭当前页，否则Webview不会自动关闭。如不声明`onClose`，Webview会自动关闭当前页。 |

#### 终端级灰度重定向
终端级灰度重定向。

代码样例
```javascript
FSOpen.webview.redirect({
    url:'fs://crm/objectlist/StockObj'
    onSuccess: function(resp) {
        alert(resp.url);
    }
});
``` 

方法名：FSOpen.webview.redirect       
JS版本：2.2.0   
客户端支持版本：6.5.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| url    | String    | 是   | 需要进行重定向的url |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| url          | String | 灰度后的url |

### 屏幕控制 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.setOrientation</td>
            <td>屏幕翻转</td>
        </tr>
    </tbody>
</table>

#### 屏幕翻转

代码样例
```javascript
FSOpen.webview.setOrientation({
    orientation: 'portrait'
});
``` 

方法名：FSOpen.webview.setOrientation      
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数        | 类型        | 必须 | 说明         |
| ------------| ------------| -----| -------------|
| orientation | String      | 否   | `portrait`代表锁定屏幕为竖屏，`landscape`代表锁定屏幕为横屏，`useSysConfig`代表跟随系统设定。默认为portrait。 |


### 导航栏 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.navbar.setTitle</td>
            <td>设置导航栏标题</td>
        </tr>
        <tr>
            <td>webview.navbar.setMiddleBtn</td>
            <td>设置导航栏问号链接</td>
        </tr>
        <tr>
            <td>webview.navbar.setLeftBtn</td>
            <td>设置导航栏左侧按钮</td>
        </tr>
        <tr>
            <td>webview.navbar.setRightBtns</td>
            <td>设置导航栏右侧按钮</td>
        </tr>
        <tr>
            <td>webview.navbar.removeRightBtns</td>
            <td>清除导航栏右侧所有按钮</td>
        </tr>
        <tr>
            <td>webview.navbar.showMenu</td>
            <td>显示导航栏右侧“更多”菜单</td>
        </tr>
        <tr>
            <td>webview.navbar.hideMenu</td>
            <td>隐藏导航栏右侧的“更多”菜单</td>
        </tr>
        <tr>
            <td>webview.navbar.createPopup</td>
            <td>创建导航级自定义菜单</td>
        </tr>
        <tr>
            <td>webview.navbar.removePopup</td>
            <td>移除导航级自定义菜单</td>
        </tr>
    </tbody>
</table>

#### 设置导航栏标题

代码样例
```javascript
FSOpen.webview.navbar.setTitle({
    title: '纷享JSAPI'
});
``` 

方法名：FSOpen.webview.navbar.setTitle        
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   

调用参数说明：   

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 要显示的标题文本，默认为空 |

#### 设置导航栏问号链接
可在导航栏标题文本后显示一个问号图标，并定义其点击后的跳转链接。

![image1][image-navbar]

代码样例
```javascript
FSOpen.webview.navbar.setMiddleBtn({
    onClick: function() {
        FSOpen.webview.open({
            url: 'https://www.fxiaoke.com/aboutus/index.html'
        });
    }
});
``` 

方法名：FSOpen.webview.navbar.setMiddleBtn        
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：   

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| onClick   | Function    | 否   | 定制按钮的点击回调 |

#### 设置导航栏左侧按钮

图示见[设置导航栏问号链接][navbar.setMiddleBtn]

代码样例
```javascript
FSOpen.webview.navbar.setLeftBtn({
    text: '关闭',
    onClick: function() {
        FSOpen.webview.close();
    }
});
``` 

方法名：FSOpen.webview.navbar.setLeftBtn        
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：   

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| text      | String      | 是   | 定制按钮的文本 |
| onClick   | Function    | 否   | 定制按钮的点击回调，如开发者声明了`onClick`，则需注意在最后根据需要调用`FSOpen.webview.close()`实现关闭当前页，否则Webview不会自动返回或关闭。如不声明`onClick`，Webview会自动关闭当前页。  |

#### 设置导航栏右侧按钮
可以在导航栏右侧显示1个或多个自定义按钮。超过两个按钮则会在更多菜单中显示这些按钮。  

图示见[设置导航栏问号链接][navbar.setMiddleBtn]  

代码样例
```javascript
var btns = [
    {btnId: '1', icon: 'fav', text: '收藏'},
    {btnId: '2', icon: 'add', text: '添加'}
];
FSOpen.webview.navbar.setRightBtns({
    items: btns,
    onClick: function(resp) {
        // {btnId: '1'}
        alert('点击了按钮：' + btns[resp.btnId - 1].text);
    }
});
``` 

方法名：FSOpen.webview.navbar.setRightBtns        
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：   

| 参数      | 类型          | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| items     | Array[Object] | 是   | 定制按钮的属性数组。超过两个按钮则会在更多菜单中显示这些按钮。如同一个页面调用了`FSOpen.webview.navbar.showMenu`，则`setRightBtns`中设置的按钮只显示第一个。 |
| onClick   | Function      | 否   | 定制按钮的点击回调。用户点击菜单的`btnId`会传入`onClick`。 |

`items`中每个`item`对象参数说明  

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| btnId     | String    | 是   | 每个`item`的唯一标示 |
| icon      | String    | 是   | 预置icon的值，优先级高于`text`参数，即优先显示图标。  |
| text      | String    | 是   | 每个`item`的文本标签 |

`item`对象中`icon`参数可选填值

 | 值         | 说明      |
 | -----------| ----------|
 | fav        | 收藏    |
 | add        | 添加    |
 | calendar   | 日历    |
 | search     | 搜索    |
 | delete     | 删除    |
 | scan       | 扫描    |
 | structure  | 架构   |
 | edit       | 编辑    |
 | group      | 群组    |
 | individual | 个人    |
 | more       | 更多    |
 | setting    | 设置    |
 | chat       | 聊天    |
 | save       | 保存    |

![image3][image-icons]

 `onClick`点击回调参数属性说明：

 | 参数      | 类型          | 说明     |
 | ----------| --------------| ---------|
 | btnId     | String        | `item`的id |

#### 清除导航栏右侧所有按钮
注意此接口只会删除掉通过`FSOpen.webview.navbar.setRightBtns`设定的按钮，不会删除`FSOpen.webview.navbar.showMenu`设定的菜单。  

代码样例
```javascript
FSOpen.webview.navbar.removeRightBtns();
``` 

方法名：FSOpen.webview.navbar.removeRightBtns      
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

#### 显示"更多"菜单
显示一个纷享提供的“更多”菜单，菜单中预置了一些常用的功能实现，比如收藏、转发到企信、转发到分享、转发到微信等。   
![image2][image-fsmenu]

通常开发者仅需自定义需要显示的菜单列表，无需再为每个功能提供实现，但如果有自己的特殊需求，也可以自定义实现各菜单的全局回调函数，详情可参考本文**Webview-“更多”菜单回调**章节。   

代码样例
```javascript
FSOpen.webview.navbar.showMenu({
    menuList: [
        'service:favorite',
        'share:toConversation',
        'share:toFeed',
        'separator',
        'share:toCRMContact',
        'share:toWXFriend',
        'share:toWXMoments',
        'share:toQQFriend',
        'share:viaMail',
        'share:viaSMS',
        'separator', //分隔符，可根据需要多次使用
        'page:refresh',
        'page:copyURL',
        'page:generateQR',
        'page:openWithBrowser'
    ]
});
``` 

方法名：FSOpen.webview.navbar.showMenu          
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   

调用参数说明：   

| 参数      | 类型          | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| menuList  | Array[String] | 是   | 定制的菜单项`menuItem`列表，默认全部显示 |

`menuItem`说明：

| 参数                 | 说明             |
| ---------------------| -----------------|
| service:favorite     | 收藏当前页面     |
| share:toConversation | 转发到企信       |
| share:toFeed         | 转发到工作流     |
| share:toCRMContact   | 转发给CRM联系人  |
| share:toWXFriend     | 分享给微信好友   |
| share:toWXMoments    | 分享到微信朋友圈 |
| share:toQQFriend     | 分享给QQ好友     |
| share:viaMail        | 通过邮件转发     |
| share:viaSMS         | 通过短信转发     |
| page:refresh         | 页面刷新         |
| page:copyURL         | 复制链接         |
| page:generateQR      | 生成二维码       |
| page:openWithBrowser | 在浏览器中打开   |
| separator            | 分隔符，用来分组菜单项，可根据需要同时在多个位置使用。 |


#### 隐藏"更多"菜单
    
代码样例
```javascript
FSOpen.webview.navbar.hideMenu();
``` 

方法名：FSOpen.webview.navbar.hideMenu      
JS版本：2.0.0   
客户端支持版本：5.4.0及以上    


#### 创建导航级自定义菜单
可以在导航栏右侧创建一个浮层菜单。  

代码样例
```javascript
var items = [
    {menuId: '1', text: '收藏'},
    {menuId: '2', text: '添加'}
];
FSOpen.webview.navbar.createPopup({
    icon: 'more',
    text: '更多',
    items: items,
    onClick: function(resp) {
        alert('点击了菜单：' + resp.menuId);
    }
});
``` 

方法名：FSOpen.webview.navbar.createPopup        
JS版本：2.2.0   
客户端支持版本：6.3.2及以上   

调用参数说明：   

| 参数      | 类型          | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| icon     | String       | 是   | 显示在导航栏上的图标 |
| text     | String       | 是   | 显示在导航栏上的文本 |
| items     | Array[Object] | 是   | 定制按钮的属性数组。 |
| onClick   | Function      | 否   | 定制按钮的点击回调。用户点击菜单的`menuId`会传入`onClick`。 |

`items`中每个`item`对象参数说明  

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| menuId    | String    | 是   | 每个`item`的唯一标示 |
| text      | String    | 是   | 每个`item`的文本标签 |

 `onClick`点击回调参数属性说明：

 | 参数      | 类型          | 说明     |
 | ----------| --------------| ---------|
 | menuId    | String        | `item`的id |



#### 移除导航级自定义菜单
    
代码样例
```javascript
FSOpen.webview.navbar.removePopup();
``` 

方法名：FSOpen.webview.navbar.removePopup      
JS版本：2.2.0   
客户端支持版本：6.3.2及以上    


#### 显示“更多”菜单列表
    
代码样例
```javascript
FSOpen.webview.navbar.showMenuList();
``` 

方法名：FSOpen.webview.navbar.showMenuList      
JS版本：2.1.4   
客户端支持版本：5.6及以上    


### “更多”菜单回调 
通过`FSOpen.webview.navbar.showMenu`设置的“更多”菜单，在用户点击的时候会触发对应的全局回调处理。开发者可实现这些回调以自定义具体（转发）的内容信息。   

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.menu.onShareToConversation</td>
            <td>“更多”菜单回调：转发到企信</td>
        </tr>
            <tr>
            <td>webview.menu.onShareToFeed</td>
            <td>“更多”菜单回调：转发到“工作”</td>
        </tr>
            <tr>
            <td>webview.menu.onShareToCRMContact</td>
            <td>“更多”菜单回调：转发到CRM联系人</td>
        </tr>
            <tr>
            <td>webview.menu.onShareToWXFriend</td>
            <td>“更多”菜单回调：转发给微信好友</td>
        </tr>
            <tr>
            <td>webview.menu.onShareToWXMoments</td>
            <td>“更多”菜单回调：转发到微信朋友圈</td>
        </tr>
            <tr>
            <td>webview.menu.onShareToQQFriend</td>
            <td>“更多”菜单回调：转发给QQ好友</td>
        </tr>
            <tr>
            <td>webview.menu.onShareViaSMS</td>
            <td>“更多”菜单回调：通过短信转发</td>
        </tr>
            <tr>
            <td>webview.menu.onShareViaMail</td>
            <td>“更多”菜单回调：通过邮件转发</td>
        </tr>
    </tbody>
</table>

#### “更多”菜单回调：转发到企信     

代码样例
```javascript
FSOpen.webview.menu.onShareToConversation({
    title: '纷享逍客',
    desc: '移动办公 自在纷享',
    link: 'http://www.fxiaoke.com',
    imgUrl: 'https://www.fxiaoke.com/static/img/index/logo.png?v=5.1.5',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToConversation   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 分享标题，默认当前页面标题(`<title>`标签内容) |
| desc      | String      | 否   | 分享摘要描述，默认当前页面描述（`<meta name="description" content="网页摘要内容在这里填写">`标签中的内容），取不到则显示页面URL。 |
| link      | String      | 否   | 分享链接地址，默认当前页面链接 |
| imgUrl    | String      | 否   | 分享缩略图地址，默认为纷享预置图标|

#### “更多”菜单回调：转发到工作流     

代码样例
```javascript
FSOpen.webview.menu.onShareToFeed({
    title: '纷享逍客',
    desc: '移动办公 自在纷享',
    link: 'http://www.fxiaoke.com',
    imgUrl: 'https://www.fxiaoke.com/static/img/index/logo.png?v=5.1.5',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToFeed   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 分享标题，默认当前页面标题 |
| desc      | String      | 否   | 分享摘要描述，默认当前页面描述（`<meta name="description" content="网页摘要内容在这里填写">`标签中的内容），取不到则显示页面URL。  |
| link      | String      | 否   | 分享链接地址，默认当前页面链接 |
| imgUrl    | String      | 否   | 分享缩略图地址，默认为纷享内置图标 |

#### “更多”菜单回调：转发给CRM联系人     

代码样例
```javascript
FSOpen.webview.menu.onShareToCRMContact({
    link: location.href,
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToCRMContact   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| link      | String      | 否   | 要转发的页面链接 |

#### “更多”菜单回调：分享给微信好友     

代码样例
```javascript
FSOpen.webview.menu.onShareToWXFriend({
    title: '纷享逍客',
    link: 'http://www.fxiaoke.com',
    imgUrl: 'https://www.fxiaoke.com/static/img/index/logo.png?v=5.1.5',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToWXFriend   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 分享标题，默认当前页面标题 |
| link      | String      | 否   | 分享链接地址，默认当前页面链接 |
| imgUrl    | String      | 否   | 分享缩略图地址，默认为纷享内置图标 |

#### “更多”菜单回调：分享到微信朋友圈     

代码样例
```javascript
FSOpen.webview.menu.onShareToWXMoments({
    title: '纷享逍客',
    link: 'http://www.fxiaoke.com',
    imgUrl: 'https://www.fxiaoke.com/static/img/index/logo.png?v=5.1.5',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToWXMoments   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 分享标题，默认当前页面标题 |
| link      | String      | 否   | 分享链接地址，默认当前页面链接 |
| imgUrl    | String      | 否   | 分享缩略图地址，默认为纷享内置图标 |

#### “更多”菜单回调：分享给QQ好友     

代码样例
```javascript
FSOpen.webview.menu.onShareToQQFriend({
    title: '纷享逍客',
    desc: '移动办公 自在纷享',
    link: 'http://www.fxiaoke.com',
    imgUrl: 'https://www.fxiaoke.com/static/img/index/logo.png?v=5.1.5',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareToQQFriend   
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 分享标题，默认当前页面标题 |
| desc      | String      | 否   | 分享摘要描述，默认当前页面标题 |
| link      | String      | 否   | 分享链接地址，默认当前页面链接 |
| imgUrl    | String      | 否   | 分享缩略图地址，默认为纷享内置图标 |

#### “更多”菜单回调：通过短信转发     

代码样例
```javascript
FSOpen.webview.menu.onShareViaSMS({
    content: '移动办公，自在纷享 {url}',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareViaSMS   
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| content   | String      | 否   | 转发内容，最多140字，可使用`{url}`(5个字符)来表示当前页面的URL，后台会在最终内容中替换。 |

#### “更多”菜单回调：通过邮件转发     

代码样例
```javascript
FSOpen.webview.menu.onShareViaMail({
    title: '纷享逍客',
    content: '移动办公，自在纷享 {url}',
    onSuccess: function(resp) {
        // 可以在这里做些分享数据统计
    },
    onFail: function(error) {
        if (error.errorCode == 40050) {
            alert('用户取消分享');
            return;
        }
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.webview.menu.onShareViaMail   
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   

调用参数说明：    

| 参数      | 类型        | 必须 | 说明         |
| ----------| ------------| -----| -------------|
| title     | String      | 否   | 转发邮件标题 |
| content   | String      | 否   | 转发邮件内容，可使用`{url}`(5个字符)来表示当前页面的URL，后台会在最终内容中替换。 |


### 页面 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.page.copyURL</td>
            <td>复制当前页面链接</td>
        </tr>
            <tr>
            <td>webview.page.generateQR</td>
            <td>生成当前页面二维码</td>
        </tr>
            <tr>
            <td>webview.page.openWithBrowser</td>
            <td>用浏览器打开当前页面</td>
        </tr>
            <tr>
            <td>webview.page.refresh</td>
            <td>刷新页面</td>
        </tr>
    </tbody>
</table>

#### 复制当前页面链接     

代码样例
```javascript
FSOpen.webview.page.copyURL();
``` 

方法名：FSOpen.webview.page.copyURL   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

#### 生成当前页面二维码     

代码样例
```javascript
FSOpen.webview.page.generateQR();
``` 

方法名：FSOpen.webview.page.generateQR   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

#### 用浏览器打开当前页面     

代码样例
```javascript
FSOpen.webview.page.openWithBrowser();
``` 

方法名：FSOpen.webview.page.openWithBrowser   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

#### 刷新页面     

代码样例
```javascript
FSOpen.webview.page.refresh();
``` 

方法名：FSOpen.webview.page.refresh  
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   


### Bounce 
此类接口仅限于iOS系统。  

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.bounce.enable</td>
            <td>启用Bounce</td>
        </tr>	
            <tr>
            <td>webview.bounce.disable</td>
            <td>禁用Bounce</td>
        </tr>
    </tbody>
</table>

#### 启用webview的bounce效果     

代码样例
```javascript
FSOpen.webview.bounce.enable();
``` 

方法名：FSOpen.webview.bounce.enable    
JS版本：2.0.0    
客户端支持版本：5.4.0及以上   

#### 禁用webview的bounce效果     

代码样例
```javascript
FSOpen.webview.bounce.disable();
``` 

方法名：FSOpen.webview.bounce.disable   
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   


### 下拉刷新 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>webview.pullRefresh.enable</td>
            <td>启用下拉刷新</td>
        </tr>	
            <tr>
            <td>webview.pullRefresh.disable</td>
            <td>禁用下拉刷新</td>
        </tr>	
            <tr>
            <td>webview.pullRefresh.stop</td>
            <td>停止刷新</td>
        </tr>
    </tbody>
</table>

#### 启用下拉刷新     
开启后，用户可在页面里下拉（刷新）。

代码样例
```javascript
FSOpen.webview.pullRefresh.enable({
    onPullRefresh: function() {
        setTimeout(function() {
            alert('数据加载成功');
            FSOpen.webview.pullRefresh.stop();
        }, 3000);
    },
    onSuccess: function() {
        alert('现在下拉试试');
    }
});
``` 

方法名：FSOpen.webview.pullRefresh.enable    
JS版本：2.0.0    
客户端支持版本：5.4.0及以上    

调用参数说明：  

| 参数          | 类型     | 必须 | 说明         |
| --------------| ---------| -----| -------------|
| onPullRefresh | Function | 否   | 用户下拉到临界值时触发的回调。可在此回调里进行一些延时操作，并在延时操作完毕后通过对应的`webview.pullRefresh.stop`来还原。 |   

#### 禁用下拉刷新     

代码样例
```javascript
FSOpen.webview.pullRefresh.disable();
``` 

方法名：FSOpen.webview.pullRefresh.disable   
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

#### 收起下拉刷新     

代码样例
```javascript
FSOpen.webview.pullRefresh.stop();
``` 

方法名：FSOpen.webview.pullRefresh.stop    
JS版本：2.0.0   
客户端支持版本：5.4.0及以上   


[webview.callback]: #{webvew.callback}
[navbar.setMiddleBtn]: #{navbar.setMiddleBtn}
[image-navbar]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/08/rBEiBlfZFEqAE_YFAADPpSDiJqY481.png
[image-fsmenu]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/09/rBEiBlfZGq6AZDdcAARZHjjCoBw598.png
[image-icons]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/01/50/rBEiBFfZFKSANSqSAAEPr-YTH1A189.png
