### 由于作者久未更新, 本插件修正jsfun-record无法使用等问题, 并优化了部分逻辑
### [原插件地址](https://ext.dcloud.net.cn/plugin?id=436#)

### 使用方式：

```html
<jcfun-record
  voicePath=""
  :maxTime="15"
  :minTime="5"
  @okClick="saveRecord"
	@show="show"
	@close="close"
>
	<view class="centerwz">录制语音</view>
</jcfun-record>	
```

```javascript
methods:{
	saveRecord(recordPath) {		
		console.log("===音频文件地址："+recordPath+"===")
		//do... 可以使用 uni.uploadFile 接口上传到服务器
	},
	show(){
		console.log('show')
	},
	close(){
		console.log('close')
	}
}
```

在 ``template`` 中使用组件



### 属性说明：

|属性名		|类型	|默认值					|说明				|
|---		|----	|---					|---																											|
|voicePath		|String	|""						|默认文件地址，可以预览																									|
|maxTime		|Number	|15|最大时间：单位秒				|
|defaultArr		|String	|5|最小时间：单位秒|


### 事件说明：

|事件称名|说明|
|---|----|---|
|okClick|点击确认后可以获得音频的地址	|
|show|显示事件 |
|close|关闭事件 |



