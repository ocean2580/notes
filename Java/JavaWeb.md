![image-20220424221333126](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220424221333126.png)

![image-20220424221817276](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220424221817276.png)



# HTML （超文本标记语言）

![image-20220424222524738](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220424222524738.png)



# CS / BS

![image-20220430091759408](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430091759408.png)





# tomcat

![image-20220430100048680](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430100048680.png)



IDEA 中 添加配置 tomcat local



![image-20220430125159969](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430125159969.png)



## 简介

![02.server_Tomcat_deploy](E:\JavaWeb\代码素材资料\Day3-tomcat-servlet\资料\02.server_Tomcat_deploy.png)





## 过程

![03.第一次使用Servlet](E:\JavaWeb\代码素材资料\Day3-tomcat-servlet\资料\03.第一次使用Servlet.png)



## 添加 HttpServlet

![image-20220430215546090](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430215546090.png)



## 添加 tomcat

![image-20220430215755329](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430215755329.png)



## 添加 工件

![image-20220430221217731](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430221217731.png)



## tomcat 部署

![image-20220430221326349](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430221326349.png)



==如果连接不上==

![image-20220501203129352](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501203129352.png)





## lib包

![image-20220501094918044](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501094918044.png)





## 总结

1. ==外部库添加 jar包 / libs添加时，给 tomcat/lib 里也添加一份==
2. 添加工件- > 添加部署
3. ![image-20220430234207818](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220430234207818.png)


# Servlet

## 编码

```Java
// 设置编码，防止中文乱码 (获取参数之前)
        request.setCharacterEncoding("UTF-8");
```



## service方法

![image-20220501103040794](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501103040794.png)



## Servlet生命周期

![image-20220501104549472](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501104549472.png)



## Http协议

![image-20220501112420287](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501112420287.png)

==http无状态== -> 服务器无法判断request的来源 



## 会话跟踪 （session） 

![image-20220501115758311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501115758311.png)



![image-20220501152528077](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501152528077.png)

session 的保存作用域是和具体某一个 session 对应的



## 服务端请求处理

### 服务器内部转发

![image-20220501153655576](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501153655576.png)

### 客户端重定向

![image-20220501153923477](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501153923477.png)

 ![image-20220501160606252](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501160606252.png)





# Thymeleaf

![image-20220501191349866](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220501191349866.png)

```xml
 <!-- 配置上下文参数 -->
    <context-param>
        <param-name>view-prefix</param-name>
        <param-value>/</param-value>
    </context-param>
    <context-param>
        <param-name>view-suffix</param-name>
        <param-value>.html</param-value>
    </context-param>
```

```java
 //此处的视图名称是 index
        //那么thymeleaf会将这个 逻辑视图名称 对应到 物理视图 名称上去
        //逻辑视图名称 ：   index
        //物理视图名称 ：   view-prefix + 逻辑视图名称 + view-suffix
        //所以真实的视图名称是：      /       index       .html
        super.processTemplate("index",req,resp);
    
```



```java
// 注解方式注册
@WebServlet("/index")
```

==该类可以不用在 web.xml 中注册==

