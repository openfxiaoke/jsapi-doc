## 弹层

<table class="api-list">
    <thead>
        <tr>
            <td>接口名</td>
            <td>接口描述</td>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>widget.showActionSheet</td>
            <td>显示弹出菜单</td>
        </tr>
        <tr>
            <td>widget.showAlert</td>
            <td>显示警告窗口</td>
        </tr>
        <tr>
            <td>widget.showConfirm</td>
            <td>显示确认窗口</td>
        </tr>
        <tr>
            <td>widget.showPreloader</td>
            <td>显示加载提示</td>
        </tr>
        <tr>
            <td>widget.hidePreloader</td>
            <td>隐藏加载提示</td>
        </tr>
        <tr>
            <td>widget.showModal</td>
            <td>显示模态窗口</td>
        </tr>
        <tr>
            <td>widget.showPrompt</td>
            <td>显示带输入框的窗口</td>
        </tr>
        <tr>
            <td>widget.showToast</td>
            <td>显示Toast</td>
        </tr>
        <tr>
            <td>widget.showDateTimePicker</td>
            <td>显示日期选择控件</td>
        </tr>
        <tr>
            <td>widget.showEditor</td>
            <td>显示文本编辑器</td>
        </tr>
        <tr>
            <td>showTextViewer</td>
            <td>显示大文本阅读器</td>
        </tr>
    </tbody>
</table>

#### 显示单选菜单
![image10][image-actioinsheet]

