# 用`Glide`加载图片

## 1.依赖(最新上GitHub)

```xml
implementation 'com.github.bumptech.glide:glide:4.11.0'
annotationProcessor 'com.github.bumptech.glide:compiler:4.11.0'
```

## 2.基本使用

**加载图片三步走，1,with; 2,load; 3 into;**

```java
Glide.with().load().into(imageView);
```

`with()`方法可以接收Context、Activity或者Fragment类型的参数 ;

`load()`方法中不仅可以传入图片地址，还可以传入图片文件File，resource，图片的byte数组等 ;

## 3.更多Glide用法

[Glide的基本用法1]: https://blog.csdn.net/xxdw1992/article/details/93624487
[Glide的基本用法2]: https://blog.csdn.net/c10WTiybQ1Ye3/article/details/78098598
[Glide原理解析]: https://blog.csdn.net/chengxuyuan22/article/details/115345179