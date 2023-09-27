## 框架Thymeleaf的学习

------



## 一、基本表达式

#### 1、**@{}**：thymeleaf中的超链接表达式

用于获取链接地址

```html
<a th:href="@{/user/login}"></a>
<a th:src="@{/user/login}"></a>
```

#### 2、**#{}**：#{}取出来的值取代了标签的值

一般和th:text一起使用较多

```html
<div th:text="#{user}"></div>
```

#### 3、**${}**：变量表达式，用于访问的是**容器上下文的变量**，比如**域变量**：**request**域，**session**域

```html
// 后端代码
session.setAttribute("list",list);

// 前端代码
<!-- 取域中的值 -->
<div>[[${list}]]</div>
```

* （1）访问Request作用域中的值

  假设 request 作用域中存在键值对：`"key"=value`，可以使用
  使用标准变量表达式：

  ```html
  <span th:text="${#httpServletRequest.getAttribute('key')}"></span>
  或者：<span th:text="${key}"></span>
  ```

- 注意：
  - 因为Model中的值本质上是在request作用域中存储的，所以两者的取值方法可以混用
  - 有些标准变量表达式在IDEA中可能会报错，但是在运行时依旧可以取到值

* （2）访问Session会话作用域中的值

  假设session 作用域中存在键值对：`"key"=value`使用标准变量表达式需要加个前缀 `session`：

```html
<span th:text="${session.key}"></span>
```

* （3）访问Application全局作用域中的值

  假设 application 作用域中存在键值对：`"key"=value`，使用标准变量表达式需要加个前缀 `application`：

```html
<span th:text="${application.key}"></span>
```

* 空值处理（重要）

语法：${对象名?.属性名}

如果一个对象**可能**为 null 时，并且要**获取该对象的属性**时，这时需要在该对象后面加一个"`?`"，当一个为 null 的对象去获取一个属性时，会报`TemplateInputException`（模板输入/解析异常）

#### 4、***{}**：选择表达式



## 二、常见的th标签表

| 关键字      | 功能介绍                                     | 案例                                                         |
| ----------- | -------------------------------------------- | ------------------------------------------------------------ |
| th:id       | 替换id                                       | `<input th:id="'xxx' + ${collect.id}"/>`                     |
| th:text     | 文本替换                                     | `<p th:text="${collect.description}">description</p>`        |
| th:utext    | 支持html的文本替换                           | `<p th:utext="${htmlcontent}">conten</p>`                    |
| th:object   | 替换对象                                     | `<div th:object="${session.user}">`                          |
| th:value    | 属性赋值                                     | `<input th:value="${user.name}" />`                          |
| th:with     | 变量赋值运算                                 | `<div th:with="isEven=${prodStat.count}%2==0"></div>`        |
| th:style    | 设置样式                                     | `th:style="'display:' + @{(${sitrue} ? 'none' : 'inline-block')} + ''"` |
| th:onclick  | 点击事件                                     | `th:onclick="'getCollect()'"`                                |
| th:each     | 属性赋值                                     | `tr th:each="user,userStat:${users}">`                       |
| th:if       | 判断条件                                     | `<a th:if="${userId == collect.userId}" >`                   |
| th:unless   | 和th:if判断相反                              | `<a th:href="@{/login}" th:unless=${session.user != null}>Login</a>` |
| th:href     | 链接地址                                     | `<a th:href="@{/login}" th:unless=${session.user != null}>Login</a> />` |
| th:switch   | 多路选择 配合th:case 使用                    | `<div th:switch="${user.role}">`                             |
| th:case     | th:switch的一个分支                          | `<p th:case="'admin'">User is an administrator</p>`          |
| th:fragment | 布局标签，定义一个代码片段，方便其它地方引用 | `<div th:fragment="alert">`                                  |
| th:include  | 布局标签，替换内容到引入的文件               | `<head th:include="layout :: htmlhead" th:with="title='xx'"></head> />` |
| th:replace  | 布局标签，替换整个标签到引入的文件           | `<div th:replace="fragments/header :: title"></div>`         |
| th:selected | selected选择框 选中                          | `th:selected="(${xxx.id} == ${configObj.dd})"`               |
| th:src      | 图片类地址引入                               | `<img class="img-responsive" alt="App Logo" th:src="@{/img/logo.png}" />` |
| th:inline   | 定义js脚本可以使用变量                       | `<script type="text/javascript" th:inline="javascript">`     |
| th:action   | 表单提交的地址                               | `<form action="subscribe.html" th:action="@{/subscribe}">`   |
| th:remove   | 删除某个属性                                 | `<tr th:remove="all"> 1.all:删除包含标签和所有的孩子。2.body:不包含标记删除,但删除其所有的孩子。3.tag:包含标记的删除,但不删除它的孩子。4.all-but-first:删除所有包含标签的孩子,除了第一个。5.none:什么也不做。这个值是有用的动态评估。` |
| th:attr     | 设置标签属性，多个属性可以用逗号分隔         | 比如 `th:attr="src=@{/image/aa.jpg},title=#{logo}"`，此标签不太优雅，一般用的比较少。 |

