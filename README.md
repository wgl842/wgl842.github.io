<!DOCTYPE html>
<html>
<head>
<title>WGL Gaming Squad Community Chat</title>
<meta name="viewport" content="width=device-width, initial-scale=1">
<style>
body {
  background: #0d0d0d;
  color: white;
  font-family: Arial;
  text-align: center;
}
#loginBox, #passcodeBox, #chatBox, #banList, #ownerTools, #subCheck {
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
<!-- Step 1: Passcode -->
<div id="passcodeBox">
  <h3>Enter Passcode</h3>
  <input id="passcodeInput" placeholder="Passcode">
  <button onclick="checkPasscode()">Enter</button>
</div>
<!-- Step 2: Screenshot check -->
<div id="subCheck">
  <h3>Upload Screenshot Showing You Subscribed</h3>
  <input type="file" id="subPic">
  <button onclick="continueToLogin()">Continue</button>
</div>
<!-- Step 3: Login -->
<div id="loginBox">
  <h3>Login</h3>
  <input id="email" placeholder="Email">
  <input id="password" placeholder="Password" type="password">
  <button onclick="login()">Login</button>
  <h4>Create Account</h4>
  <input id="newEmail" placeholder="New Email">
  <input id="newPassword" placeholder="New Password" type="password">
  <button onclick="signup()">Create Account</button>
</div>
<!-- Step 4: Chat -->
<div id="chatBox">
  <h3>Live Chat</h3>
  <div id="messages"></div>
  <input id="msgInput" placeholder="Type a message...">
  <button onclick="sendMsg()">Send</button>
  <br><br>
  <button onclick="alert('VC coming soon!')">WGL Chat VC</button>
  <button onclick="alert('AI Assistant coming soon!')">AI Assistant</button>
</div>
<!-- Owner Tools -->
<div id="ownerTools">
  <h3>Owner Controls</h3>
  <input id="banUserInput" placeholder="Email to ban">
  <button onclick="banUser()">Ban User</button>
  <h4>Banned Users</h4>
  <div id="banList"></div>
</div>
<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-database.js"></script>
<script>
// --------------------
// CONFIG (YOU MUST INSERT YOUR FIREBASE KEYS HERE)
// --------------------
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
// ----------------------
// 1. PASSCODE
// ----------------------
const PASSCODE = "1";
document.getElementById("passcodeBox").style.display = "block";
function checkPasscode() {
  if (document.getElementById("passcodeInput").value === PASSCODE) {
    document.getElementById("passcodeBox").style.display = "none";
    document.getElementById("subCheck").style.display = "block";
  } else {
    alert("Wrong passcode");
  }
}
// ----------------------
// 2. Screenshot Check
// ----------------------
function continueToLogin() {
  const file = document.getElementById("subPic").files[0];
  if (!file) return alert("Upload screenshot first!");
  document.getElementById("subCheck").style.display = "none";
  document.getElementById("loginBox").style.display = "block";
}
// ----------------------
// 3. Login / Signup
// ----------------------
function signup() {
  firebase.auth().createUserWithEmailAndPassword(
    newEmail.value,
    newPassword.value
  ).then(() => {
    alert("Account created! Now log in.");
  }).catch(e => alert(e.message));
}
function login() {
  firebase.auth().signInWithEmailAndPassword(
    email.value,
    password.value
  ).then(() => {
    startChat();
  }).catch(e => alert(e.message));
}
// ----------------------
// 4. Chat System
// ----------------------
function startChat() {
  document.getElementById("loginBox").style.display = "none";
  document.getElementById("chatBox").style.display = "block";
  const user = firebase.auth().currentUser;
  // Load messages
  db.ref("messages").on("child_added", snap => {
    const msg = snap.val();
    let div = document.createElement("div");
    div.className = "msg";
    div.textContent = msg.sender + ": " + msg.text;
    messages.appendChild(div);
    messages.scrollTop = messages.scrollHeight;
  });
  // Owner Tools
  if (user.email === "Owner") {
    ownerTools.style.display = "block";
    loadBans();
  }
}
function sendMsg() {
  const user = firebase.auth().currentUser;
  if (!msgInput.value.trim()) return;
  db.ref("messages").push({
    sender: user.email,
    text: msgInput.value
  });
  msgInput.value = "";
}
// ----------------------
// 5. Ban System
// ----------------------
function banUser() {
  const email = banUserInput.value;
  db.ref("bans/" + email.replace(".", "_")).set(true);
  loadBans();
}
function loadBans() {
  db.ref("bans").once("value", snap => {
    const list = snap.val() || {};
    banList.innerHTML = "";
    Object.keys(list).forEach(user => {
      let p = document.createElement("p");
      p.textContent = user.replace("_", ".");
      banList.appendChild(p);
    });
  });
}
</script>
</body>
</html>
 
