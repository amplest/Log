# 记录

## 技术栈

- 前端: `vue-admin-template v4.4.0`
- 后端: `Lavaral7`

## 前端部署

``` bash
# 开发环境
npm run dev
# 正式环境
npm run build:prod
```

## 后端环境启动

- PHPStudy (FTP/MySQL/Nginx)
- 环境启动(安装Composer)

``` bash
# composer安装
composer install
# jwt秘钥
php artisan jwt:secret
# 挨个执行
composer run post-autoload-dump
composer run post-root-package-install
composer run post-create-project-cmd
# .env中配置数据库信息,数据库可以通过数据库管理工具去进行创建数据库
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=lxcms_db
DB_USERNAME=root
DB_PASSWORD=root
# 配置完成之后执行数据库回滚
php artisan migrate:fresh --seed
```

## UI 规范

## 命名规范

- 常量(大写): VUE_APP_BASE_API
- 变量(驼峰): baseApi
- 方法(驼峰): getInfo()
- 组件(首字母大写): MapApi

## 代码提交规范

用于说明git commit的类别，只允许使用下面的标识

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