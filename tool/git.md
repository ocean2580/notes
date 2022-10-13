# Git操作

## 初始化

```
git init 初始化

git status  查看状态

```



```
vim h.txt  新建/修改 文件
```

   Insert 进入编辑模式

   Esc 退出编辑模式



 Esc 后： yy复制  p黏贴

​              :wq 保存

​       ![image-20220422123243712](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422123243712.png)



## 文件添加/移出至暂存区（追踪）

```
git add 文件名 
```

![image-20220422123945008](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422123945008.png)



本地 - > 工作区



## 提交本地库

==形成历史版本==

```
git commit -m"日志信息" 文件名
```

![image-20220422205148105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422205148105.png)

工作区 - > 本地库



## 历史版本查看

```
git relog
git log # 更准确
```

![image-20220422205539241](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422205539241.png)



## 修改文件

当已commit文件被修改

![image-20220422205838946](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422205838946.png)

==clear 或者 ctrl+L 清屏==



对被修改文件 add, commit:

![image-20220422210437936](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422210437936.png)

```
cat a.txt # 查看文件内容
```



## 历史版本穿梭

```
git reset --hard 版本号
```

==移动head指针==

![image-20220422211356903](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422211356903.png)

.git文件的内容随git操作改变



# 分支

![image-20220422212652006](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422212652006.png)

版本控制过程中，同时推进多个任务，为每个任务创建==单独==的分支（==深拷贝副本==），从开发主线分离开（ 指针的引用），如果有一个分支功能开发失败，直接删除这个分支就可以



![image-20220422213227803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422213227803.png)

同时==并行==推进多个功能的开发



![image-20220422213528675](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422213528675.png)



## 查看

```
git branch -v  
```

![image-20220422213700199](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422213700199.png)



## 创建

```
git branch 分支名  
```

![image-20220422213843982](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422213843982.png)



## 切换

```
git checkout 分支名  
```

![image-20220422214201339](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422214201339.png)

修改并提交：

![image-20220422214609092](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422214609092.png)



## 合并分支

```
git merge 分支名  # 指定 -> 当前
```

![image-20220422215431537](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422215431537.png)



> 冲突：如果两个分支在同一文件的同一位置有两套完全不同的修改，git无法决定，需要==人为地==决定新代码内容



![image-20220422220638245](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422220638245.png)

![image-20220422220653582](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422220653582.png)

 

冲突部分：

![image-20220422220751072](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422220751072.png)



解决方法：

1) 手动修改

![image-20220422221101750](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422221101750.png)

2) 再执行添加提交（==提交处不加文件名==）

![image-20220422221546268](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422221546268.png)



# Git团队协作

 

 队内：

![image-20220422223445941](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422223445941.png)





跨团队：

![image-20220422223936136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422223936136.png)





# GitHub操作

## 远程仓库操作

![image-20220422225955290](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422225955290.png)

远程地址 为 ==远程仓库的HTTP/SSH==



### 别名

```
git remote add 别名 SSH
```

![image-20220422230315526](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422230315526.png)



初始化，==通过http连接本地与远程==



### 推送分支

```
git push 别名 分支名
```

![image-20220422230837856](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422230837856.png)



修改后再次推送：

![image-20220422231439170](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422231439170.png)



==如果添加新的文件，先 pull 再 push==



分支提交：

![image-20220422234914769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422234914769.png)

![image-20220422234854476](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422234854476.png)

自动创建分支



## 拉取

```
git pull 别名 （远程）分支名
```

网页修改：

![image-20220422231704295](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422231704295.png)

![image-20220422232248220](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422232248220.png)



## 克隆

```
git clone SSH
```

![image-20220422233615855](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422233615855.png)

只需要对（新建）文件夹 init 后 clone



clone 会 ：

 1、拉取代码 2、初始化本地仓库 3、创建别名

![image-20220422234120719](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220422234120719.png)



# 与IDEA集成

## 文件配置

C:\Users\Administrator 中

配置好 git.ignore             .gitconfig



## 定位

![image-20220423101457232](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220423101457232.png)



## 添加

Git > GitHub >

![image-20220423102043571](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20220423102043571.png)



==提交src文件==

