## 使用
#### 直接引用js文件

```
// 访问https://github.com/3ccc/superlongjing
// 使用dist文件夹下的jlongjing.min.js
<script src="./dist/jlongjing.min.js"></script>
```

## 规范
|名称|字段|说明|
|--|--|--|
|返回码|code|值为0时，正常|
|数据|data|出参|
|异常信息|msg|code!=0时，返回该字段|

### 入参格式及类型

```
{
	keyName1:'字符串', // 字符串
	keyName2:['数组'], // 数组
	keyName3:{key:''}, // 对象
	keyName2:true 	   // boolean
}
```

### 出参格式及类型

```
//成功
{
	"code":0,
	"data":{
		"key1":value1，
		"key2":value2
	}
}
//失败
{
	"code":-1,
	"msg":"错误信息"
}
```

### js调用Native(应用调用app)

* 使用格式

```
// fnName = 'reboot'  方法名称   string
// data = {}  入参   jsonObject
// callback  = function(){} 回调函数  function
longjing.call(fnName,data,callback);
```

* 示例一:

```
// 主动调用方法，无返回值
longjing.call('loadSuccess');  // 告知app，页面加载完成
```

* 示例二:

```
// 主动调用方法，有返回值
// app返回的是{code:0,data:{ip:'127.0.0.1'}};
// longjing过滤出返回值中的data
var appId = longjing.call('getAppId').id;
```

* 示例三:

```
// 主动调用方法，回调中返回
// 回调返回的是{code:0,data:{url:'...'}};
// longjing过滤出回调返回值中的data
longjing.call('tts.speak',{text:'...'},function(res){
   // 语音播放完毕后的回调处理
   // todo ...
})
```

### Native调用JS(app通知到应用)

* 使用格式

```
// fnName = 'reboot'  方法名称   string
// data = {}  入参   jsonObject
// callback  = function(){} 回调函数  function
longjing.register(fnName,data,callback);
```

示例一：

```
// 注册一个软键盘状态改变时通知
longjing.register('SOFT_INPUT_CHANGED ',function(){});
```

## 介绍
### 概要
1. 小应用调用
1. 包括OS和外置设备(远期支持)，如读卡器

## 方法名称
### 获取app号(getAppId)

* 输入:无
* 输出：

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|app的id|id|string|N|说明|

* 使用示例：
```
let appid = longjing.call('getAppId').id;
```

### 读取配置(readConfig)
* 输入:无
* 输出：

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|内容|content|JSON Object|N|说明|

* 使用示例：

```
//方式1：
let config = longjing.call('readConfig').content; // 获取全部配置
//方式2：
let config = longjing.config(); // 获取全部配置
//方式3：
let name = longjing.config('name'); // 获取指定配置的值
```

### 写入配置(writeConfig)

* 输入:

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|内容|content|JSON Object|N|说明|

* 输出：无

* 使用示例：

```
//方式1:
longjing.call('writeConfig',{content:'配置内容'}); // 此方式会覆盖所有配置内容
//方式2：
longjing.config('name','张三');  // 此方式增加一项配置，不会删除已有配置内容
```

### 加载成功(loadSuccess)

* 输入:无
* 输出：无

* 使用示例：

```
longjing.call('loadSuccess');
```

### 语音(tts)

#### 合成播放(speak)

* 输入:

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|文本|text|string|N|需要语音叫号的内容|

* 输出：异步输出

* 使用示例：

```
longjing.call('tts.speak',{text:'需要语音叫号的内容'},function(){
	// 语音叫号完毕后的处理
});
```

### 设置Led颜色(setLedColor)

* 说明：该方法只能部分设备可以使用

 * 输入:

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|类型|type|init|N|DEVICE_DR = 1;//德睿  DEVICE_QS = 2;//仟视  DEVICE_SKW = 3;//盛科维|
|颜色|color|string|N|if( type == DEVICE_SKW  ){16进制指令，例如: ff0000 红色}else {LED_OFF = 0; // 关灯LED_COLOR_RED = 1; // 红灯LED_COLOR_GREEN = 2; // 绿灯LED_COLOR_YELLOW = 3; // 黄灯}|

* 输出：无

* 使用示例：

```
longjing.call('setLedColor',{type:1,color:'ff0000'});
longjing.setLedColor({type:1,color:'ff0000'});
```

### 只读配置(config)

* 只读配置项

|名称|	字段|	类型|	说明|
|--|--|--|--|
|设备号|	deviceNumber|	string|同时是：终端号、屏幕号|
|商户号|	sellerId|	string|	|
|网点号|	pointNo|	string|	|
|小应用号|	appId	|string|	|

* 使用示例：

```
let deviceNumber = longjing.config('deviceNumber');
```

### 驱动(driver)

#### 开启RFID(startRFID)

* 说明：该方法只能部分设备可以使用

* 输入: 无

* 输出：异步输出

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|数据|data|JSON String|N||

* 使用示例：

```
longjing.call('driver.startRFID',function(res){
	// 异步输出数据 res.data.data
	// res数据结构如下：
	// {
	// 	"code":0,
	// 	"data":{
	// 		"data":数据data
	// 	}
	// }
});
```


#### 关闭RFID(stopRFID)

* 说明：该方法只能部分设备可以使用

* 输入: 无

* 输出：无

* 使用示例：

```
longjing.call('driver.stopRFID');
```


### 开启红外感应(startInfrared)

* 方法： startInfrared

* 说明：该方法只能部分设备可以使用

* 输入: 无

* 输出：异步输出

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|数据|data|JSON String|N||

* 使用示例：

```
longjing.call('driver.startInfrared',function(res){
	// 异步输出数据 res.data.data
	// res数据结构如下：
	// {
	// 	"code":0,
	// 	"data":{
	// 		"data":数据data
	// 	}
	// }
});
```


### 关闭红外感应(stopInfrared)

* 方法： stopInfrared

* 说明：该方法只能部分设备可以使用

* 输入: 无

* 输出：无

* 使用示例：

```
longjing.call('driver.stopInfrared');
```

## 通知
### 软键盘状态改变(SOFT_INPUT_CHANGED )

* 输入:

|名称|字段|类型|可选|说明|
|--|--|--|--|--|
|显示|visible|boolean|N|true 显示 ，false 隐藏|

* 输出：无
* 使用示例：

```
longjing.register('SOFT_INPUT_CHANGED',function(){
	// 软键盘状态改变的处理
});
```