==注意：==

> 由于一个标签内可以包含多个th:x属性，其生效的优先级顺序为：
>
> `include,each,if/unless/switch/case,with,attr/attrprepend/attrappend,value/href,src ,etc,text/utext,fragment,remove`



### 三、Thymeleaf框架的使用

#### 1、添加依赖（pom.xml）

```xml
<!--SpringBoot 集成 Thymeleaf 的起步依赖-->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>

```



#### 2、基本配置（application.properties）

* thymeleaf 页面的缓存开关，默认 true 开启缓存（建议在开发阶段关闭 thymeleaf 页面缓存，目的实时看到页面）

```properties
spring.thymeleaf.cache=false
```

* thymeleaf  的前缀与后缀，下面给出的是SpringBoot的默认值，也可以通过这两个参数进行修改

```properties
# 前缀：
spring.thymeleaf.prefix=classpath:/templates/
# 后缀：
spring.thymeleaf.suffix=.html
```

* ==注意：==在默认配置下，`thymeleaf`对`.html`的内容要求很严格，建议增加下面的配置：

```properties
#去掉html5语法验证
spring.thymeleaf.mode=LEGACYHTML5
```

`spring.thymeleaf.mode`的默认值是`HTML5`，其实是一个很严格的检查，改为`LEGACYHTML5`可以得到一个可能更友好亲切的格式要求。

使用该配置时，需要额外引入`NekoHTML`库，相关依赖：

```xml
<dependency>
    <groupId>net.sourceforge.nekohtml</groupId>
    <artifactId>nekohtml</artifactId>
</dependency>
```



#### 3、 后端通过[Model](https://so.csdn.net/so/search?q=Model&spm=1001.2101.3001.7020)传值

通过**Model对象将后台的值传递回前台**，Model对象实际上是将后台的数据放到了**request作用域**中，也可以使用HttpServletRequest对象进行传值，但是在idea中会显示报错，且没有提示，但是值可以正常取出来。推荐使用Model对象

```java
@Controller
public class ThymeleafController {

    @RequestMapping("/leaf")
    public String leaf(Model model) {
        model.addAttribute("data", "SpringBoot集成Thymeleaf");
        return "aa";
    }
}
```



#### 4、创建Thymeleaf文件

Springboot 使 用 thymeleaf 作 为 视 图 展 示 ， 约 定 将 模 板 文 件 放 置 在 src/main/resource/templates 目录下，静态资源放置在 src/main/resource/static 目录下。

在templates新建一个普通的HTML5文件，然后在html标签中添加属性xmlns:th="http://www.thymeleaf.org"就可以了

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org"></html>
```