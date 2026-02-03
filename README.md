<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>TBL Cafe | Digital Menu</title>

<style>
body{
margin:0;
font-family:Arial,Helvetica,sans-serif;
background:#000;
color:#fff;
}

header{
background:#000;
padding:15px;
display:flex;
justify-content:space-between;
align-items:center;
border-bottom:1px solid #222;
}

header h1{margin:0;font-size:22px;}
.admin-btn{
background:#fff;
color:#000;
padding:6px 14px;
border-radius:20px;
font-size:13px;
}

.hero{
text-align:center;
padding:40px 10px;
background:url("https://images.unsplash.com/photo-1517248135467-4c7edcad34c4") center/cover;
}

.hero h2{margin:0;font-size:28px;}
.hero p{opacity:0.9;}

section{padding:20px;}

.menu{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(160px,1fr));
gap:15px;
}

.card{
background:#111;
border-radius:15px;
padding:10px;
text-align:center;
}

.card img{
width:100%;
height:120px;
object-fit:cover;
border-radius:12px;
}

.card button{
width:100%;
padding:8px;
border:none;
border-radius:20px;
margin-top:8px;
background:#fff;
color:#000;
font-weight:bold;
}

.checkout{
background:#111;
border-radius:15px;
padding:15px;
margin-top:30px;
}

.checkout input{
width:100%;
padding:10px;
margin-bottom:10px;
border-radius:8px;
border:none;
}

.cart-item{
display:flex;
justify-content:space-between;
align-items:center;
margin:8px 0;
font-size:14px;
}

.qty button{
background:#fff;
border:none;
padding:2px 8px;
border-radius:50%;
margin:0 4px;
}

.total{
font-size:18px;
margin-top:10px;
}

.order-btn{
width:100%;
padding:12px;
border:none;
border-radius:25px;
background:#25D366;
color:#000;
font-size:16px;
font-weight:bold;
margin-top:15px;
}
</style>
</head>

<body>

<header>
<h1>TBL Cafe</h1>
<div class="admin-btn">Admin</div>
</header>

<div class="hero">
<h2>Order Online</h2>
<p>Fast • Easy • WhatsApp Order</p>
</div>

<section>
<h2>Menu</h2>

<div class="menu" id="menu"></div>

<div class="checkout">
<h3>Checkout</h3>

<input id="name" placeholder="Your Name">
<input id="phone" placeholder="Mobile Number">
<input id="address" placeholder="Address / Table No">

<div id="cart"></div>
<div class="total">Total: ₹<span id="total">0</span></div>

<button class="order-btn" onclick="placeOrder()">Place Order</button>
</div>
</section>

<script>
const items=[
{name:"Plain Maggie",price:40,img:"https://images.unsplash.com/photo-1604908554166-7c9a9a94a42b"},
{name:"Cheese Burger",price:80,img:"https://images.unsplash.com/photo-1550547660-d9450f859349"},
{name:"Cold Coffee",price:90,img:"https://images.unsplash.com/photo-1509042239860-f550ce710b93"},
{name:"Fries",price:70,img:"https://images.unsplash.com/photo-1541592106381-b31e9677c0e5"}
];

let cart=[];

function loadMenu(){
let html="";
items.forEach((i,idx)=>{
html+=`
<div class="card">
<img src="${i.img}">
<h4>${i.name}</h4>
<p>₹${i.price}</p>
<button onclick="addItem(${idx})">Add</button>
</div>`;
});
menu.innerHTML=html;
}

function addItem(i){
let found=cart.find(c=>c.name===items[i].name);
if(found){found.qty++;}
else{cart.push({...items[i],qty:1});}
renderCart();
}

function renderCart(){
let html="";
let total=0;
cart.forEach((c,idx)=>{
total+=c.price*c.qty;
html+=`
<div class="cart-item">
${c.name} (₹${c.price})
<div class="qty">
<button onclick="changeQty(${idx},-1)">-</button>
${c.qty}
<button onclick="changeQty(${idx},1)">+</button>
</div>
</div>`;
});
cartDiv.innerHTML=html;
totalSpan.innerText=total;
}

function changeQty(i,val){
cart[i].qty+=val;
if(cart[i].qty<=0)cart.splice(i,1);
renderCart();
}

function placeOrder(){
let name=nameInput.value;
let phone=phoneInput.value;
let address=addressInput.value;

if(!name||!phone||!address||cart.length==0){
alert("Please fill all details");
return;
}

let msg="🛎️ *New Order - TBL Cafe*%0A%0A";
msg+=`👤 ${name}%0A📞 ${phone}%0A🏠 ${address}%0A%0A`;
msg+="🧾 *Order Details*%0A";

cart.forEach(c=>{
msg+=`• ${c.name} x${c.qty} = ₹${c.price*c.qty}%0A`;
});

msg+=`%0A💰 Total: ₹${totalSpan.innerText}`;

window.open(`https://wa.me/7909325564?text=${msg}`,"_blank");
}

const menu=document.getElementById("menu");
const cartDiv=document.getElementById("cart");
const totalSpan=document.getElementById("total");
const nameInput=document.getElementById("name");
const phoneInput=document.getElementById("phone");
const addressInput=document.getElementById("address");

loadMenu();
</script>

</body>
</html>
