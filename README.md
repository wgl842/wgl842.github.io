<!DOCTYPE html>
<html>
<head>
<title>WGL Gaming Squad Fan Chat</title>

<style>
body {
    background: #111;
    color: white;
    font-family: Arial, sans-serif;
    text-align: center;
}

.container {
    width: 350px;
    margin: auto;
    margin-top: 40px;
    background: #222;
    padding: 20px;
    border-radius: 12px;
}

/* login/signup boxes */
input {
    width: 90%;
    padding: 10px;
    margin: 10px 0;
    border-radius: 6px;
    border: none;
}

button {
    width: 95%;
    padding: 12px;
    background: dodgerblue;
    color: white;
    border: none;
    border-radius: 6px;
    font-size: 17px;
    cursor: pointer;
}

#chatBox {
    width: 100%;
    height: 200px;
    background: black;
    overflow-y: auto;
    padding: 10px;
    margin-top: 10px;
    border-radius: 6px;
    text-align: left;
}

#chatBox p {
    color: #ddd;
}
</style>
</head>
<body>

<h1>WGL GAMING SQUAD FAN CHAT</h1>

<!-- LOGIN SCREEN -->
<div id="loginScreen" class="container">
    <h2>Login</h2>
    <input id="loginUser" placeholder="Username">
    <input id="loginPass" type="password" placeholder="Password">
    <button onclick="login()">Login</button>

    <p>Don't have an account?</p>
    <button onclick="showSignup()">Sign Up</button>
</div>

<!-- SIGN UP -->
<div id="signupScreen" class="container" style="display:none;">
    <h2>Create Account</h2>
    <input id="newUser" placeholder="Create username">
    <input id="newPass" type="password" placeholder="Create password">
    <button onclick="signup()">Sign Up</button>

    <p>Already have an account?</p>
    <button onclick="showLogin()">Back to Login</button>
</div>

<!-- CHAT -->
<div id="chatScreen" class="container" style="display:none;">
    <h2>Chat Room</h2>

    <div id="chatBox"></div>

    <input id="msgInput" placeholder="Type a message...">
    <button onclick="sendMsg()">Send</button>
</div>

<script>
// ------------------------
// USER ACCOUNT SYSTEM
// ------------------------

function signup() {
    let user = document.getElementById("newUser").value;
    let pass = document.getElementById("newPass").value;

    if (localStorage.getItem("acc_" + user)) {
        alert("Username already exists.");
        return;
    }

    // save account
    localStorage.setItem("acc_" + user, pass);
    alert("Account created!");

    showLogin();
}

function login() {
    let user = document.getElementById("loginUser").value;
    let pass = document.getElementById("loginPass").value;

    let realPass = localStorage.getItem("acc_" + user);

    if (realPass === pass) {
        localStorage.setItem("currentUser", user);
        showChat();
    } else {
        alert("Incorrect username or password.");
    }
}

// SWITCH SCREENS
function showSignup() {
    loginScreen.style.display = "none";
    signupScreen.style.display = "block";
}

function showLogin() {
    signupScreen.style.display = "none";
    loginScreen.style.display = "block";
}

function showChat() {
    loginScreen.style.display = "none";
    signupScreen.style.display = "none";
    chatScreen.style.display = "block";
}

// ------------------------
// CHAT SYSTEM
// ------------------------

function sendMsg() {
    let msg = document.getElementById("msgInput").value;
    let user = localStorage.getItem("currentUser");

    if (msg.trim() === "") return;

    let box = document.getElementById("chatBox");
    box.innerHTML += `<p><b>${user}:</b> ${msg}</p>`;
    msgInput.value = "";
    box.scrollTop = box.scrollHeight; // auto scroll
}
</script>

</body>
</html>
