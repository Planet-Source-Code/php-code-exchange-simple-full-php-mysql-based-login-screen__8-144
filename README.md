<div align="center">

## Simple, Full, PHP/MySQL based login screen


</div>

### Description

This is a simple PHP script that uses a MySQL table to store the login/password. I have included some extra code which is to be put into some sort of include file which is in turn to be included on every page you want to be protected (ie not viewable unless logged in via the login screen). Note: I know that this line: $msg = "<meta http-equiv=\"Refresh\" content=\"0;url=./mainpage.php\">"; is real lame but it's the only thing I could think of that is fairly compatable. The reason I did not use header() here is because using header() after setcookie() is not compatible with some versions of IE. by Graham
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[PHP Code Exchange](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/php-code-exchange.md)
**Level**          |Advanced
**User Rating**    |4.4 (22 globes from 5 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Databases](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases__8-5.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/php-code-exchange-simple-full-php-mysql-based-login-screen__8-144/archive/master.zip)





### Source Code

```
<?
if($username && $password) {
 mysql_connect() or die ("Whoops");
 $password = md5($password);
 $sql = "select * from login where username='$username'";
 $r = mysql_db_query("reg_users",$sql);
 if(!mysql_num_rows($r)) {
  header("Location: $SCRIPT_NAME");
 }
 $user = mysql_fetch_array($r);
 if($user["password"] == $password) {
  $password = serialize($password);
  setcookie("login_cookie","$username $password");
  $msg = "<meta http-equiv=\"Refresh\" content=\"0;url=./mainpage.php\">";
 }else{
   header("Location: $SCRIPT_NAME");
 }
}
if($msg) echo $msg;
?>
<html>
<title>Login to PHP Coders DB</title>
<body bgcolor="yellow" text="black">
<form method="post" action="<?echo $SCRIPT_NAME;?>">
<center><font size=+5><b>Welcome!</b></font></center>
<br>
<br>
<br>
<table cellspacing=0 cellpadding=0 width=320 align="center">
<tr><td>
Username:
</td><td>
<input name="username" type="text" width=10>
</td></tr>
<tr><td>
Password:
</td><td>
<input name="password" type="password" width=10>
</td></tr>
<tr><td colspan=2 align="center">
<input name="login" type="submit">
</td></tr>
</table>
</form>
</html>
<? /* Here is that include file code */ ?>
<?
if(!$login_cookie)
 header("Location: ./login.php");
if($phpcoders) {
 mysql_connect() or die ("Whoops");
 $user = explode(" ","$login_cookie");
 $sql = "select * from login where username='$user[0]'";
 $r = mysql_db_query("reg_users",$sql);
 if(!mysql_num_rows($r)) {
  header("Location: ./login.php");
  mysql_free_result($r);
 }
 $chkusr = mysql_fetch_array($r);
 mysql_free_result($r);
 if(unserialize($user[1]) != $chkusr[1])
  header("Location: ./login.php");
}
?>
```

