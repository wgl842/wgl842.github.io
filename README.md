<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WGL Chat Fan Zone</title>
<style>
body { margin:0; font-family:Arial,sans-serif; background:#111; color:white; }
header { background:#222; padding:15px; text-align:center; font-size:24px; }
.login-box, .chat-box, .owner-box { max-width:400px; margin:50px auto; padding:20px; background:#222; border-radius:10px; }
.login-box input, .owner-box input, .chat-input input { width:100%; padding:10px; margin:10px 0; border:none; border-radius:6px; font-size:16px; }
.btn { width:100%; padding:12px; background:#09f; border:none; border-radius:6px; font-size:18px; cursor:pointer; color:white; }
.btn:hover { background:#06c; }
.messages { height:300px; overflow-y:auto; background:#000; padding:10px; border-radius:6px; margin-bottom:10px; }
.msg { background:#333; padding:8px; border-radius:5px; margin-bottom:5px; }
#aiAssistant { margin-top:20px; padding:10px; background:#222; border-radius:6px; }
</style>
</head>
<body>

<header>WGL GAMING SQUAD FAN CHAT</header>

<div class="login-box" id="loginBox">
<h2>Fan Login</h2>
<p>Enter any passcode to join:</p>
<input type="text" id="fanPass" placeholder="Your passcode">
<button class="btn" onclick="fanLogin()">Enter</button>
<p id="loginMsg" style="color:red;"></p>
</div>

<div class="chat-box" id="chatBox" style="display:none;">
<h2>Chat Room</h2>
<div class="messages" id="messages"></div>
<div class="chat-input">
<input type="text" id="msgInput" placeholder="Type a message…" onkeydown="if(event.key==='Enter') sendMsg()">
<button class="btn" onclick="sendMsg()">Send</button>
</div>

<button class="btn" onclick="toggleVC()">Join WGL Chat VC</button>

<div id="aiAssistant">
<h3>AI Assistant 🤖</h3>
<input type="text" id="aiInput" placeholder="Ask me anything">
<button class="btn" onclick="askAI()">Send</button>
<div id="aiResponse"></div>
</div>
</div>

<div class="owner-box" id="ownerBox">
<h2>Owner Login</h2>
<input type="password" id="ownerPass" placeholder="Owner password">
<button class="btn" onclick="ownerLogin()">Enter Owner Panel</button>
</div>

<div class="chat-box" id="adminPanel" style="display:none;">
<h3>Owner Panel</h3>
<button class="btn" onclick="clearChat()">Clear Chat</button>
<h4>Banned Users</h4>
<ul id="bannedList"></ul>
<input type="text" id="banUser" placeholder="Enter passcode to ban">
<button class="btn" onclick="banUser()">Ban User</button>
</div>

<script>
let messagesArr = [];
let banned = [];
let fanPasscode = "";

function fanLogin() {
    const input = document.getElementById("fanPass").value;
    if(banned.includes(input)) {
        document.getElementById("loginMsg").textContent = "You are banned!";
        return;
    }
    fanPasscode = input;
    document.getElementById("loginBox").style.display="none";
    document.getElementById("chatBox").style.display="block";
}

function sendMsg() {
    const input = document.getElementById("msgInput").value.trim();
    if(input==="") return;
    if(banned.includes(fanPasscode)) return;
    messagesArr.push(fanPasscode+": "+input);
    updateMessages();
    document.getElementById("msgInput").value="";
}

function updateMessages() {
    const box = document.getElementById("messages");
    box.innerHTML = messagesArr.map(msg => "<div class='msg'>"+msg+"</div>").join("");
    box.scrollTop = box.scrollHeight;
}

function toggleVC() {
    alert("Voice Chat simulated (VC coming soon!)");
}

function askAI() {
    const input = document.getElementById("aiInput").value.toLowerCase();
    let reply = "Ask about WGL Gaming Squad!";
    if(input.includes("rules")) reply="Follow rules: Be respectful, no bullying, listen to owner, do not share personal info.";
    if(input.includes("chat")) reply="Use WGL Text Chat or WGL Chat VC after login!";
    if(input.includes("vc")) reply="Voice chat lets you talk with fans!";
    document.getElementById("aiResponse").innerText=reply;
}

function ownerLogin() {
    const pass = document.getElementById("ownerPass").value;
    if(pass==="sub") {
        document.getElementById("adminPanel").style.display="block";
        alert("Owner WGL logged in!");
    } else {
        alert("Wrong password!");
    }
}

function clearChat() {
    messagesArr = [];
    updateMessages();
}

function banUser() {
    const user = document.getElementById("banUser").value.trim();
    if(user==="") return;
    banned.push(user);
    const list = document.getElementById("bannedList");
    list.innerHTML = banned.map(u=>"<li>"+u+"</li>").join("");
    alert("User "+user+" banned!");
}
</script>

</body>
</html>
