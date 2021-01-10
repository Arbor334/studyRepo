## Selinux_study

###  :accept: 所用到的命令

- `ls -Z  filename`   查看文件的Selinux 信息
- `seinfo -u`       查看系统中的身份

### 什么是Selinux

> Security Enhanced Linux 的缩写， 安全强化linux 
>
> 简单来说，我们平常对用户设置的RWX权限只是一种名为`自主访问控制（DAC）`的管理`传统文件权限与用户关系`的方式，这种方式对root是无效的。
>
> <font color="red">但是</font>，这种方式是不太安全的。如果有一个进程被别有用心的人获取，还是一个root进程的话，这个进程就可以在系统上执行对资源的读写操作。这样是肯定不行的
>
> <font color="green">与之相对应的</font>，是采用了` 以策略规则制定特定进程读取特定文件`的  `强制访问控制(MAC)`方式，即只能借由这个进程和自己默认的权限来处理它自己的文件资源。

### Selinux 运行模式

