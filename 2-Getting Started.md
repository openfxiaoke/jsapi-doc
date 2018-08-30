## Getting Started

### 引入JS文件和CSS文件  
1， 如果仅应用纷享**JS API**，可以选择直接在Web页面在线引用对应JS文件和CSS文件：
```html
<!-- 纷享JS API核心JavaScript文件 -->
<script src="http://www.fxiaoke.com/open/jsapi/2.1.3/fsapi.min.js"></script>

<!-- 纷享JS UI组件库CSS文件 -->
<link rel="stylesheet" href="http://www.fxiaoke.com/open/assets/css/1.0.0/fsui.css"/>
```
或是将JS文件和CSS文件下载到本地，放到应用站点的本地目录中引用。
```html
<!-- 纷享JS API核心JavaScript文件 -->
<script src="/js/fsapi.min.js"></script>

<!-- 纷享JS UI组件库CSS文件 -->
<link rel="stylesheet" href="/assets/css/fsui.css"/>

```
 
成功加载JS文件后，可直接使用全局变量`FSOpen`调用API，如`FSOpen.runtime.getVersion`。   


2， 如需应用纷享**UI组件库**，还需另下载对应的JS文件和CSS文件，放在站点的本地目录中引用。下载下来的压缩包需先解压，其中`uikit.js`和`uikit.css`包含所有的组件定义。如仅应用了部分组件，也可以仅引用`components`文件夹中对应的分类js文件和css文件，以加快页面加载速度。  

[**UI组件库下载地址**][uikitdownload]


### JS API使用约定
**所有接口均接受一个JSON object参数，基本格式如下：**
```
{
    param1: 'xxx',  // 接口参数1（示例）
    ...
    paramn: 'xxx',  // 接口参数n（示例）
    onSuccess: function(resp) {
        // 成功回调处理...
    },
    onFail: function(error) {
        // 失败回调处理...
    }
}
``` 

**所有成功回调（如有）返回如下内容：**
```
{
    result1: 'xxx',  // 返回参数1（示例）
    ...
    resultn: 'xxx',  // 返回参数n（示例）
}
``` 

**所有错误回调（如有）返回如下内容：**
```
{
    errorCode: 40000, // 错误码
    errorMessage: '请求参数错误'  // 错误描述
}
``` 

错误码可参考[错误码汇总表][nav.appendix.error]。


### 初始化API运行环境
在使用JS API之前，必须使用`FSOpen.init`方法对运行环境进行预配置，否则接口调用会出错。
代码样例
```
FSOpen.init({
    appId: 'FSAID_1313de7',
    timestamp: '123456',
    nonceStr: 'randxxxx',
    signature: '784E655546BCEC2D5B8392B5998FE1403B6DD0DA',

    onSuccess: function(resp) {
        // 初始化成功回调. 所有JS API执行必须在初始化成功后，否则会出错。
    },
    onFail: function(error) {
        // 失败回调处理
        if (error.errorCode === 30000) {
            alert('请更新纷享客户端到最新版本。');
        } else {
            alert('初始化失败：' + JSON.stringify(error.errorMessage));
        }
    }
});
```

方法名：FSOpen.init  
JS版本：2.0.0  
客户端支持版本：5.4.0及以上  

**参数说明**

| 参数名        | 类型          | 必填       | 参数描述  |
| ------------- |:-------------:|:-------------:|:-----|
| appId         | String       | 是 |应用ID，在纷享Web端开启应用开发者模式后可查看到appID。   |
| timestamp     | Long         | 是 |生成签名的时间戳     |
| nonceStr      | String       | 是 |生成签名的随机字符串 |
| signature     | String       | 是 |签名。值为十六进制的字符表示形式，如`784E655546BCEC2D5B8392B5998FE1403B6DD0DA`，大小写敏感，需要全部转为大写。 |

> **signature签名算法**  
> ##### jsapi_ticket  
> jsapi_ticket是纷享应用调用纷享JS API的临时票据，在签名计算中使用。正常情况下，jsapi_ticket的有效期为7200秒，通过corpAccessToken来获取。详情参考[获取jsapi_ticket][nav.open.ticket]。  
> *为提高性能，且避免频繁刷新jsapi_ticket导致api调用受限，影响自身业务，开发者必须在自己的服务全局缓存jsapi_ticket 。*    

> ##### 签名算法  
> 签名生成规则如下：参与签名的字段包括noncestr（随机字符串）, 有效的jsapi_ticket, timestamp（时间戳）, url（当前网页的URL，不包含#及其后面部分） 。对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序）后，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成字符串src_string，参数名均为小写字符，字段名和字段值都采用原始值，不进行URL转义。最后对src_string作sha1加密，即signature=sha1(src_string)。  

