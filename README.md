<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>SafeWork - Industrial Safety</title>

<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">

<style>
*{margin:0;padding:0;box-sizing:border-box;font-family:'Poppins',sans-serif;}

body{background:#f4f7fb;}

/* HEADER */
header{
background:linear-gradient(135deg,#0b3c5d,#115173);
text-align:center;
padding:50px 20px;
color:white;
position:relative;
}

header img{
height:180px;
margin-bottom:15px;
}

header h1{
font-size:30px;
}

/* CONTROLES */
.controls{
display:flex;
justify-content:center;
gap:15px;
flex-wrap:wrap;
margin:30px 0;
}

.controls input, .controls select{
padding:10px 15px;
border-radius:8px;
border:1px solid #ccc;
}

/* BOTON CARRITO */
.cart-toggle{
position:fixed;
top:20px;
right:20px;
background:#f4b400;
border:none;
padding:12px 18px;
border-radius:8px;
cursor:pointer;
font-weight:600;
z-index:1001;
}

/* PRODUCTOS */
.container{
max-width:1300px;
margin:auto;
padding:20px;
}

.products{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:25px;
}

.card{
background:white;
border-radius:15px;
overflow:hidden;
box-shadow:0 8px 20px rgba(0,0,0,0.08);
transition:0.3s;
}

.card:hover{transform:translateY(-8px);}

.card img{
width:100%;
height:220px;
object-fit:cover;
}

.card-content{
padding:18px;
text-align:center;
}

.card h3{color:#0b3c5d;margin-bottom:10px;}

.price{
color:#f4b400;
font-weight:700;
margin-bottom:12px;
}

.add-btn{
background:#0b3c5d;
color:white;
border:none;
padding:8px 14px;
border-radius:6px;
cursor:pointer;
}

/* CARRITO */
.cart{
position:fixed;
top:0;
right:-400px;
width:350px;
height:100%;
background:white;
box-shadow:-5px 0 20px rgba(0,0,0,0.2);
padding:20px;
transition:0.4s;
overflow:auto;
z-index:1000;
}

.cart.active{right:0;}

.cart h2{margin-bottom:15px;}

.cart ul{list-style:none;}

.cart li{
display:flex;
justify-content:space-between;
margin-bottom:10px;
font-size:14px;
}

.remove-btn{
background:red;
color:white;
border:none;
padding:3px 7px;
border-radius:5px;
cursor:pointer;
}

.total{
margin-top:15px;
font-weight:700;
}

.close-cart{
background:#0b3c5d;
color:white;
border:none;
padding:5px 8px;
border-radius:5px;
cursor:pointer;
float:right;
}
</style>
</head>

<body>

<header>
<img src="logo.jpg" alt="Logo">
<h1 id="title">Industrial Safety Equipment</h1>
</header>

<button class="cart-toggle" onclick="toggleCart()">🛒 Cart</button>

<div class="controls">
<input type="text" id="search" placeholder="Search product..." onkeyup="filterProducts()">
<select id="category" onchange="filterProducts()">
<option value="all">All Categories</option>
<option value="protection">Protection</option>
<option value="clothing">Clothing</option>
<option value="emergency">Emergency</option>
</select>
<select onchange="changeLanguage(this.value)">
<option value="en">English</option>
<option value="es">Español</option>
</select>
</div>

<div class="container">
<div class="products" id="productList">

<!-- PRODUCT CARDS -->

<script>
const products = [
{name:"Safety Helmet", price:450, img:"Casco.jpg", category:"protection"},
{name:"Safety Gloves", price:120, img:"guantes.jpg", category:"protection"},
{name:"Safety Glasses", price:200, img:"lentes.jpg", category:"protection"},
{name:"Safety Vest", price:300, img:"chaleco.jpg", category:"clothing"},
{name:"Safety Boots", price:1200, img:"botas.jpg", category:"clothing"},
{name:"Face Mask", price:80, img:"mascarilla.jpg", category:"protection"},
{name:"Hearing Protection", price:150, img:"auditivos.jpg", category:"protection"},
{name:"Safety Harness", price:2500, img:"arnes.jpg", category:"protection"},
{name:"Back Support Belt", price:650, img:"fajas.jpg", category:"clothing"},
{name:"Face Shield", price:400, img:"caretas.jpg", category:"protection"},
{name:"Ear Plugs", price:60, img:"tapones.jpg", category:"protection"},
{name:"Knee Pads", price:350, img:"rodillera.jpg", category:"protection"},
{name:"Coveralls", price:900, img:"overoles.jpg", category:"clothing"},
{name:"Safety Signs", price:700, img:"señales.jpg", category:"emergency"},
{name:"Fire Extinguisher", price:3000, img:"extintores.jpg", category:"emergency"}
];

let cartItems=[];

function displayProducts(){
const list=document.getElementById("productList");
list.innerHTML="";
products.forEach(p=>{
list.innerHTML+=`
<div class="card" data-category="${p.category}">
<img src="${p.img}">
<div class="card-content">
<h3>${p.name}</h3>
<p class="price">RD$ ${p.price}</p>
<button class="add-btn" onclick="addToCart('${p.name}',${p.price})">Add</button>
</div></div>`;
});
}

function filterProducts(){
let search=document.getElementById("search").value.toLowerCase();
let category=document.getElementById("category").value;
let cards=document.querySelectorAll(".card");

cards.forEach(card=>{
let name=card.querySelector("h3").textContent.toLowerCase();
let matchSearch=name.includes(search);
let matchCategory=category==="all" || card.dataset.category===category;
card.style.display=(matchSearch && matchCategory)?"block":"none";
});
}

function toggleCart(){
document.querySelector(".cart").classList.toggle("active");
}

function addToCart(name,price){
cartItems.push({name,price});
renderCart();
}

function removeItem(index){
cartItems.splice(index,1);
renderCart();
}

function renderCart(){
const list=document.getElementById("cart-list");
list.innerHTML="";
let total=0;

cartItems.forEach((item,index)=>{
total+=item.price;
list.innerHTML+=`
<li>${item.name} - RD$ ${item.price}
<button class="remove-btn" onclick="removeItem(${index})">X</button></li>`;
});

document.getElementById("total").textContent=total;
}

function changeLanguage(lang){
if(lang==="es"){
document.getElementById("title").textContent="Equipos de Seguridad Industrial";
}
else{
document.getElementById("title").textContent="Industrial Safety Equipment";
}
}

displayProducts();
</script>

</div>
</div>

<!-- CART PANEL -->
<div class="cart">
<button class="close-cart" onclick="toggleCart()">Close</button>
<h2>Shopping Cart</h2>
<ul id="cart-list"></ul>
<p class="total">Total: RD$ <span id="total">0</span></p>
</div>

</body>
</html>
