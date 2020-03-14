## 天天生鲜项目

天天生鲜电商平台是学习后端常见的项目，我依照b站上的教程视频（基于django1.8），在Python3+Django2的平台上实践了该项目。

### 开发环境

Python: 3.7.6

Django: 2.1.7

Pycharm: 2019.2

OS: VMWare Ubuntu18.04

部署: 腾讯云私人服务器

### 技术栈

Redis: 作为django缓存和session的存储

Celery: 负责用户注册时异步发送邮件以及根据需求生成静态页面

FastDFS + Nginx: 存储网站静态文件，实现项目和资源分离，达到分布式效果

haystack + whoosh +jieba: 全文检索框架，通过修改底层haystack库使之对中文搜索更加友好

### 功能

- [x] 功能模块
    - [x] 用户模块
        - [x] 注册
        - [x] 登录
        - [x] 激活(celery)
        - [x] 退出
        - [x] 个人中心
        - [x] 地址管理
    - [x] 商品模块
        - [x] 首页(celery)
        - [x] 商品详情
        - [x] 商品列表
        - [x] 搜索功能(haystack+whoosh)
    - [x] 购物车模块(redis)
        - [x] 增加
        - [x] 删除
        - [x] 修改
        - [x] 查询
    - [x] 订单模块
        - [x] 确认订单页面
        - [x] 订单创建
        - [x] 请求支付(支付宝)
        - [x] 查询支付结果
        - [x] 评论

### 项目架构
![project_framework](https://github.com/LMFrank/Dailyfresh_Project/blob/master/images/project_framework.bmp)
### 项目部署（开发环境）
* 依赖库安装
```shell
pip install -r requirements.txt
```
* mysql数据库创建
```sql
CREATE DATABASE dailyfresh CHARSET=utf8;
```
* 启动celery
```shell
celery -A celery_tasks.tasks worker --loglevel=info
```
* 启动FastDFS服务, 启动nginx
```shell
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf start
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf start
/usr/local/nginx/sbin/nginx
```
* 迁移数据库
```shell
python manage.py makemigrations
python manage.py migrate
```
* 启动
```shell
python manage.py runserver
```

### 项目部署（生产环境）
我购买了腾讯云私人服务器，用作部署测试。可以自行使用docker打包所需环境，并且在服务器上下载，方便部署。

### 注意事项
1. python3使用pymysql替代python2.0的mysqldb
2. django1.0与django2.0有部分语法差异，诸如：url和path、re_path，使用外键时需加上on_delete等
3. 项目模板使用的是django自带的模板，可以替换为jinja2，体验优于原生模板。管理系统可以替换为xadmin
4. 该项目为前后端不分离，如果希望前后端分离开发，可以引入Django REST Framework

### 后记
该项目仅作为个人学习所用，欢迎大家指正该项目的不足之处。