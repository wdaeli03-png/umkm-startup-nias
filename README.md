<!DOCTYPE html>
<html lang="id">
<head>
<meta charset="UTF-8">
<title>UMKM Startup Nias Barat - Level 3</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;500;700&display=swap" rel="stylesheet">

<style>
body{margin:0;font-family:Poppins;background:#f4f7fa}

/* LOGIN */
.login{
height:100vh;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
gap:10px;
}
.login input{
padding:10px;
width:220px;
border-radius:8px;
border:1px solid #ccc;
}
.login button{
padding:10px 20px;
border:none;
border-radius:8px;
background:#000;
color:#fff;
cursor:pointer;
}

/* HEADER */
.header{
background:linear-gradient(90deg,#ffd36b,#fff3d6);
padding:15px;
text-align:center;
font-weight:700;
}

/* MENU */
.menu{
display:flex;
gap:10px;
justify-content:center;
flex-wrap:wrap;
padding:10px;
}
.menu button{
padding:8px 12px;
border:none;
border-radius:8px;
cursor:pointer;
background:#222;
color:#fff;
}

/* CONTAINER */
.container{
padding:15px;
display:grid;
grid-template-columns:repeat(auto-fit,minmax(240px,1fr));
gap:15px;
}

/* CARD */
.card{
background:white;
border-radius:12px;
padding:15px;
box-shadow:0 2px 8px rgba(0,0,0,0.1);
}

.card img{
width:100%;
height:140px;
object-fit:cover;
border-radius:10px;
}

.btn{
margin-top:8px;
padding:7px 10px;
border:none;
border-radius:8px;
cursor:pointer;
color:white;
background:#2196f3;
}

.btn-green{background:#25d366}
.btn-red{background:#e53935}

/* PANEL */
.panel{
display:none;
padding:15px;
}

input,select{
padding:10px;
width:100%;
margin-bottom:8px;
border-radius:8px;
border:1px solid #ccc;
}

.box{
background:#fff;
padding:15px;
border-radius:10px;
margin-bottom:10px;
box-shadow:0 2px 8px rgba(0,0,0,0.1);
}
</style>
</head>

<body>

<!-- LOGIN -->
<div class="login" id="loginBox">
<h2>🚀 UMKM STARTUP LOGIN</h2>
<input id="user" placeholder="username">
<input id="pass" type="password" placeholder="password">
<button onclick="login()">LOGIN</button>
<p>demo: admin / 1234 | seller / 1234 | user / 1234</p>
</div>

<!-- APP -->
<div id="app" style="display:none">

<div class="header">🚀 UMKM STARTUP NIAS BARAT - LEVEL 3</div>

<div class="menu">
<button onclick="show('market')">Marketplace</button>
<button onclick="show('seller')">Seller Panel</button>
<button onclick="show('orders')">Orders</button>
<button onclick="logout()">Logout</button>
</div>

<!-- MARKET -->
<div class="panel" id="market">
<div class="container" id="list"></div>
</div>

<!-- SELLER -->
<div class="panel" id="seller">
<div class="box">
<h3>➕ Tambah Produk</h3>
<input id="name" placeholder="Nama Produk">
<input id="img" placeholder="Link Gambar">
<input id="price" placeholder="Harga">
<button class="btn" onclick="addProduct()">Tambah</button>
</div>
</div>

<!-- ORDERS -->
<div class="panel" id="orders">
<div id="orderList"></div>
</div>

</div>

<script>

let role="";
let products = JSON.parse(localStorage.getItem("products")) || [
{name:"Ikan Asap",img:"https://via.placeholder.com/300",price:25000},
{name:"Beras Lokal",img:"https://via.placeholder.com/300",price:120000}
];

let orders = JSON.parse(localStorage.getItem("orders")) || [];

function login(){
let u=document.getElementById("user").value;
let p=document.getElementById("pass").value;

if(p!=="1234"){alert("Password salah");return;}

role=u;

document.getElementById("loginBox").style.display="none";
document.getElementById("app").style.display="block";

show("market");
render();
}

function logout(){
location.reload();
}

function show(page){
document.querySelectorAll(".panel").forEach(p=>p.style.display="none");
document.getElementById(page).style.display="block";

if(page==="market") render();
if(page==="orders") renderOrders();
}

function render(){

let list=document.getElementById("list");
list.innerHTML="";

products.forEach((p,i)=>{
list.innerHTML+=`
<div class="card">
<img src="${p.img}">
<h3>${p.name}</h3>
<p>Rp ${p.price}</p>

<button class="btn" onclick="buy(${i})">Beli</button>
</div>
`;
});
}

function addProduct(){

if(role!=="seller" && role!=="admin"){
alert("Hanya seller yang bisa tambah produk");
return;
}

let n=document.getElementById("name").value;
let i=document.getElementById("img").value;
let p=document.getElementById("price").value;

products.push({name:n,img:i,price:p});
localStorage.setItem("products",JSON.stringify(products));

render();
alert("Produk ditambahkan!");
}

function buy(i){

let order={
product:products[i].name,
price:products[i].price,
time:new Date().toLocaleString()
};

orders.push(order);
localStorage.setItem("orders",JSON.stringify(orders));

alert("Order berhasil! (simulasi checkout)");
}

function renderOrders(){

let o=document.getElementById("orderList");
o.innerHTML="";

if(orders.length===0){
o.innerHTML="<p>Belum ada order</p>";
return;
}

orders.forEach((o1)=>{
o.innerHTML+=`
<div class="box">
<h4>${o1.product}</h4>
<p>Rp ${o1.price}</p>
<small>${o1.time}</small>
</div>
`;
});
}

</script>

</body>
</html>
