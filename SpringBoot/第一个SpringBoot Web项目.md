# 一、创建项目

在IDEA 中用Spring Assistant创建SpringBoot项目

如果没有Spring Assistant选项，则去插件中下载

![](image/20220507192809.png)

![](image/20220507192959.png)

![](image/QQ图片20220507193038.png)

# 二、建包

![](image/QQ图片20220507193144.png)

主入口，注意注解

```java
//这个Mian类本身就是SpringBoot的一个组件
//@SpringBootAppLlication :标注这个类是一个springboot的应用
@SpringBootApplication
public class FirstspringprogectApplication {

	public static void main(String[] args) {
		//将springboot的应用启动
		SpringApplication.run(FirstspringprogectApplication.class, args);
	}

}

```

在controler包中新建“接口”。一个可以供前端访问的接口,注意注解

```java
@RestController
@RequestMapping("/hello")
public class HelloController {
    //接口：http://localhost:8080/hello/hello
    @GetMapping("/hello")
    @ResponseBody
    public String hello(){
        //调用业务，接收前端参数
        return "helloworld";
    }
}
```

运行项目后tomcat服务会启动,tomcat服务是默认端口号是8080

![](image/QQ图片20220507193858.png)

此时可以在**浏览器**访问刚刚写好的接口:http://localhost:8080/hello/hello

![](image/QQ图片20220507193933.png)

### 三、自定义设置

## 修改tomcat服务的端口号

当然，也可以修改tomcat服务的端口号，找到目录中的**application.properties**文件进行修改

```ini
#更改项目的端口号
server.port=8081
```

![](image/QQ图片20220507194208.png)

## 修改SpringBoot banner样式

![](image/QQ图片20220507194606.png)

找一个字符艺术字体网站：https://www.bootschool.net/ascii-art/search

找一个你喜欢的复制下来

在目录的resources中新建一个banner.txt

![](image/QQ图片20220507195329.png)

把字符粘贴进去

![](image/QQ图片20220507195355.png)

然后运行项目就好了！

![](image/QQ图片20220507195435.png)

