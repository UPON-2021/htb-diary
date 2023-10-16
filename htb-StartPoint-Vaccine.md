# Vaccine

### 靶机信息:

* 10.129.95.174 
* 本机ip 10.10.16.8

```shell
nmap -sV 10.129.95.174  
Starting Nmap 7.94 ( https://nmap.org ) at 2023-10-04 15:07 CST
Nmap scan report for 10.129.95.174
Host is up (0.31s latency).
Not shown: 997 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0p1 Ubuntu 6ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 12.79 seconds

```



### Notes

直接Curl得到

```html
<body class="align">

  <div class="grid">

    <form action="" method="POST" class="form login">

      <div class="form__field">
        <label for="login__username"><svg class="icon"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#user"></use></svg><span class="hidden">Username</span></label>
        <input id="login__username" type="text" name="username" class="form__input" placeholder="Username" required>
      </div>

      <div class="form__field">
        <label for="login__password"><svg class="icon"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#lock"></use></svg><span class="hidden">Password</span></label>
        <input id="login__password" type="password" name="password" class="form__input" placeholder="Password" required>
      </div>

      <div class="form__field">
        <input type="submit" value="Sign In">
      </div>

    </form>


  </div>

```

 sql注入，无结果，ftp下载得到一个`backup.zip`，但是压缩包加密的

![image-20231004152337277](.\images\htb-StartPoint-Vaccine\image-20231004152337277.png)

算一下crc32

![image-20231004152545265](.\images\htb-StartPoint-Vaccine\image-20231004152545265.png)



应该可以明文攻击

![image-20231004160625548](.\images\htb-StartPoint-Vaccine\image-20231004160625548.png)

emm 明文爆破半天啥都没有，然后hashcat起爆

![image-20231004185500137](.\images\htb-StartPoint-Vaccine\image-20231004185500137.png)

~~俩小时还真给爆出来了~~

自带的字典不够大，github上找个字典就给爆了

![image-20231004160735772](.\images\htb-StartPoint-Vaccine\image-20231004160735772.png)

压缩包密码`741852963`

```php
<!DOCTYPE html>
<?php
session_start();
  if(isset($_POST['username']) && isset($_POST['password'])) {
    if($_POST['username'] === 'admin' && md5($_POST['password']) === "2cb42f8734ea607eefed3b70af13bbd3") {
      $_SESSION['login'] = "true";
      header("Location: dashboard.php");
    }
  }
?>
<html lang="en" >
<head>
  <meta charset="UTF-8">
  <title>MegaCorp Login</title>
  <link href="https://fonts.googleapis.com/css?family=Open+Sans:400,700" rel="stylesheet"><link rel="stylesheet" href="./style.css">

</head>
  <h1 align=center>MegaCorp Login</h1>
<body>
<!-- partial:index.partial.html -->
<body class="align">

  <div class="grid">

    <form action="" method="POST" class="form login">

      <div class="form__field">
        <label for="login__username"><svg class="icon"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#user"></use></svg><span class="hidden">Username</span></label>
        <input id="login__username" type="text" name="username" class="form__input" placeholder="Username" required>
      </div>

      <div class="form__field">
        <label for="login__password"><svg class="icon"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#lock"></use></svg><span class="hidden">Password</span></label>
        <input id="login__password" type="password" name="password" class="form__input" placeholder="Password" required>
      </div>

      <div class="form__field">
        <input type="submit" value="Sign In">
      </div>

    </form>


  </div>

  <svg xmlns="http://www.w3.org/2000/svg" class="icons"><symbol id="arrow-right" viewBox="0 0 1792 1792"><path d="M1600 960q0 54-37 91l-651 651q-39 37-91 37-51 0-90-37l-75-75q-38-38-38-91t38-91l293-293H245q-52 0-84.5-37.5T128 1024V896q0-53 32.5-90.5T245 768h704L656 474q-38-36-38-90t38-90l75-75q38-38 90-38 53 0 91 38l651 651q37 35 37 90z"/></symbol><symbol id="lock" viewBox="0 0 1792 1792"><path d="M640 768h512V576q0-106-75-181t-181-75-181 75-75 181v192zm832 96v576q0 40-28 68t-68 28H416q-40 0-68-28t-28-68V864q0-40 28-68t68-28h32V576q0-184 132-316t316-132 316 132 132 316v192h32q40 0 68 28t28 68z"/></symbol><symbol id="user" viewBox="0 0 1792 1792"><path d="M1600 1405q0 120-73 189.5t-194 69.5H459q-121 0-194-69.5T192 1405q0-53 3.5-103.5t14-109T236 1084t43-97.5 62-81 85.5-53.5T538 832q9 0 42 21.5t74.5 48 108 48T896 971t133.5-21.5 108-48 74.5-48 42-21.5q61 0 111.5 20t85.5 53.5 62 81 43 97.5 26.5 108.5 14 109 3.5 103.5zm-320-893q0 159-112.5 271.5T896 896 624.5 783.5 512 512t112.5-271.5T896 128t271.5 112.5T1280 512z"/></symbol></svg>

</body>
<!-- partial -->
  
</body>
</html>

```



哈希是 `2cb42f8734ea607eefed3b70af13bbd3`

![image-20231004161441302](.\images\htb-StartPoint-Vaccine\image-20231004161441302.png)

非常好密码



进去后有个搜索窗口，可以sql注入

sqlmap启动~

![image-20231004162402241](.\images\htb-StartPoint-Vaccine\image-20231004162402241.png)

`/etc/passwd`

```
s:x:111:117:PostgreSQL administrator,,,:/var/lib/postgresql
```

dashbord.php



```php
 if($_SESSION['login'] !== "true") {
          header("Location: index.php");
          die();
        }
        try {
          $conn = pg_connect("host=localhost port=5432 dbname=carsdb user=postgres password=P@s5w0rd!");
        }

        catch ( exception $e ) {

```



ssh 连上去了

![image-20231004170239195](.\images\htb-StartPoint-Vaccine\image-20231004170239195.png)

看看权限

![image-20231004170521500](.\images\htb-StartPoint-Vaccine\image-20231004170521500.png)

vi 进去后`:/bin/sh` 撅了

# Unified



* 靶机IP 10.129.96.149
* 本机IP  10.10.16.8

nmap后 8443进去康康

![image-20231004193145244](.\images\htb-StartPoint-Vaccine\image-20231004193145244.png)

msf 一通操作之后直接打进去

![image-20231005151246297](D:\htb日记\images\htb-StartPoint-Vaccine\image-20231005151246297.png)

数据库里直接找到密码

NotACrackablePassword4U2022



etc/passwd没找到啥内容，直接root登录





# Marku

22 80 443 

![image-20231005153957599](.\images\htb-StartPoint-Vaccine\image-20231005153957599.png)



admin password进去了

prcess.php可以直接打xxe

就能读私钥了