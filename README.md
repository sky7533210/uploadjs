# uploadjs
前端的一个文件上传的插件
本插件在里面集成了spark-md5,不需要在引入md5了。
# 代码流程
1.在你的脚本中new FileUpload后出file选择一个文件后spark-md5会自动计算它前20MB的md5(这个可以再new 出设置)
