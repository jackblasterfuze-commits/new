<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Dana on Fire | Creative Projects</title>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.22.0/firebase-auth-compat.js"></script>

<style>
*{margin:0;padding:0;box-sizing:border-box;-webkit-tap-highlight-color:transparent;}
body{
font-family:-apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,Oxygen,Ubuntu,sans-serif;
background:#000;color:#fff;overflow-x:hidden;height:100vh;touch-action:pan-y;
}

/* Background */
.background{
position:fixed;top:0;left:0;width:100%;height:100%;
background-image:url('https://wallpapers.com/images/hd/red-black-background-h18bb4lw1nyraagm.jpg');
background-size:cover;background-position:center;
filter:blur(8px);transform:scale(1.1);z-index:-1;
}

/* ================= FIX APPLIED HERE ================= */
.user-profile{
position:fixed;
top:8px;          /* moved up */
right:15px;
z-index:1001;
display:flex;
align-items:center;
gap:10px;
}
/* ==================================================== */

.user-avatar{
width:40px;height:40px;border-radius:50%;
background:rgba(255,87,34,.2);
display:flex;align-items:center;justify-content:center;
font-size:18px;border:2px solid rgba(255,255,255,.2);
cursor:pointer;
}

.user-name{
background:rgba(0,0,0,.7);
padding:8px 15px;border-radius:20px;
font-size:14px;font-weight:600;
backdrop-filter:blur(10px);
border:1px solid rgba(255,255,255,.1);
}

.logout-btn{
background:rgba(255,87,34,.2);
color:#ff5722;border:none;
padding:8px 15px;border-radius:20px;
font-weight:600;cursor:pointer;
}

/* Header */
.header{
padding:70px 15px 10px;   /* SPACE RESERVED FOR PROFILE ICON */
text-align:center;
background:rgba(0,0,0,.7);
backdrop-filter:blur(10px);
border-bottom:1px solid rgba(255,255,255,.1);
z-index:10;
}

.logo{
display:flex;align-items:center;justify-content:center;
gap:12px;font-size:28px;font-weight:900;
}
.logo-icon{color:#ff5722;font-size:34px;animation:flicker 2s infinite alternate;}
@keyframes flicker{0%,100%{opacity:1}50%{opacity:.8}}

.container{
max-width:100%;
height:100vh;
display:flex;
flex-direction:column;
overflow:hidden;
}

.tab-content{
flex:1;overflow-y:auto;
padding:20px 15px 80px;
}

/* Dock */
.dock{
position:fixed;bottom:0;left:0;width:100%;
background:rgba(255,255,255,.1);
backdrop-filter:blur(20px);
border-top:1px solid rgba(255,255,255,.2);
padding:10px 15px;
display:flex;justify-content:space-around;
border-radius:20px 20px 0 0;
}

.dock-item{
display:flex;flex-direction:column;
align-items:center;
color:rgba(255,255,255,.7);
text-decoration:none;
padding:8px 12px;
border-radius:12px;
transition:.2s;
}
.dock-item.active,.dock-item:hover{
color:#fff;
background:rgba(255,255,255,.15);
transform:translateY(-5px);
}

.tab-pane{display:none;}
.tab-pane.active{display:block}

/* Responsive */
@media(max-width:480px){
.user-name{display:none;}
.header{padding-top:80px;}
}
</style>
</head>

<body>

<div class="user-profile" id="userProfile" style="display:none;">
<div class="user-avatar" id="userAvatar"><i class="fas fa-user"></i></div>
<div class="user-name" id="userName"></div>
<button class="logout-btn" id="logoutBtn">Logout</button>
</div>

<div class="background"></div>

<div class="container" id="mainContainer" style="display:none;">
<header class="header">
<div class="logo">
<i class="fas fa-fire-flame-curved logo-icon"></i>
<span>Dana on Fire</span>
</div>
</header>

<div class="tab-content">
<div class="tab-pane active" id="home">
<p style="text-align:center;opacity:.8">Home Content</p>
</div>

<div class="tab-pane" id="projects">
<p style="text-align:center;opacity:.8">Projects Content</p>
</div>

<div class="tab-pane" id="about">
<p style="text-align:center;opacity:.8">About Content</p>
</div>
</div>

<nav class="dock">
<a href="#" class="dock-item active" data-tab="home"><i class="fas fa-home"></i></a>
<a href="#" class="dock-item" data-tab="projects"><i class="fas fa-folder"></i></a>
<a href="#" class="dock-item" data-tab="about"><i class="fas fa-info-circle"></i></a>
</nav>
</div>

<script>
const firebaseConfig={
apiKey:"AIzaSyAOGZTqdkmcwo-wcpDOduMYua6eyjN6f6A",
authDomain:"dana-on-fire.firebaseapp.com",
projectId:"dana-on-fire",
storageBucket:"dana-on-fire.firebasestorage.app",
messagingSenderId:"136143488788",
appId:"1:136143488788:web:263ff21f7ca852d75e6401"
};

firebase.initializeApp(firebaseConfig);
const auth=firebase.auth();

auth.onAuthStateChanged(user=>{
if(user){
document.getElementById('userProfile').style.display='flex';
document.getElementById('mainContainer').style.display='flex';
document.getElementById('userName').textContent=user.displayName||user.email;
}else{
document.getElementById('userProfile').style.display='none';
}
});

document.querySelectorAll('.dock-item').forEach(i=>{
i.onclick=e=>{
e.preventDefault();
document.querySelectorAll('.dock-item').forEach(d=>d.classList.remove('active'));
i.classList.add('active');
document.querySelectorAll('.tab-pane').forEach(t=>t.classList.remove('active'));
document.getElementById(i.dataset.tab).classList.add('active');
};
});
</script>

</body>
</html>
