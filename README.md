
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


```python
%load_ext sql
%sql mysql+pymysql://root:12345678@127.0.0.1/tang_poetry
```




    'Connected: root@tang_poetry'




```python
%sql SHOW tables;
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    2 rows affected.





<table>
    <tr>
        <th>Tables_in_tang_poetry</th>
    </tr>
    <tr>
        <td>poetries</td>
    </tr>
    <tr>
        <td>poets</td>
    </tr>
</table>




```python
%sql DESCRIBE poetries
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    6 rows affected.





<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Null</th>
        <th>Key</th>
        <th>Default</th>
        <th>Extra</th>
    </tr>
    <tr>
        <td>id</td>
        <td>int(11)</td>
        <td>NO</td>
        <td>PRI</td>
        <td>None</td>
        <td>auto_increment</td>
    </tr>
    <tr>
        <td>poet_id</td>
        <td>int(11)</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>content</td>
        <td>text</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>title</td>
        <td>varchar(255)</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>created_at</td>
        <td>datetime</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>updated_at</td>
        <td>datetime</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
</table>




```python
%sql DESCRIBE poets
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    4 rows affected.





<table>
    <tr>
        <th>Field</th>
        <th>Type</th>
        <th>Null</th>
        <th>Key</th>
        <th>Default</th>
        <th>Extra</th>
    </tr>
    <tr>
        <td>id</td>
        <td>int(11)</td>
        <td>NO</td>
        <td>PRI</td>
        <td>None</td>
        <td>auto_increment</td>
    </tr>
    <tr>
        <td>name</td>
        <td>varchar(255)</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>created_at</td>
        <td>datetime</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
    <tr>
        <td>updated_at</td>
        <td>datetime</td>
        <td>YES</td>
        <td></td>
        <td>None</td>
        <td></td>
    </tr>
</table>



## 例子

### 查看唐朝写诗歌最多的人


```python
%%sql

SELECT
    poets.name,
    COUNT(poetries.id) AS poetries_count
FROM
    poetries
LEFT JOIN poets ON poets.id = poetries.poet_id
GROUP BY
    poets.id
ORDER BY
    poetries_count
DESC
LIMIT 10
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    10 rows affected.





<table>
    <tr>
        <th>name</th>
        <th>poetries_count</th>
    </tr>
    <tr>
        <td>白居易</td>
        <td>2643</td>
    </tr>
    <tr>
        <td>杜甫</td>
        <td>1158</td>
    </tr>
    <tr>
        <td>李白</td>
        <td>896</td>
    </tr>
    <tr>
        <td>佚名</td>
        <td>841</td>
    </tr>
    <tr>
        <td>齐己</td>
        <td>783</td>
    </tr>
    <tr>
        <td>刘禹锡</td>
        <td>703</td>
    </tr>
    <tr>
        <td>元稹</td>
        <td>593</td>
    </tr>
    <tr>
        <td>李商隐</td>
        <td>555</td>
    </tr>
    <tr>
        <td>贯休</td>
        <td>553</td>
    </tr>
    <tr>
        <td>韦应物</td>
        <td>551</td>
    </tr>
</table>



### 全唐诗库的总数量


```python
%%sql 

SELECT
    COUNT(*)
FROM
    poetries
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    1 rows affected.





<table>
    <tr>
        <th>COUNT(*)</th>
    </tr>
    <tr>
        <td>43030</td>
    </tr>
</table>




```python
%%sql

SELECT
    poets.name,
    poetries.title,
    poetries.content
FROM
    poetries
LEFT JOIN poets ON poets.id = poetries.poet_id
WHERE
    poets.name = '杨玉环'
```

     * mysql+pymysql://root:***@127.0.0.1/tang_poetry
    1 rows affected.





<table>
    <tr>
        <th>name</th>
        <th>title</th>
        <th>content</th>
    </tr>
    <tr>
        <td>杨玉环</td>
        <td>赠张云容舞</td>
        <td>罗袖动香香不已，红蕖袅袅秋烟里。轻云岭上乍摇风，嫩柳池边初拂水。</td>
    </tr>
</table>


