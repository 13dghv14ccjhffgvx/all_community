1、找个印象最深的项目说说？(简历中不止一个项目)
2、你项目中遇到的最大的问题是什么？你是怎么解决的？
3、你项目中用到的技术栈是如何学习的？
4、为什么做这个项目，技术选型为什么是这样的？
5、登录怎么做的？单点登录说说你的理解？
6、项目遇到的最大挑战是什么？(类似问题2)
7、说说项目中的闪光点和亮点？
8、项目怎么没有尝试部署上线呢？
9、介绍项目具体做了什么？(项目背景)
10、如果让你对这个项目优化，你会从哪几个点来优化呢？

### Github Token: `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`

小技术点记录：

index.html

![image-20230913191446973](C:\Users\86152\AppData\Roaming\Typora\typora-user-images\image-20230913191446973.png)

register.html

![image-20230913191639616](C:\Users\86152\AppData\Roaming\Typora\typora-user-images\image-20230913191639616.png)

th:fragment / include / replace
（1）th:fragment
用于在页面中指定一个通用（复用）的代码片段，例如在页面 common/view1.html 中有如下定义：

```html
<div th:fragment="pageHead">
	一些代码，比如说同一个网站每个页面的头和尾都相同，将公共的代码抽取出来写在这里
</div>
```

（2）th:include
在页面中引入 th:fragment 定义的代码片段，主要有下面三种形式：

th:inclued="view::selector"："::"前面是模板文件名，后面是选择器
::selector：只写选择器，这里指fragment名称，则加载本页面对应的fragment
view或者"view::html"：只写模板文件名或者使用 html 标签，则加载整个页面
比如说，加载（1）中 th:fragment 定义的代码片段：

```html
<div th:include="common/view1::pageHead"></div>
```

（3）th:replace
th:replace 和 th:include 都是加载代码块内容，但是还是有所不同

* `th:include`：加载模板的内容： 读取加载节点的内容（不含节点名称），替换div内容

* `th:replace`：替换当前标签为模板中的标签，加载的节点会整个替换掉加载它的div

例如：

```html
<!-- th:fragment 定义用于加载的块 -->
<span th:fragment="view"> 
这是公共部分
</span>
```

引用时如下：

```html
include：
<div th:include="pagination::view">1</div>
replace：
<div th:replace="pagination::view">2</div>

```

结果如下：

```html
include：
<div>这是公共部分</div>
replace：
<span>这是公共部分</span>

```



1. md5加密技术的原理

   String.format(String format, Object... args)

   ![image-20230919165452638](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230919165452638.png)

   

2. 会话管理

* HTTP的基本性质

  * HTTP是简单的
  * HTTP是可扩展的
  * HTTP是无状态的，有会话的

  ![image-20230919165958211](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230919165958211.png)

  ==注意：==HTTP本质是无状态的，使用Cookies可以创建有状态的会话



* Cookie
  * 是服务器发送给浏览器，并保存在浏览器端（客户端）的一小块数据
  * 浏览器下次访问该浏览器时，会自动携带该数据，将其发送给服务器



* Session
  * 是JavaEE的标准，用于在服务端记录客户端信息，保存在服务器端
  * 数据存放在服务器端更加安全，但是也会增加服务端的内存压力





1.30 开发首页

1.47 版本控制

2.1 发送邮件

2.7 开发注册功能
String.format(String format,Object... args)

2.11 会话管理（cookie、session）

2.17 生成验证码

* 导包

```xml
<dependency>
    <groupId>com.github.penggle</groupId>
    <artifactId>kaptcha</artifactId>
    <version>2.3.2</version>
</dependency>
```

**主要的一个类**：

![image-20230919180500120](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230919180500120.png)

![image-20230919180719654](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230919180719654.png)

实现类如下：

![image-20230919180838769](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230919180838769.png)

* 编写kaptcha配置类：KaptchaConfig类

