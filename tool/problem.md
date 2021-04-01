# 常见问题及帮助

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
5. 如何针对项目去设置提交者邮箱
``` sh
# 或者设置本地项目库配置
git config user.name "Author Name"
git config user.email "Author Email"
```
6. 存在冲突需要考虑merge的时候
``` sh
git reset --soft HEAD~2
```
7. 本地删除文件后需要拉新文件的操作
``` sh
git fetch --all
git reset --hard origin/master
git pull
```
8. 提交规范

- feat：新功能（feature）。

- fix/to：修复bug，可以是QA发现的BUG，也可以是研发自己发现的BUG。

- fix：产生diff并自动修复此问题。适合于一次提交直接修复问题

- to：只产生diff不自动修复此问题。适合于多次提交。最终修复问题提交时使用fix

- docs：文档（documentation）。

- style：格式（不影响代码运行的变动）。

- refactor：重构（即不是新增功能，也不是修改bug的代码变动）。

- perf：优化相关，比如提升性能、体验。

- test：增加测试。

- chore：构建过程或辅助工具的变动。

- revert：回滚到上一个版本。

- merge：代码合并。

- sync：同步主线或分支的Bug。

9. 放弃本期修改,强制拉取最新代码

``` sh
git fetch --all
git reset --hard origin/4.0_eio
git pull
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

## 快速导出文件树结构

``` sh
npm install -g treer
# 忽略某些文件或文件夹
treer -i "node_modules"
# 保存目录结构到文件
treer -e "test.txt"
```

## vs code 配置信息

``` json
{
  "sync.gist": "",
  "workbench.settings.useSplitJSON": true,
  "workbench.iconTheme": "material-icon-theme",
  "files.defaultLanguage": "html",

  "editor.formatOnSave": false,
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "vue"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },

  "eslint.run": "onSave",

  "window.zoomLevel": 0,

  "vetur.format.defaultFormatter.html": "js-beautify-html",
  "vetur.format.defaultFormatter.js": "vscode-typescript",
  "vetur.format.defaultFormatterOptions": {
    "prettier": {
      // 格式化不加分号
      "semi": false,
      // 格式化为单引号
      "singleQuote": true
    },
    "js-beautify-html": {
      "wrap_attributes": "auto"
    }
  },

  "explorer.confirmDelete": false,
  "files.autoSave": "onFocusChange",

  "[vue]": {
    "editor.defaultFormatter": "octref.vetur"
  },
  "[html]": {
    "editor.defaultFormatter": "vscode.html-language-features"
  },
  "[javascript]": {
    "editor.defaultFormatter": "vscode.typescript-language-features"
  },
  "explorer.confirmDragAndDrop": false,
  "[json]": {
    "editor.defaultFormatter": "vscode.json-language-features"
  },
  "javascript.format.insertSpaceAfterFunctionKeywordForAnonymousFunctions": false,
  "workbench.editorAssociations": [

  ],
  
  "liveSassCompile.settings.formats":[
    // 扩展
    {
        "format": "compact",//可定制的出口CSS样式（expanded，compact，compressed，nested）
        "extensionName": ".min.css",//编译后缀名
        "savePath": "/css"//编译保存的路径
    } 
    
  ],
"liveSassCompile.settings.excludeList": [
    "**/node_modules/**",
    ".vscode/**"
  ]
}
```