<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Блог Артёма PRO</title>

<style>
:root{
--bg:#0f172a;
--card:#1e293b;
--text:#e2e8f0;
--accent:#38bdf8;
}

body{
margin:0;
font-family:Arial;
background:var(--bg);
color:var(--text);
}

nav{
display:flex;
justify-content:center;
gap:10px;
padding:15px;
background:#020617;
border-bottom:1px solid #1e293b;
position:sticky;
top:0;
z-index:10;
}

nav button{
padding:10px 15px;
border:none;
border-radius:10px;
background:#1e293b;
color:white;
cursor:pointer;
transition:0.3s;
}

nav button:hover{
background:var(--accent);
}

nav button.active{
background:var(--accent);
}

.container{
max-width:800px;
margin:auto;
padding:20px;
}

.page{display:none}
.page.active{display:block}

.card{
background:var(--card);
padding:20px;
border-radius:15px;
margin-bottom:20px;
box-shadow:0 5px 20px rgba(0,0,0,0.5);
animation:fade 0.5s;
}

@keyframes fade{
from{opacity:0;transform:translateY(10px)}
to{opacity:1;transform:translateY(0)}
}

input,textarea{
width:100%;
padding:12px;
margin-top:10px;
border-radius:10px;
border:none;
background:#020617;
color:white;
}

button.save{
margin-top:10px;
padding:12px;
border:none;
border-radius:10px;
background:var(--accent);
color:black;
cursor:pointer;
font-weight:bold;
}

.result img{
width:100%;
border-radius:12px;
margin-top:10px;
}

h2{
margin-top:0;
}

.footer{
text-align:center;
padding:20px;
opacity:0.5;
}

</style>
</head>

<body>

<nav>
<button onclick="showPage('about', this)" class="active">Обо мне</button>
<button onclick="showPage('hobby', this)">Хобби</button>
<button onclick="showPage('ach', this)">Достижения</button>
<button onclick="clearAll()">🗑 Очистить</button>
</nav>

<div class="container">

<div id="about" class="page active">
<div class="card">
<h2>👤 Обо мне</h2>
<input id="name" placeholder="Имя и фамилия">
<textarea id="aboutText" placeholder="Расскажи о себе"></textarea>
<input type="file" id="aboutImg">
<button class="save" onclick="save('about')">Сохранить</button>
<div id="aboutResult" class="result"></div>
</div>
</div>

<div id="hobby" class="page">
<div class="card">
<h2>🎯 Моё хобби</h2>
<textarea id="hobbyText" placeholder="Расскажи о хобби"></textarea>
<input type="file" id="hobbyImg">
<button class="save" onclick="save('hobby')">Сохранить</button>
<div id="hobbyResult" class="result"></div>
</div>
</div>

<div id="ach" class="page">
<div class="card">
<h2>🏆 Мои достижения</h2>
<textarea id="achText" placeholder="Твои достижения"></textarea>
<input type="file" id="achImg">
<button class="save" onclick="save('ach')">Сохранить</button>
<div id="achResult" class="result"></div>
</div>
</div>

</div>

<div class="footer">
Сайт Артёма 🚀
</div>

<script>

function showPage(id, btn){
document.querySelectorAll('.page').forEach(p=>p.classList.remove('active'))
document.getElementById(id).classList.add('active')

document.querySelectorAll('nav button').forEach(b=>b.classList.remove('active'))
btn.classList.add('active')
}

function save(type){
let text='', file=null, name=''

if(type==='about'){
text=document.getElementById('aboutText').value
file=document.getElementById('aboutImg').files[0]
name=document.getElementById('name').value
}
if(type==='hobby'){
text=document.getElementById('hobbyText').value
file=document.getElementById('hobbyImg').files[0]
}
if(type==='ach'){
text=document.getElementById('achText').value
file=document.getElementById('achImg').files[0]
}

if(file){
let reader=new FileReader()
reader.onload=function(){
localStorage.setItem(type, JSON.stringify({text, img:reader.result, name}))
render()
}
reader.readAsDataURL(file)
}else{
localStorage.setItem(type, JSON.stringify({text, img:null, name}))
render()
}
}

function render(){
['about','hobby','ach'].forEach(type=>{
let data = JSON.parse(localStorage.getItem(type) || '{}')
let div = document.getElementById(type+'Result')
if(!div) return

let html = ''

if(type==='about' && data.name){
html += `<h3>${data.name}</h3>`
}

if(data.text){
html += `<p>${data.text}</p>`
}

if(data.img){
html += `<img src="${data.img}">`
}

if(!data.text && !data.img){
html += `<p style="opacity:0.5">Пока ничего нет...</p>`
}

div.innerHTML = html
})
}

function clearAll(){
if(confirm('Удалить все данные?')){
localStorage.clear()
render()
}
}

render()

</script>

</body>
</html>
