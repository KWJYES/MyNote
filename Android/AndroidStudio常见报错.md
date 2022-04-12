# 3 files found with path 'META-INF/DEPENDENCIES'.

大概意思是：错误：在OS独立路径'META-INF / DEPENDENCIES'中找到多个文件

解决办法：

在出现问题的build.gradle的android{}中添加

```xml
packagingOptions {
	// exclude '报错路径'
    exclude 'META-INF/DEPENDENCIES'
}
```

大概原理为：

在Android Gradle构建中,不允许在输出中多次包含具有相同路径的相同文件.在您的构建中,有两个`META-INF/DEPENDENCIES`文件来自不同的地方.由于您的应用程序根本不需要此文件,因此最简单的方法是告诉构建系统完全忽略它,这就是该`exclude`指令的作用.