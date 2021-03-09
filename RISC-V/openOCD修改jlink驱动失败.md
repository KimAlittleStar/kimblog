Open OCD win10系统下转换jlink为 winUSB失败
昨日在搭建织女星开发板的Eclipse环境中发现 使用 UsbDriverTool置换jlink驱动为 winUSB时报错:
```不能为驱动添加签名```
 ```驱动添加失败```
 原因为:win10系统强制每个驱动都需要为其添加签名,不添加签名者无法安装.  
 
**解决方法:** 取消win10的强制签名
步骤:开始菜单下点击设置.进入安全与更新->恢复->高级启动->立即重新启动  

**截图来自本人CSDN blog**
![系统截图](https://img-blog.csdnimg.cn/20210309215243573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NTc1NjQ1,size_16,color_FFFFFF,t_70)  

启动后 鼠标点击答疑解难->高级选项->启动设置(更改windows启动行为)->里面只有一个按钮(重启)->重启后进入一个蓝色界面,界面提示按F7禁用强制驱动签名  
完成后重启即可.  
或者使用cmd输入以下命令:  
`bcdedit.exe /set nointegritychecks on`  
恢复强制签名  
`bcdedit.exe /set nointegritychecks off`