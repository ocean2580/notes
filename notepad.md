# Java

命令依赖于执行它的对象不同而有差异。
“声明变量”等同于“创建变量”。

lambda表达式没有命名，用来像传递数据一样传递操作。2.函数接口指的是只有一个抽象方法的接口，被当做是lambda表达式的类型。

Object 是引用数据类型，只申明而不创建实例，只会在栈内存中开辟空间，默认为空，空占1 bit.
所有的泛型在编译阶段就已经被处理成了普通类和方法（擦除）

JVM 是并不支持语法糖的，语法糖在程序编译阶段就会被还原成简单的基础语法结构，真正支持语法糖的是 Java 编译器。



建立session, 保存cookie, 验证token



# Sketch

https://youtu.be/KDverpI-_HE   少年时期
https://youtu.be/FyZyzkbmIHQ   霸气的眼神
https://youtu.be/DlFM6G7R-RU   Mikasa
https://youtu.be/8_20a74BCv8   combination
https://youtu.be/GllS80YNhoo   Mikasa
https://youtu.be/TRaF3wv99LU   Eren
https://youtu.be/k9r-k0O84zY   rukia
https://youtu.be/_n3WKPKXE3k   Mikasa
https://youtu.be/g__PxAxw304   Levi




镁光 8G DDR4 2666频率

# Linux

宝塔：
  外网面板地址: http://116.205.186.165:8888/5e180334
  内网面板地址: http://192.168.0.139:8888/5e180334
  username: azbf7i7s
  password: b4c45863

项目地址：
 http://116.205.186.165:8080/login



apt install openjdk-8-jdk 

看端口：
 lsof -i:9090
 netstat -anp |grep 端口
 kill -9 pid

后端：
 nohup java -jar XXX.jar >temp.txt &

前端：
 启动    /usr/local/nginx/sbin/nginx 
 停止    /usr/local/nginx/sbin/nginx -s stop
 重启   /usr/local/nginx/sbin/nginx -s reload



# Docker



容器是镜像的实例。
镜像类似程序文件是静态的，容器相当于进程是动态的。

镜像：包含应用程序以及其相关依赖的一个基础文件系统，以只读的方式被用于创建容器的运行环境。
只在一个容器中运行一个应用程序
几乎所有的镜像都是通过在base镜像的基础上安装和配置需要的软件构建出来的

JVM -> JRE -> JDK

docker pull REPO
docker images
docker run -it REPO:TAG
docker ps -a
docker start|stop|rm CONTAINER ID|NAMES  
docker rmi REPO
docker history REPO:TAG


vim Dockerfile
...
docker build -t <镜像名称> <构建目录>
docker tag ubuntu-java-file:latest 用户名/仓库名称:版本
docker login -u oliverdu
docker push oliverdu/ubuntu-java:1.0
docker search oliverdu/ubuntu-java
docker pull oliverdu/ubuntu-java:1.0

...
COPY target/DockerTest-0.0.1-SNAPSHOT.jar app.jar
...
docker run -p 8081:9090 -d springboot-test:1.0



## 安装MySQL

https://www.cnblogs.com/niunafei/p/12803829.html







# Interceptor vs Filter

![image-20220913095415553](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220913095415553.png)

过滤器就是==筛选==出你要的东西，比如request中你要的那部分
拦截器在做安全方面用的比较多，比如==终止==一些流程

过滤前-拦截前-action执行-拦截后-过滤后

Filter是基于函数回调的，而Interceptor则是基于Java反射的。
Filter依赖于Servlet容器，而Interceptor不依赖于Servlet容器。
Filter对几乎所有的请求起作用，而Interceptor只能对action请求起作用。
Interceptor可以访问Action的上下文，值栈里的对象，而Filter不能。
在action的生命周期里，Interceptor可以被多次调用，而Filter只能在容器初始化时调用一次。



# SpringSecurity + JWT

认证：验证当前访问系统的是不是本系统的用户，并且要确认具体是哪个用户

授权：经过认证后判断当前用户是否有权限进行某个操作

SpringSecurity的原理其实就是一个==过滤器链==，内部包含了提供各种功能的过滤器。

UsernamePasswordAuthenticationFilter 认证
ExceptionTranslationFilter 异常处理
FilterSecurityInterceptor 授权

Authentication(DTO) 当前用户信息
AuthenticationManager 认证方法
UserDetailsService 加载用户信息
UserDetails()

BCryptPasswordEncoder
加密后字符串的长度为固定的60位。其中：$是分割符，无意义；2a是bcrypt加密版本号；10是cost的值；而后的前22位是salt值；再然后的字符串就是密码的密文了。  
解释一下：$2a是BCrypt的版本号，$10是“强度”（也称为 BCrypt 中的日志轮数），接着22位是随机生成的。即密文的前29位是生成的salt。之后的31位是生成的密文

subject -> createJWT() -> jwt -> parseJWT() -> claims -> getSubject() -> subject

AM authenticate认证 ? userid -> jwt -> ResponseResult : 抛出异常;
完整用户信息存入redis, userid -> key 
chain.doFilter(继续过滤)将请求转发给过滤器链下一个filter , 如果没有filter那就是你请求的资源
token是一个令牌用于客户端和服务端交互的，例如b站点开视频，点开动态。比如登录都属于交互，有些用户如何第一次登录肯定是没有token的，所以应该放行

request -> getHeader("token") -> jwt



前端防君子，后端防小人
Role-Based Access Control(RBAC): 角色即权限集 

User用户，Role角色，Menu权限
user_role, role_menu
小数据连表快，大数据段快

认证异常 封装成AuthenticationException然后调用**AuthenticationEntryPoint**对象的方法
授权异常 封装成AccessDeniedException然后调用**AccessDeniedHandler**对象的方法

同源策略要求源相同(协议、域名、端口号都完全一致)才能正常进行通信。





# Nginx

sudo update apt

sudo apt install nginx

cd /etc/nginx/

sudo vim nginx.conf



http中：

server {
                listen       ==前端端口==;
                server_name  ==公网IP==;
                add_header Access-Control-Allow-Origin *;
                location / {
                        root /usr/project/dist; 
                        index /usr/project/dist/index.html;
                        try_files $uri $uri/ /index.html;
                        charset utf-8;
                }
        }



sudo systemctl restart nginx.service

sudo /etc/nginx/sbin/nginx -s reload



 404:  https://blog.csdn.net/Bean_ph/article/details/121373102