代码样例
```javascript
FSOpen.widget.showActionSheet({
    title: '标题',
    cancelBtnLabel: '取消',
    actionBtnLabels: ['湖人', '马刺', '火箭'],
    onSuccess: function(resp) {
        if (resp.actionIndex == 0) {
            alert('选择了湖人');
        } else if (resp.actionIndex == 1) {
            alert('选择了马刺');
        } else {
            alert('选择了火箭');
        }
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showActionSheet   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数            | 类型          | 必须 | 说明         |
| ----------------| --------------| -----| -------------|
| title           | String        | 否   | 控件标题。如果为空，Android系统默认显示“选项”，ios系统不显示。 |
| cancelBtnLabel  | String        | 否   | `取消`选项文本，默认为“取消”，点击后收起单选列表。 |
| actionBtnLabels | Array[String] | 是   | 选项文本列表 |

成功回调返回参数：

| 参数        | 类型      | 说明     |
| ------------| ----------| ---------|
| actionIndex | Number    | 选择的索引号，从0开始，从上到下依次递增+1。 |

#### 显示警告窗口     
![image11][image-alert]

代码样例
```javascript
FSOpen.widget.showAlert({
    title: '标题',
    content: '消息内容',
    btnLabel: '我知道了',
    onSuccess: function(resp) {
        // return nothing
    },
    onFail: function(error) {
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showAlert   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数     | 类型      | 必须 | 说明         |
| ---------| ----------| -----| -------------|
| title    | String    | 否   | 弹窗标题 |
| content  | String    | 否   | 弹窗消息内容 |
| btnLabel | String    | 否   | 弹窗按钮文本，默认“OK”。 |

#### 显示确认窗口     
![image12][image-confirm]

代码样例
```javascript
FSOpen.widget.showConfirm({
    title: '标题',
    content: '消息内容',
    btnLabels: ['取消','确定'],
    onSuccess: function(resp) {
        if (resp.btnIndex == 0) {
            alert('取消');
        } else {
            alert('确定');
        }
    },
    onFail: function(error) {
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showConfirm   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| title     | String    | 否   | 弹窗标题 |
| content   | String    | 否   | 弹窗消息内容 |
| btnLabels | String    | 是   | 弹窗左右按钮的文本，最多两个。 |

成功回调返回参数：    

| 参数     | 类型     | 说明     |
| ---------| ---------| ---------|
| btnIndex | Number   | 点击的按钮索引，从0开始，从左到右分别为0、1 |

#### 显示加载提示     
![image13][image-preloader]

代码样例
```javascript
FSOpen.widget.showPreloader({
    text: '正在加载中',
    icon: true,
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        // do sth
    }
});
``` 

方法名：FSOpen.widget.showPreloader   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| text      | String    | 否   | loading显示的文本，空表示不显示文字 |
| icon      | Boolean   | 否   | 是否显示图标，默认为true；若text为空，则强制为true。 |

#### 隐藏加载提示     


代码样例
```javascript
FSOpen.widget.hidePreloader();
``` 

方法名：FSOpen.widget.hidePreloader   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     


#### 显示模态提示     
![image14][image-modal]

代码样例
```javascript
FSOpen.widget.showModal({
    title: '升级提示',
    imgUrl: 'https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/04/rBEiBVfZFl6AOJ6eAABPHGYzNOo452.png',
    content: '有很多新功能哦~',
    btnLabels: ['我知道了','升级'],
    onSuccess: function(resp) {
        if (resp.btnIndex == 0) {
            alert('取消');
        } else {
            alert('确定');
        }
    },
    onFail: function(error) {
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showModal   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| title     | String    | 否   | 弹窗标题 |
| imgUrl    | String    | 否   | 弹窗图片内容，默认为空不显示 |
| content   | String    | 否   | 弹窗文本内容，默认为空 |
| btnLabels | String    | 是   | 弹窗左右按钮的文本，最多两个。 |

成功回调返回参数：   

| 参数     | 类型     | 说明     |
| ---------| ---------| ---------|
| btnIndex | Number   | 点击的按钮索引，从0开始，从左到右分别是0、1 |

#### 显示带输入框的窗口     
![image15][image-prompt]

代码样例
```javascript
FSOpen.widget.showPrompt({
    title: '标题',
    content: '消息内容',
    btnLabels: ['取消','确定'],
    onSuccess: function(resp) {
        if (resp.btnIndex == 0) {
            alert('取消');
        } else {
            alert('确定');
        }
    },
    onFail: function(error) {
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showPrompt   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| title     | String    | 否   | 弹窗标题说明 |
| content   | String    | 否   | 弹窗消息内容 |
| btnLabels | String    | 是   | 弹窗左右按钮的文本，最多两个。 |

成功回调返回参数：  

| 参数     | 类型     | 说明     |
| ---------| ---------| ---------|
| btnIndex | Number   | 点击的按钮索引，从0开始，从左到右分别是0、1 |
| value    | String   | 弹窗输入框的值 |

#### 显示Toast     

代码样例
```javascript
FSOpen.widget.showToast({
    icon: 'success',
    text: '提示信息',
    duration: 3000,
    delay: 1000,
    onSuccess: function(resp) {
        // do sth
    },
    onFail: function(error) {
        // do sth
    }
});
``` 

方法名：FSOpen.widget.showToast   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数      | 类型      | 必须 | 说明         |
| ----------| ----------| -----| -------------|
| text      | String    | 否   | 要显示的提示信息，默认为空 |
| icon      | String    | 否   | 要显示的图标样式，有`success`和`error`。默认为`success` |
| duration  | Number    | 否   | 显示持续时间，单位毫秒，默认按系统规范 |
| delay     | Number    | 否   | 延迟显示时间，单位毫秒，默认0 |


#### 显示日期选择控件     
![image17][image-picker]

```javascript
FSOpen.widget.showDateTimePicker({
    dateType: 'month',
    defaultValue: '2015-03-24',
    onSuccess: function(resp) {
        alert('选择的时间值：' + resp.value);
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.widget.showDateTimePicker   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数         | 类型      | 必须 | 说明         |
| -------------| ----------| -----| -------------|
| dateType     | String    | 是   | 时间选择器的选择类型。此类型将决定输入和输出的格式类型 |
| defaultValue | String    | 否   | 时间默认值，若为空将根据`dateType`获取当前时间的对应值，如`dateType`是`time`（时分）类型，则为24小时制。 |

`dateType`参数说明：

| 值       | 输入输出格式  | 说明     |
| ---------| --------------| ---------|
| month    | yyyy-MM       | 年月     |
| day      | yyyy-MM-dd    | 年月日   |
| time     | HH:mm         | 时分，24小时制 |
| week     | yyyy-MM-dd~yyyy-MM-dd | 周 |
| day&#124;time | yyyy-MM-dd HH:mm | 年月日 时分 |

成功回调返回参数：  

| 参数       | 类型      | 说明     |
| -----------| ----------| ---------|
| value      | String    | 选择的时间字符串，与`dateType`对应。如果有时分秒，则为24小时制。 |

#### 显示文本编辑器     
![image18][image-editor]

代码样例
```javascript
FSOpen.widget.showEditor({
    min: 1,
    max: 140,
    placeholder: '请输入内容',
    navbar: {
        title: '标题',
        leftLabel: '返回',
        leftArrow: true,
        rightLabel: '发送'
    },
    backFillData: {
        content: '我是输入',
    },
    components: ['emoji', 'at'],
    onSuccess: function(resp) {
        alert('你的输入是：' + resp.content);
    },
    onFail: function(error) {
        alert('操作失败，错误码：' + error.errorCode);
    }
});
``` 

方法名：FSOpen.widget.showEditor   
JS版本：2.0.0    
客户端支持版本：5.4.0及以上     

调用参数说明：     

| 参数         | 类型          | 必须 | 说明         |
| -------------| --------------| -----| -------------|
| min          | Number        | 否   | 最小输入字符限制，不区分中英文。默认不做限制（可以为空） |
| max          | Number        | 否   | 最大输入字符限制，不区分中英文。系统最多支持3000字。|
| placeholder  | String        | 否   | 占位符。默认为空 |
| navbar       | Object        | 否   | 输入框标题栏控制，具体说明见下面。|
| backFillData | Object        | 否   | 回填数据内容，仅支持文本数据回填，具体说明见下面。默认为空 |
| components   | Array[String] | 否   | 快捷输入组件。目前支持：`emoji`-表情，`at`-@功能 |

`navbar`参数字段说明：

| 参数       | 类型      | 说明     |
| -----------| ----------| ---------|
| title      | String    | 顶部中间抬头文本 |
| leftLabel  | String    | 顶部左侧按钮文本 |
| leftArrow  | Boolean   | 顶部左侧按钮是否使用箭头图标，iOS适用。 |
| rightLabel | String    | 顶部右侧按钮文本 |

`backFillData`参数字段说明：

| 参数          | 类型          | 说明     |
| --------------| --------------| ---------|
| content       | String        | 需要回填的文本内容 |

成功回调返回参数：  

| 参数          | 类型          | 说明     |
| --------------| --------------| ---------|
| content       | String        | 输入纯文本内容，如“批准[微笑]，@北京研发中心”。Emoji表情由H5端自行解析 |


#### 显示大文本阅读器     

```javascript
FSOpen.widget.showTextViewer({
    text: '我是一段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段段很长的文本',
    onSuccess: function(resp) {
        alert(JSON.stringify(resp));
    },
    onFail: function(error) {
        alert('获取失败，错误码：' + error.errorCode);
    }
});
```

方法名：FSOpen.widget.showTextViewer   
JS版本：2.1.7    
客户端支持版本：6.2.0及以上     

调用参数说明：     

| 参数         | 类型      | 必须 | 说明         |
| -------------| ----------| -----| -------------|
| text         | String    | 是   | 大文本内容 |


[image-actioinsheet]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/04/rBEiBVfZFMuACJRaAAKkouRyTpE291.png
[image-alert]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/08/rBEiBlfZFRqAf9baAAMtSsaZwiY740.png
[image-confirm]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/0D/rBEiBVfePH-AM7ULAAG3fca1nvA945.png
[image-preloader]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/01/50/rBEiBFfZFy2AOjjvAAQQ1QPdiYU545.png
[image-modal]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/04/rBEiBVfZF0mAa8DaAAPD-mQkvF0449.png
[image-prompt]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/09/rBEiBlfZGMGAHTbSAAM7wVYwZwk729.png
[image-picker]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/01/50/rBEiBFfZGNKAQFaoAAN4ohNc4Hs196.png
[image-editor]: https://open.fxiaoke.com/fscdn/img?imgId=group1/M00/02/04/rBEiBVfZGOmAQfy-AAG40v_WcD4630.png