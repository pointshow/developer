## 命令API

### 命令清单

* [查看APP在前台](terminal/manage/isAppForeground)
* [查看节目播放](terminal/manage/isProgramRunning)
* [获取日志清单](terminal/manage/getDevLog)
* [浏览文件目录](terminal/manage/readFile)
* [获取数据库](terminal/manage/getDB)
* [退出播放](terminal/manage/quitPlay)
* [重启APP](terminal/manage/relaunchApp)
* [退出APP](terminal/manage/quitApp)
* [截屏](terminal/manage/screenshot)


### 命令API说明

以上命令请求格式统一。

请求地址 = 终端的IP地址 +端口号8080+ API名称

|API名称|请求类型|支持跨域|dataType|contentType|支持框架|
|--|--|--|--|--|--|
|command/execute|POST|Y|json|application/json|1.axios 2.jquery的ajax|


### 入参

|名称|编码|类型|必须|默认值|说明|
|--|--|--|--|--|--|
|命令|method|string|Y|cmd||
|是否需要通知|needNotify|boolean|N|false|需要返回数据时，必须填写true。默认为false|
|数据|data|object|Y|无||

### 出参

|名称|编码|类型|必须|默认值|说明|
|--|--|--|--|--|--|
|返回码|code|init|Y|无|0代表请求成功|
|数据|data|object|N|false|有返回数据时有该字段|

### 命令API使用示例

```
// jquery的ajax使用示例：
$.ajax({
	type: 'POST',
	url: (终端的IP地址+ 8080 + '/command/execute'), // 示例： http://127.0.0.1:8080/command/execute   实际使用中 127.0.0.1要替换成真实IP
	dataType: 'json',
	contentType: 'application/json',
	data:JSON.stringify({
		method:'cmd', 		// 必须有，且值固定为cmd
		needNotify:false,   // 可以不传该参数，该值默认为false,如果需要返回值，则必传且为true，如：查看APP在前台。 
		data:{              // 必须有，且内部cmd值不能为空
			cmd:'', 		// 必须有，命令API对应的具体命令
			data:{}			// 可以不传该参数，部分接口需要，如：浏览文件目录
		} 			
	}),
	success:function(res){
		if(res.code ==0){
			// todo 处理业务
		}
	},
	error:function(){
	}
})
```

## 普通API

* [升级APP](terminal/manage/upgradeApp)
* [升级HTML](terminal/manage/upgradeHtml)


