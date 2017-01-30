{:title "github pages搭建博客"
 :layout :post
 :tags  ["cryogen" "clojure" "github"]}

### 安装cryogen

```bash
lein new cryogen my-blog
cd my-blog
lein ring server
```

### 修改Cryogen配置参数

编辑resources/templates/config.edn,修改配置参数blog-prefix为空。
修改后，需要运行lein ring server，这样才会重新产生resources/public目录下的文件

### register and git a github pages account robinchenyu.github.io

参考: [pages.github.com](https://pages.github.com/)

### 克隆robinchenyu.github.io项目到本地

克隆项目到本地后，拷贝templates/public下的所有文件到项目中，然后测试
[robinchenyu.github.io](https://robinchenyu.github.io)

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
