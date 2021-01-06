## Centos7Selinux安装

[问题解决方案的地址](https://blog.twtnn.com/2012/07/selinux-seinfosesearch.html)

笔者使用的是 Centos7 来学习linux 的

但是使用  `seinfo`命令的时候出现了 command not found

原来是没有安装selinux  

```linux
sudo yum install  setools-console.x86_64
```

安装完毕后 

> seinfo --help 查看即可