# 全唐诗数据库
爬取的全唐诗数据库

## 使用

1. 新建数据库

```bash
mysql> create database tang_poetry;
mysql> exit;
```

2. 导入数据

```bash
mysql -u root -p -h localhost tang_poetry < tang_poetry.sql
```

## 内容

有两张表，一张作者，一张古诗

## 截图

全唐诗中写诗歌最多的人，以及数量。

![全唐诗中写诗歌最多的人，以及数量。](http://i.imgur.com/Mcwl2TG.png)

杨贵妃的诗歌

![杨贵妃的诗歌](http://i.imgur.com/qgY0SKb.png)
