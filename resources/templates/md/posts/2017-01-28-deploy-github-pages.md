{:title "github pages搭建博客"
 :layout :post
 :tags  ["cryogen" "clojure" "github"]}

### 安装cryogen

```bash
lein new cryogen myblog
cd myblog
lein ring server
```
创建一个脚本blog.cmd，并放入到PATH指向的目录下
```cmd
cd c:/msys64/home/chenyu/myblog
lein ring server
```

### 修改Cryogen配置参数

编辑resources/templates/config.edn,修改配置参数blog-prefix为空。
修改后，需要运行lein ring server，这样才会重新产生resources/public目录下的文件

### register and git a github pages account robinchenyu.github.io

参考: [pages.github.com](https://pages.github.com/)

### 克隆robinchenyu.github.io项目到本地

克隆项目到本地后，把templates/public下的所有文件放到项目中master分支；并把myblog目录下的工程文件全部放入到source分支

---------------------------------------------
添加工程文件到source分支
```bash
cd ~/myblog
git init
git add *
git add .gitignore
git commit -m "add source files"
git branch source
git checkout source
git remote add git@github.com:robinchenyu/robinchenyu.github.io
git branch --set-upstream-to=origin/source source
git push source origin
```
--------------------------------------------
添加html文件到master分支
```bash
cd ~/myblog/resources/public
git init
git add *
git commit -m "add html files"
git remote add git@github.com:robinchenyu/robinchenyu.github.io
git branch --set-upstream-to=origin/master master
git push master origin
```

# then merge with github page files

git checkout master

然后测试
[robinchenyu.github.io](http://blog912.cn)

### 域名绑定

1. 在github.com博客项目中根目录下新加CNAME文件，文件内容为新域名
   如: blogcn.cn
2. 在购买的域名中设置DNS服务器
   f1g1ns1.dnspod.net
   f1g1ns2.dnspod.net 
3. 在DNSPod中添加github pages的域名
   - 先添加一个CNAME，主机记录写@，后面记录值写上你的username.github.io
   - 再添加一个CNAME，主机记录写www，后面记录值也是username.github.io
   把username替换为你的帐户名
