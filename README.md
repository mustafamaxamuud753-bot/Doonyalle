<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Chat App</title>
<style>
body{font-family:Arial;background:#ece5dd}
#login,#chat{max-width:400px;margin:40px auto}
#chat{display:none}
#messages{height:300px;overflow-y:auto;background:#fff;padding:10px}
.msg{margin:5px 0;padding:8px;border-radius:6px;background:#dcf8c6}
input,button{width:100%;padding:10px;margin-top:5px}
</style>
</head>
<body>

<div id="login">
  <h3>Sign Up / Login</h3>
  <input id="username" placeholder="Username">
  <button onclick="login()">Enter Chat</button>
</div>

<div id="chat">
  <h3>WhatsApp-like Chat</h3>
  <div id="messages"></div>
  <input id="message" placeholder="Type message">
  <button onclick="sendMsg()">Send</button>
</div>

<script src="/socket.io/socket.io.js"></script>
<script>
const socket = io();
let user="";

function login(){
  user=document.getElementById("username").value;
  if(user){
    socket.emit("join",user);
    document.getElementById("login").style.display="none";
    document.getElementById("chat").style.display="block";
  }
}

function sendMsg(){
  const msg=document.getElementById("message").value;
  socket.emit("chat",{user,msg});
  document.getElementById("message").value="";
}

socket.on("chat",data=>{
  const div=document.createElement("div");
  div.className="msg";
  div.innerText=data.user+": "+data.msg;
  document.getElementById("messages").appendChild(div);
});
</script>
</body>
</html><!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Chat App</title>
<style>
body{font-family:Arial;background:#ece5dd}
#login,#chat{max-width:400px;margin:40px auto}
#chat{display:none}
#messages{height:300px;overflow-y:auto;background:#fff;padding:10px}
.msg{margin:5px 0;padding:8px;border-radius:6px;background:#dcf8c6}
input,button{width:100%;padding:10px;margin-top:5px}
</style>
</head>
<body>

<div id="login">
  <h3>Sign Up / Login</h3>
  <input id="username" placeholder="Username">
  <button onclick="login()">Enter Chat</button>
</div>

<div id="chat">
  <h3>WhatsApp-like Chat</h3>
  <div id="messages"></div>
  <input id="message" placeholder="Type message">
  <button onclick="sendMsg()">Send</button>
</div>

<script src="/socket.io/socket.io.js"></script>
<script>
const socket = io();
let user="";

function login(){
  user=document.getElementById("username").value;
  if(user){
    socket.emit("join",user);
    document.getElementById("login").style.display="none";
    document.getElementById("chat").style.display="block";
  }
}

function sendMsg(){
  const msg=document.getElementById("message").value;
  socket.emit("chat",{user,msg});
  document.getElementById("message").value="";
}

socket.on("chat",data=>{
  const div=document.createElement("div");
  div.className="msg";
  div.innerText=data.user+": "+data.msg;
  document.getElementById("messages").appendChild(div);
});
</script>
</body>
</html>const express = require("express");
const app = express();
const http = require("http").createServer(app);
const io = require("socket.io")(http);

app.use(express.static(__dirname));

io.on("connection",socket=>{
  socket.on("join",user=>{
    socket.user=user;
  });

  socket.on("chat",data=>{
    io.emit("chat",data); // ilaa 1000 users
  });
});

http.listen(3000,()=>console.log("Server running on 3000"));npm init -y
npm install express socket.io
node server.js
