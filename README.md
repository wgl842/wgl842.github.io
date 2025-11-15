<!DOCTYPE html>
<html>
<head>
  <title>WGL Gaming Squad Community Chat</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    body {
      background: #0d0d0d;
      color: white;
      font-family: Arial, sans-serif;
      text-align: center;
    }
    #usernameBox, #chatBox {
      max-width: 400px;
      margin: auto;
      padding: 20px;
      background: #1a1a1a;
      border-radius: 10px;
      display: none;
    }
    input, button {
      width: 90%;
      padding: 10px;
      margin-top: 10px;
      border-radius: 8px;
      border: none;
    }
    button {
      background: #4caf50;
      color: white;
      font-size: 16px;
      font-weight: bold;
    }
    #messages {
      height: 300px;
      overflow-y: auto;
      background: #111;
      padding: 10px;
      border-radius: 10px;
      text-align: left;
      margin-top: 10px;
    }
    .msg {
      padding: 8px;
      margin: 5px 0;
      background: #222;
      border-radius: 6px;
    }
  </style>
</head>
<body>
<h2>WGL Gaming Squad Community Chat</h2>
<!-- Step 1: Enter Username -->
<div id="usernameBox">
  <h3>Enter your Username</h3>
  <input id="usernameInput" placeholder="Username">
  <button onclick="setUsername()">Enter Chat</button>
</div>
<!-- Step 2: Chat -->
<div id="chatBox">
  <h3>Live Chat</h3>
  <div id="messages"></div>
  <input id="msgInput" placeholder="Type a message...">
  <button onclick="sendMsg()">Send</button>
</div>
<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js"></script>
<script>
// --------------------
// FIREBASE CONFIG
// --------------------
// You must replace the values below with your own Firebase project config
const firebaseConfig = {
  apiKey: "YOUR_KEY_HERE",
  authDomain: "YOUR_KEY_HERE",
  databaseURL: "YOUR_KEY_HERE",
  projectId: "YOUR_KEY_HERE",
  storageBucket: "YOUR_KEY_HERE",
  messagingSenderId: "YOUR_KEY_HERE",
  appId: "YOUR_KEY_HERE"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.database();
// --------------------
// Username Setup
// --------------------
let username = "";
document.getElementById("usernameBox").style.display = "block";
function setUsername() {
  const input = document.getElementById("usernameInput").value.trim();
  if(!input) return alert("Enter a username!");
  username = input;
  document.getElementById("usernameBox").style.display = "none";
  document.getElementById("chatBox").style.display = "block";
  startChat();
}
// --------------------
// Chat System
// --------------------
function startChat() {
  db.ref("messages").on("child_added", snap => {
    const msg = snap.val();
    const div = document.createElement("div");
    div.className = "msg";
    div.textContent = msg.sender + ": " + msg.text;
    document.getElementById("messages").appendChild(div);
    document.getElementById("messages").scrollTop = document.getElementById("messages").scrollHeight;
  });
}
function sendMsg() {
  const text = document.getElementById("msgInput").value.trim();
  if(!text) return;
  db.ref("messages").push({
    sender: username,
    text: text
  });
  document.getElementById("msgInput").value = "";
}
</script>
</body>
</html>
 