> 示例：
>
>
> ```
> // 各计算因子声明
> jsapi_ticket=abc123456789
> noncestr=randxxxx
> timestamp=123456
> url=https://www.fxiaoke.com/s?ie=utf8&wd=broker&tn=87048150_dg
> 
> // 拼接出src_string。对所有待签名参数按照字段名的ASCII 码从小到大排序（字典序），顺序依次为 jsapi_ticket，noncestr，timestamp，url，用&拼接其键值对。
> jsapi_ticket=abc123456789&noncestr=randxxxx&timestamp=123456&url=https://www.fxiaoke.com/s?ie=utf8&wd=broker&tn=87048150_dg
> 
> // 对src_string进行sha1签名，得到signature（大小写敏感，需要全部转为大写）
> 784E655546BCEC2D5B8392B5998FE1403B6DD0DA
> ```
> 
> 注意事项
> 1. 签名用的noncestr和timestamp必须与`FSOpen.init`中的nonceStr和timestamp相同。
> 2. 签名用的url必须是调用JS接口页面的完整URL，但不包含#及其后面部分。
> 3. 出于安全考虑，请务必在服务器端实现签名的逻辑。
> 4. 上述案例中的各参数取值以及计算结果均为标准算法实际运行结果，开发者可用其检验自己的算法实现。  

<FOR_FS_INTERNAL_ONLY>
原`FSOpen.config`, `FSOpen.ready`, `FSOpen.error`在js 2.0.0仍然可用，但更推荐使用`FSOpen.init`实现整个初始化过程。另外，对于纷享内部开发，可在特定情况下使用简式的`FSOpen.init`调用，仅传入appId，无需与开平服务器通讯获取jsapi_ticket、计算签名、兑换open-userId，代码样例：

```javascript
FSOpen.init({
  appId: 'FSAID_1313de7', // 该appId必须是被深研开平加入了白名单，且绑定了域名
  internalId: true,  // 如果internalId设定为true，则所有接口中涉及到userId的传入、传出，都将使用普通userId赋值，而不是open-userId
  onSuccess: function(resp) {
    // 初始化成功回调. 所有JS API执行必须在初始化成功后，否则会出错。
  },
  onFail: function(error) {
    // 失败回调处理
    if (error.errorCode === 30000) {
      alert('请更新纷享客户端到最新版本。');
    } else {
      alert('初始化失败：' + JSON.stringify(error.errorMessage));
    }
  }
});
```
</FOR_FS_INTERNAL_ONLY>

### 返回说明 
**成功回调返回参数**   
调用成功会回调`onSuccess`方法，可通过类似`resp.parm1``resp.parm2`获取相关返回参数，本文提到的“成功回调返回参数”即指这些提取出来的参数，如`parm1``param2`，后文不再赘述。
`FSOpen.init`执行成功即代表API运行环境初始化成功，无需处理返回结果，当然开发者还是可以在`onSuccess`回调中添加自己的代码，如数据统计、表单提交等。   

*注：后文中如接口执行成功无需处理返回结果，将省略成功返回说明。*  

**失败回调返回参数**  
调用失败会回调`onFail`方法，可通过`error.errorCode`, `error.errorMessage`获取错误描述，后文不再赘述。   
`FSOpen.init`可能返回的错误：  

| 错误码    | 错误描述                |
| ----------| -----------------------------------------------|
| 30000     | 当前客户端版本不支持JavaScript API。通常是因为客户端版本小于5.4.0，需要更新到最新版本。 |
| 30001     | API运行环境未初始化     |
| 30002     | API运行环境初始化失败   |
| 40000     | 接口调用参数错误        |
| 40001     | 初始化签名校验失败。检查signature签名生成算法是否正确，或是js ticket已过期，可尝试更新js ticket。 |

*注：后文中接口调用失败如无特殊情况，将省略失败返回说明，开发者可自行查阅[错误码汇总表][nav.appendix.error]。*      

### 样例

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="utf-8" />
    <title>纷享问问</title>
    <meta name="description" content="纷享问答社区，鼓励原创与分享" />
    <!-- FS UI Kit -->
    <link rel="stylesheet" href="assets/css/uikit.css" />
    <script src="assets/js/uikit.js"></script>

    <!-- FS core JavaScript. Better to include it at the end so the pages loaded faster -->
    <script src="http://www.fxiaoke.com/open/jsapi/2.1.3/fsapi.min.js"></script>
</head>

<body>
    <nav class="navbar navbar-default">
        <ul>
            <li><a href="#">技术专区</a></li>
            <li><a href="#">销售专区</a></li>
            <li><a href="#">公告区</a></li>
        </ul>
    </nav>
    <div class="searchbar">
        <input type="text" class="form-control" placeholder="请输入问题关键字" />
    </div>
    <!-- 300 lines of code is omitted  -->
    
    <script>
	  FSOpen.init({
		    appId : 'FSAID_1313de7',
		    timestamp : 1463564391382,
		    nonceStr : 'HUsfdsOIIejfoweu2u283',
		    signature : '1520C2C8223BC8D46525639743E53359CDD7B163',

		    onSuccess : function (resp) {
			      // 初始化成功回调. 所有JS API执行必须在初始化成功后，否则会出错。
		    },

		    onFail : function (error) {
			      // 失败回调处理
			      if (error.errorCode === 30000) {
				        alert('请更新纷享客户端到最新版本。');
			      } else {
				        alert('初始化失败：' + JSON.stringify(error.errorMessage));
			      }
		    }
	  });
    </script>
</body>
</html>

```

[nav.appendix.error]:  http://open.fxiaoke.com/wiki.html#artiId=117
[nav.open.ticket]:   http://open.fxiaoke.com/wiki.html#artiId=17
[uikitdownload]: http://open.fxiaoke.com/open/uikit/uikit.zip