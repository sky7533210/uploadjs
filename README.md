# 概述
前端的一个文件上传的插件
此插件在里面集成了spark-md5,不需要在引入spark-md5了，无需引入jQuery就可以使用。

此插件会把文件分割成小块文件（具体大小可以自行设置）然后计算小块文件的md5，通过get请求服务器
服务器可以根据这个小块去查询服务端是否有该小块文件，如果有响应码设为207，前端就不会上传该块的
数据了，如果响应码设置208，前端就会上传该文件块的数据，后端可根据块号与块数判断该文件是否全部块
上传完成，如果上传完成可以合并也可以不合并。

# 使用
	f=new FileUpload("/fileupload.php","#btn","#btn2",20*1024*1024);
	或
	f=new FileUpload("/fileupload.php","#btn");
	## 参数：
		"/fileupload.php"：文件上传后台服务器的地址，需要提供一个get和一个post方式的接口
			get：
				get方式用于验证该块是否再服务器上是否存在，如果存在响应码应设为207通知前
				端不需要上传该文件块如果不存在响应码设为208通知前端需要上传该文件块。设置
				其他会报服务端错误。
				get请求附带的参数：
					md5：该文件块的md5值
					filename：文件的名字
					totalSize：该文件大小
					chunkSize：块大小
					totalChunks：快数
					curChunkNumber：当前块号
					type：文件类型
			post：用于把文件块上传到服务器使用multipart/form-data的格式提交
				附带参数：
					md5：该文件块的md5值
					filename：文件的名字
					totalSize：该文件大小
					chunkSize：块大小
					totalChunks：快数
					curChunkNumber：当前块号
					type：文件类型
					curChunkSize：当前块实际大小
					chunkFile:文件快内容

		"#btn"：元素的选择器点击该元素可以弹出文件选择框
		"#btn2"：把文件拖动到该元素上并释放时会触发上传。
		如果你想点击与拖动放到同一元素上，只需要写一个元素的选择器就可以，无需要写第二个。
		20*1024*1024：文件分割的块的大小。可省默认20*1024*1024


## 回调函数：	
f.onAddFiles=function(files){}：  添加文件触发 参数：添加的文件数组
f.onStart=function(){}：总开始上传文件  无参
f.onComplete=function(){}：  文件队列中的文件全部文件完成  无参
f.onStartOneFile=function(file){}：文件队列中一个文件开始上传  参数：开始的上传的一个文件
f.onCompleteOneFile=function(file){}  文件队列中一个文件上传完成  参数：完成的文件
f.onProgress=function(file,loaded,total){}:当前正在上传中文件的进度  参数：正在上传的一个文件   上传了多少  总大小
f.ondragenter=function(dom){} 拖动的文件进入绑定的dom  参数：绑定的dom
f.ondragleave=function(dom){} 拖动的文件离开绑定的dom  参数：绑定的dom
f.ondrop=function(dom){}  拖动的文件在dom中释放  参数：绑定的dom
f.onPause=function(){} 暂停当前文件的上传  无参

## 方法：
f.pause() 暂停当前文件的上传
f.upload() 暂停后调用，会继续上传

可以参考 fileupload.html简单示例
