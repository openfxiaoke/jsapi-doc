## 媒体

### 文件 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
            <td>接口说明</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>media.file.preview</td>
            <td>文件预览</td>
            <td></td>
        </tr>
        <tr>
            <td>media.file.upload</td>
            <td>文件上传</td>
            <td>只支持安卓系统</td>
        </tr>
        <tr>
            <td>media.file.download</td>
            <td>文件下载</td>
            <td>只支持安卓系统</td>
        </tr>
        <tr>
            <td>media.file.reupload</td>
            <td>文件重传</td>
            <td>只支持安卓系统</td>
        </tr>
    </tbody>
</table>

#### 文件预览
此接口只支持预览存储在纷享文件系统里的`N-Path`路径地址。  

代码样例
```javascript
FSOpen.media.file.preview({
    fileSize: 1024*1024,
    fileName: '文件名字',
    fileNPath: 'N_201606_29_f13bbed15ba14413bc0aef29be255817.docx',
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.media.file.preview     
JS版本：2.1.0  
客户端支持版本：5.4.2及以上  

调用参数说明：    

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| fileSize  | Number    | 否   | 预览文档的文件大小，单位byte  |
| fileName  | String    | 否   | 预览文档名字  |
| fileNPath | String    | 是   | 文档所对应的N-Path地址，资源需是存储在纷享平台上，采用N-Path地址引用，如`N_201512_08_101239c8308f4ea7325f69df4fba386f1.pptx`。目前支持的文件后缀有`doc``docx``pdf``ppt``pptx`等通用文档格式。 |
| isAliCloud  | Boolean    | 否   | 文档url是否是阿里云服务器url  |
| failureTime  | Long    | 否   | 阿里云url的过期时间，如果是阿里云服务器url,刚需要填写failureTime字段  |

#### 文件上传
文件上传会有两个阶段的回调：第一阶段在调用接口，并选择完毕文件后，即会通过`onSuccess`回调给业务方，业务方可在这个阶段展示文件的信息；第二阶段在文件上传完毕后，通过`onUpload`将上传结果回调给业务方   

**该接口默认上传的文件会永久存储在文件系统**

代码样例
```javascript
FSOpen.media.file.upload({
    onUpload: 'return function(r){alert(JSON.stringify(r))}'
});
``` 

方法名：FSOpen.media.file.upload     
JS版本：2.1.0   
客户端支持版本：5.4.2及以上   

调用参数说明：     

| 参数      | 类型           | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| maxSize   | Number        | 否   | 文件最大尺寸限制，单位byte |
| maxFileCount | Number     | 否   | 最大可支持上传文件个数，默认1 |
| onUpload  | Function      | 否   | 上传成功的回调接口，每上传一张图片完毕（无论成功与否）就会回调一次 |

`onUpload`回调参数说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| file      | Object    | 本地回调的上传图片结果 |

`file`的回调信息说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| result      | Boolean   | 上传结果 |
| id          | String    | 本次上传的文件ID（唯一） |
| fileName    | String    | 文件名称 |
| fileSize    | Number    | 文件大小 |
| fileNPath   | String    | 本地上传的文件`N-Path`信息 |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| selectedFile   | Object | 选择的文件信息 |
| excludedFile   | Object | 被过滤（不满足`maxSize`）的文件信息 |

`file`的回调信息说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| id          | String    | 本次上传的文件ID（唯一） |
| fileName    | String    | 文件文件名称 |
| fileSize    | Number    | 文件大小 |
| fileLocalPath | String    | 要上传的文件的本地路径访问地址 |

#### 文件下载
此接口仅适用于Android系统。

```javascript
FSOpen.media.file.download({
    fileUrl: 'N_201606_29_f13bbed15ba14413bc0aef29be255817.docx',
    fileName: '纷享JSAPI开发文档.docx',
    onProgress: function(resp) {
        console.log(resp.loaded, resp.total);
    },
    onSuccess: function(resp) {
        console.log(resp.fileLocalPath);
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.media.file.download   
JS版本：2.1.0  
客户端支持版本：5.4.2及以上  

调用参数说明：    

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| fileId    | String    | 是   | 文件ID |
| fileToken | String    | 是   | 文件Token |
| fileId    | String    | 是   | 文件ID |
| fileUrl   | String    | 是   | 要下载的文件地址，支持`N-Path`地址和标准的HTTP链接地址 |
| fileName  | String    | 是   | 要下载的文件存储名 |
| onProgress| Function  | 否   | 下载进度回调 |

> 该接口支持`APath`，`NPath`和`Token`的三种下载方式；
> 当下载的是`APath`和`NPath`文件时，`fileUrl`必填；
> 当下载的是`Token`文件时，`fileId`，`fileToken`，`fileName`三个参数都必须指定

`onProgress`回调参数说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| loaded      | Number    | 已下载文件大小，以byte为单位 |
| total       | Number    | 总下载文件大小，以byte为单位 |

成功回调返回参数：  

| 参数          | 类型      | 说明     |
| --------------| ----------| ---------|
| fileLocalPath | String    | 下载文件的本地路径 |


#### 文件重传

代码样例
```javascript
FSOpen.media.file.reupload({
    fileLocalPath: 'http://www.fxiaoke.com/jsApi/uploadImageThumb?path=/storage/sdcard1/DCIM/Camera/IMG_20170104_102105.jpg'
});
``` 

方法名：FSOpen.media.file.reupload       
JS版本：2.1.3   
客户端支持版本：5.4.4及以上   

调用参数说明：     

| 参数      | 类型           | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| fileLocalPath  | String        | 是   | 原在文件选择的时候（调用`file.upload`）返回的本地文件地址 |

成功回调返回参数：  

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| result      | Boolean    | 上传结果 |
| id          | String    | 本次上传的文件ID（唯一） |
| fileName   | String    | 文件名称 |
| fileSize   | String    | 文件大小，单位byte |
| fileNPath  | String    | 本地上传的文件`N-Path`信息 |

### 图片 

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>media.image.preview</td>
            <td>图片预览</td>
        </tr>
        <tr>
            <td>media.image.upload</td>
            <td>图片上传</td>
        </tr>
        <tr>
            <td>media.image.submit</td>
            <td>图片转存为永久，此接口用于将`N-Path`上传的临时上传图片转存为永久</td>
        </td>
        <tr>
            <td>media.image.save</td>
            <td>图片保存到本地</td>
        </td>
        <tr>
            <td>media.image.reupload</td>
            <td>图片重传</td>
        </tr>
        <tr>
            <td>media.image.shareToWX</td>
            <td>图片重传</td>
        </tr>
    </tbody>
</table>

#### 图片预览

代码样例
```javascript
FSOpen.media.image.preview({
    index: 0,
    imgUrls: [
        'https://www.fxiaoke.com/static/img/index/icon-wx-small.png?v=5.1.5',
        'https://www.fxiaoke.com/static/img/index/icon-kh-small.jpg?v=5.1.5'
    ],
    deletable: true,
    onSuccess: function(imgUrls) {
        console.log(imgUrls)
    }
});
``` 

方法名：FSOpen.media.image.preview     
JS版本：2.0.0  
客户端支持版本：5.4.0及以上   

调用参数说明：     

| 参数      | 类型          | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| index     | Number        | 否   | 从第几张图片开始预览，索引从0开始计算。默认为0 |
| imgUrls   | Array[String] | 是   | 图片地址列表，默认为空 |
| deletable | Boolean       | 否   | 是否可在预览时删除 |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| imgUrls        | Array[String] | 最终预览的图片url列表 |


#### 图片上传
图片上传会有两个阶段的回调：第一阶段在调用接口，并选择完毕文件后，即会通过`onSuccess`回调给业务方，业务方可在这个阶段展示文件的信息；第二阶段在图片上传完毕后，通过`onUpload`将上传结果回调给业务方，此回调将会触发多次，即每上次完毕（无论成功与否）一次图片，就会调用一次回调    

**该接口默认上传的图片会永久存储在文件系统，如需要存储为临时地址，请通过设置`delaySubmit`参数来存储为临时路径，并通过`media.image.submit`接口来转存为永久路径**

代码样例
```javascript
FSOpen.media.image.upload({
    source: ['album', 'camera'], 
    delaySubmit: true, 
    onUpload: 'return function(r){alert(JSON.stringify(r))}'
});
``` 

方法名：FSOpen.media.image.upload     
JS版本：2.1.0   
客户端支持版本：5.4.2及以上   

调用参数说明：     

| 参数      | 类型          | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| source    | Array[String] | 否   | 图片上传源，目前只有两个：`album`表示相册，`camera`表示拍照，默认仅从相册上传 |
| max       | Number        | 否   | 最大上传张数，默认不限制 |
| delaySubmit | Boolean     | 否   | 如果为`true`则延迟提交，即上传到文件系统的临时路径（将在7天后删除），需要业务方调用`FSOpen.media.image.submit`进行转存为永久路径。默认为false，即上传为永久路径 |
| originImg   | Boolean     | 否   | 是否原图上传，如果非原图上传，app端将会先做一次图片压缩。默认为`false`，即非原图上传 |
| maxSize   | Number        | 否   | 图片最大尺寸限制，单位byte。 如果originImg=true，则maxSize用来限制原图，如果originImg=false，则maxSize用来限制压缩后大小 |
| watermark | Boolean       | 否   | 是否打水印 |
| onUpload  | Function      | 否   | 上传成功的回调接口，每上传一张图片完毕（无论成功与否）就会回调一次 |

`onUpload`回调参数说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| count      | Number    | 总共上传多少张图片 |
| done       | Number    | 已上传完多少张图片 |
| image      | Number    | 本地回调的上传图片结果 |

`image`的回调信息说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| result      | Boolean    | 上传结果 |
| id          | String    | 本次上传的图片ID（唯一） |
| imageName   | String    | 图片文件名称 |
| imageSize   | String    | 图片大小，单位byte |
| imageNPath  | String    | 本地上传的图片`N-Path`信息（无论是否延迟上传） |
| imageNPathUrl | String  | 本地上传的图片`N-Path`访问路径（无论是否延迟上传） |

成功回调返回参数：  

| 参数           | 类型      | 说明     |
| ---------------| ----------| ---------|
| selectedImages | Array[Object] | 选择的文件信息列表，单项数据`image`说明如下 |
| excludedImages | Array[Object] | 被过滤（不满足`maxSize`）的图片信息列表，单项数据`image`说明如下 |

`image`的回调信息说明：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| id          | String    | 本次上传的图片ID（唯一） |
| imageName   | String    | 图片文件名称 |
| imageSize   | String    | 图片大小，单位byte |
| imageLocalPath | String    | 要上传的图片的本地路径访问地址 |
| imageLocalData | String    | 要上传的图片的等比压缩后的base64编码值，可直接展示，目前仅ios端实现该功能，但安卓也会返回该字段。老版本需要做兼容，如：<code>image.src = resp.imageLocalData &#124;&#124; imageLocalPath</code> |

#### 图片转正

代码样例
```javascript
FSOpen.media.image.submit({
    imageNPath: 'TN_51d6f18211cb4a849613d38b058b1078',
    imageName: 'filename.pptx'
});
``` 

方法名：FSOpen.media.image.submit     
JS版本：2.1.2   
客户端支持版本：5.4.3及以上   

调用参数说明：     

| 参数       | 类型          | 必须 | 说明         |
| -----------| --------------| -----| -------------|
| imageName  | String        | 否   | 图片文件名称 |
| imageNPath | String        | 否   | 图片在文件系统的`N-Path` |

成功回调返回参数：  

| 参数          | 类型      | 说明     |
| --------------| ----------| ---------|
| result        | String    | 转正后的`N-Path`信息 |

#### 图片另存为本地文件

代码样例
```javascript
FSOpen.media.image.save({
    url: 'http://www.fxiaoke.com/open/jsapi/res/logo.jpg'
});
``` 

方法名：FSOpen.media.image.save     
JS版本：2.1.2   
客户端支持版本：5.4.3及以上   

调用参数说明：     

| 参数      | 类型           | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| imageUrl  | String        | 是   | 需要保存的图片url地址，支持`N-Path`，6.2版本开始支持支持保存base64编码 |

成功回调返回参数：  

| 参数          | 类型      | 说明     |
| --------------| ----------| ---------|
| result        | String    | 保存的本地绝对路径地址 |

#### 图片重传

代码样例
```javascript
FSOpen.media.image.reupload({
    imageLocalPath: 'http://www.fxiaoke.com/jsApi/uploadImageThumb?path=/storage/sdcard1/DCIM/Camera/IMG_20170104_102105.jpg'
});
``` 

方法名：FSOpen.media.image.reupload       
JS版本：2.1.3   
客户端支持版本：5.4.4及以上   

调用参数说明：     

| 参数      | 类型           | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| imageLocalPath  | String        | 是   | 原在图片选择的时候（调用`image.upload`）返回的本地图片地址 |

成功回调返回参数：  

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| result      | Boolean    | 上传结果 |
| id          | String    | 本次上传的图片ID（唯一） |
| imageName   | String    | 图片文件名称 |
| imageSize   | String    | 图片大小，单位byte |
| imageNPath  | String    | 本地上传的图片`N-Path`信息 |
| imageNPathUrl | String  | 本地上传的图片`N-Path`访问路径 |

#### 图片分享到微信

代码样例
```javascript
FSOpen.media.image.shareToWX({
    imageUrl: ''
});
``` 

方法名：FSOpen.media.image.shareToWX       
JS版本：2.1.5   
客户端支持版本：5.6.1及以上   

调用参数说明：     

| 参数      | 类型           | 必须 | 说明         |
| ----------| --------------| -----| -------------|
| imageUrl  | String        | 是   | 要分享的图片地址 |

成功回调返回参数：  

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| result      | Boolean    | 上传结果 |




