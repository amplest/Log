# Tools

## git

如何将本地项目通过Git工具推送至Github
1. 创建ssh，生成`id_rsa`和`id_rsa.pub`文件
``` sh
$ git config --global user.name "youremailname"
$ git config --global user.email "youremailname@gmail.com"
$ ssh-keygen -t rsa -C "youremailname@gmail.com”
```
2. `id_rsa.pub`文件中的秘钥复制到github的`SSH and GPG keys`
3. 使用如下命令在对应的项目文件夹内分别完成远程库链接及上传操作
``` sh
$ git init              # 初始化本地代码仓
$ git add -A            # 添加本地代码
$ git commit -m "..."   # 提交本地代码
$ git remote add origin git@github.com:username/repositoriesname.git #添加远程版本库
$ git push origin master -f # 上传代码
```
4. 常见问题&命令 
``` sh
# warning: LF will be replaced by CRLF in xxx (问题)
$ git config --global core.autocrlf false
# 检查本机是否有.ssh文件
cd ~/.ssh
```

## npm 

``` sh
$ npm -v                        # 查看node版本
$ npm cache clean -f            # 清楚nodejs的cache
$ npm install npm@latest -g     # 更新npm到最新版本
```

## docsify
使用docsify写文档发布到github设置github pages
- 全局安装docsify，具体安装步骤 -> [docsify官网](https://docsify.js.org/#/zh-cn/quickstart)
- github中创建存放docsify文档的仓库，并开启`github pages`，`source`类型为`master branch`（这样就可以吧docsify中docs文件夹内的文件直接传到master分支使用）
- 如果自定义域名的话，`Custom domain`中填写域名，然后再域名管理台进行解析（CNAME）
- 在`index.html`同级目录下增加`CNAME`文件，文件中写上自己需要指向的域名即可

## wampserve
WampServer多站点多目录配置

- `httpd.conf`文件修改，路径`\wamp\bin\apache\apache2.4.9\conf\httpd.conf`
  - 查找到`#Include conf/extra/httpd-vhosts.conf`，去掉`#`
- `httpd-vhosts.conf`文件修改，路径`\wamp\bin\apache\Apache2.4.9\conf\extra\httpd-vhosts.conf`，添加如下代码
  - DocumentRoot：项目路径
  - ServerName：服务器名称，写成域名，方便区分
  - Directory：项目路径，与 DocumentRoot 无脑一致即可

```
<VirtualHost *:80>   
    DocumentRoot "D:/wamp/www/test"   
    ServerName test.cn   
    <Directory "D:/wamp/www/test">
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        Allow from all
    </Directory>
</VirtualHost>
```

- 添加本地解析，`hosts`文件修改，路径`Windows\System32\drivers\etc\hosts`，添加如下内容

```
127.0.0.1     test.cn
```

## vs code

Setting Sync 配置

## 快速导出文件树结构

``` sh
npm install -g treer
# 忽略某些文件或文件夹
treer -i "node_modules"
# 保存目录结构到文件
treer -e "test.txt"
```