```java
 package com.example.demo.config;

import com.google.code.kaptcha.impl.DefaultKaptcha;
import com.google.code.kaptcha.util.Config;
import org.springframework.context.annotation.Bean;

import org.springframework.stereotype.Component;

import java.util.Properties;

/**
 * @Configuration 和 @Component
 * @Configuation 的本质就是 Component
 */
@Component
public class KaptchConfig {
    @Bean
    public DefaultKaptcha getDefaultKaptcha() {
        com.google.code.kaptcha.impl.DefaultKaptcha defaultKaptcha = new com.google.code.kaptcha.impl.DefaultKaptcha();
        Properties properties = new Properties();
        // 图片边框
        properties.setProperty("kaptcha.border", "no");
        // 边框颜色
        properties.setProperty("kaptcha.border.color", "black");
        //边框厚度
        properties.setProperty("kaptcha.border.thickness", "1");
        // 图片宽
        properties.setProperty("kaptcha.image.width", "120");
        // 图片高
        properties.setProperty("kaptcha.image.height", "60");
        //图片实现类
        properties.setProperty("kaptcha.producer.impl", "com.google.code.kaptcha.impl.DefaultKaptcha");
        //文本实现类
        properties.setProperty("kaptcha.textproducer.impl", "com.google.code.kaptcha.text.impl.DefaultTextCreator");
        //文本集合，验证码值从此集合中获取
        properties.setProperty("kaptcha.textproducer.char.string", "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ");
        //验证码长度
        properties.setProperty("kaptcha.textproducer.char.length", "4");
        //字体
        properties.setProperty("kaptcha.textproducer.font.names", "宋体");
        //字体颜色
        properties.setProperty("kaptcha.textproducer.font.color", "black");
        //文字间隔
        properties.setProperty("kaptcha.textproducer.char.space", "4");
        //干扰实现类
        properties.setProperty("kaptcha.noise.impl", "com.google.code.kaptcha.impl.DefaultNoise");
        //干扰颜色
        properties.setProperty("kaptcha.noise.color", "blue");
        //干扰图片样式
        properties.setProperty("kaptcha.obscurificator.impl", "com.google.code.kaptcha.impl.WaterRipple");
        //背景实现类
        properties.setProperty("kaptcha.background.impl", "com.google.code.kaptcha.impl.DefaultBackground");
        //背景颜色渐变，结束颜色
        properties.setProperty("kaptcha.background.clear.to", "white");
        //文字渲染器
        properties.setProperty("kaptcha.word.impl", "com.google.code.kaptcha.text.impl.DefaultWordRenderer");
        Config config = new Config(properties);
        defaultKaptcha.setConfig(config);
        return defaultKaptcha;
    }

}
```

* Controller层

```java
    @Resource
    DefaultKaptcha defaultKaptcha;
    //生成验证码
    @RequestMapping("/Code")
    public ResultVo Code() throws IOException {
        // 生成文字验证码
        String text=defaultKaptcha.createText();
        System.out.println("文字验证码为"+text);
        // 生成图片验证码
        ByteArrayOutputStream out = null;
        BufferedImage image = defaultKaptcha.createImage(text);
        out=new ByteArrayOutputStream();
        ImageIO.write(image,"jpg",out);
        // 对字节组Base64编码
        return ResultVo.success("img",Base64.getEncoder().encodeToString(out.toByteArray()));
    }
```





2.23 登录、退出登录功能

![image-20230920085800366](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230920085800366.png)



自己：开发忘记密码的功能

* 点击登录页面的“忘记密码”链接，打开忘记密码页面
* 在表单中输入注册的邮箱，点击获取验证码按钮，服务器为该邮箱发送一份验证码
* 在表单中填写收到的验证码及新密码，点击重置密码，服务器对密码进行修改



2.27 显示登录信息

![image-20230921205904763](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230921205904763.png)

![](https://gitee.com/huang-jintong/csdn-blog-gallery/raw/master/img/image-20230924121118047.png)







2.33 账号设置

2.41 检查登录状态

