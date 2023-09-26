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
