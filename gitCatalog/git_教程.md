## git工作区域

[图片来源](<https://www.bilibili.com/video/BV1Xx411m7kn?p=7>)

![](D:\TyporaImg\git\git.jpg)

### 向仓库中添加文件流程

![](D:\TyporaImg\git\git1.jpg)

# git初始化及仓库创建和操作

## 基本信息设置

- 设置用户名

  > git config --global user.name 'Arbor334'

- 设置用户名邮箱

  > git config --global user.email 'xxxxxxxx@qq.com'

![](D:\TyporaImg\git\github.jpg)

## 初始化一个新的仓库

- 创建文件夹

![](D:\TyporaImg\git\github1.jpg)

###  初始化仓库

![](D:\TyporaImg\git\github2.jpg)

### 向仓库中添加文件[点击](#git工作区域 )

> 工作区---> 暂存区----> git仓库 

![](D:\TyporaImg\git\github3.jpg)

> 添加到缓存区

![](D:\TyporaImg\git\github4.jpg)

> 添加到仓库

![](D:\TyporaImg\git\github5.jpg)

### 修改仓库文件

![](D:\TyporaImg\git\github6.jpg)

![](D:\TyporaImg\git\github7.jpg)

### 删除仓库文件

![](D:\TyporaImg\git\github8.jpg)

# git 远程仓库

![](D:\TyporaImg\git\git9.jpg)

![](D:\TyporaImg\git\github9.jpg)

## git链接远程仓库





## git克隆操作

> 目的： 将远程仓库（github对应的项目）复制到本地

```git
git clone 仓库地址
```

- 仓库地址

  ![](D:\TyporaImg\git\github10.jpg)

![](D:\TyporaImg\git\github11.jpg)

```git
从克隆到提交到远程仓库步骤：

【1】、 git clone 仓库地址
【2】、 进入到要修改的目录下 修改文件
【3】、git status 查看状态
【4】、git add  修改的文件
【5】、git status 查看状态
【6】、 git push 上传  

## ： 如果遇到Authentication failed 这个错误 
        可能是因为 没有设置密码  
        >   git  config  --global user.password  你的密码
        应该就可以了
##: 还有一件事
         查看配置   
         >git config --list

```

# 个人站点

## 访问

> https://用户名.github.io

## 搭建步骤

【1】、创建个人站点-> 新建仓库（仓库名必须是【用户名.gituhb.io】）

【2】、在仓库下新建index.html文件即可

![](D:\TyporaImg\git\io.jpg)

- 打开网页搜索 https://用户名.github.io   会出现 404

这里我们要新建一个index.html文件

![](D:\TyporaImg\git\github13.jpg)

![](D:\TyporaImg\git\github14.jpg)

- 再次搜索 https://用户名.github.io  

![](D:\TyporaImg\git\github15.jpg)

> 【1】、github pages 仅支持静态网页  
>
> 【2】、仓库里面只能是.html文件 

## Project pages 项目站点

- https://用户名.github.io/仓库名

### 搭建步骤

【1】、进入项目主页，点击settings

【2】、在settings页面，点击

