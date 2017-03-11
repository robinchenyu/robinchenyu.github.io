+++
date = "2017-02-16T08:36:54+05:30"
draft = true
title = "Fabric warn_only设置"
+++

### 使用worn_only控制出错

使用fabric脚本非常部署项目非常方便, 但是fabric默认会判断返回值，如果返回值
不为0则会导致脚本中断执行
使用方法如下
```python
with settings(warn_only=True):
    run("ls noexistdir")
```

这时在访问一个不存在的文件夹并不会停止脚本，会继续执行下面的动作，但是会打印出错误日志。