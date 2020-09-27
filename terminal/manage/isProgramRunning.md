## 查看当前是否在播放节目

### 命令
isProgramRunning

### needNotify
true

### 出参

|名称|编码|类型|必须|默认值|说明|
|--|--|--|--|--|--|
|返回码|code|init|Y|无|0代表请求成功|
|数据|data|object|N|false|有返回数据时有该字段|

#### 数据

|名称|编码|类型|必须|默认值|说明|
|--|--|--|--|--|--|
|是否在播放|running|boolean|Y|无|在播放：true 不在播放：false|

### 使用示例

```
// jquery的ajax使用示例：
$.ajax({
	type: 'POST',
	url: (终端的IP地址+ 8080 + '/command/execute'), // 示例： http://127.0.0.1:8080/command/execute   实际使用中 127.0.0.1要替换成真实IP
	dataType: 'json',
	contentType: 'application/json',
	data:JSON.stringify({
		method:'cmd', 		// 必须有，且值固定为cmd
		needNotify:true,   
		data:{              // 必须有，且内部cmd值不能为空
			cmd:'isProgramRunning' 		// 必须有，命令API对应的具体命令
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