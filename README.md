<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>KRONOS LAB</title>

<style>
body{
margin:0;
font-family:Arial, Helvetica, sans-serif;
background:#0b1420;
color:white;
}

/* HEADER CON LOGO */
header{
background:#0f1c2e;
text-align:center;
padding:25px;
letter-spacing:3px;
border-bottom:2px solid #c8a96a;
}

.logo-container{
display:flex;
justify-content:center;
margin-bottom:10px;
}

.logo-circular{
width:140px;
height:140px;
border-radius:50%;
object-fit:cover;
border:3px solid #c8a96a;
box-shadow:0 0 20px rgba(200,169,106,0.7);
}

header h1{
margin:10px 0 0 0;
font-size:32px;
}

nav{
background:#0f1c2e;
display:flex;
justify-content:center;
gap:40px;
padding:15px;
border-bottom:2px solid #c8a96a;
}

nav a{
color:white;
text-decoration:none;
font-weight:bold;
}

nav a:hover{color:#c8a96a;}

section{
padding:60px 40px;
}

h2{
text-align:center;
margin-bottom:40px;
color:#c8a96a;
}

.productos{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(250px,1fr));
gap:30px;
}

.card{
background:#111f33;
border:1px solid #c8a96a;
border-radius:12px;
padding:20px;
text-align:center;
transition:0.3s;
}

.card:hover{transform:translateY(-6px);}

.card img{
width:200px;
height:200px;
object-fit:cover;
}

.precio{
color:#c8a96a;
margin:10px 0;
font-size:18px;
}

.card button{
background:#c8a96a;
color:black;
padding:10px;
border:none;
border-radius:6px;
cursor:pointer;
font-weight:bold;
}

.card button:hover{background:white;}

.modal{
display:none;
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
background:rgba(0,0,0,0.85);
justify-content:center;
align-items:center;
}

.modal-contenido{
background:#0f1c2e;
width:500px;
padding:25px;
border-radius:12px;
border:1px solid #c8a96a;
max-height:90vh;
overflow-y:auto;
}

.modal-contenido input,
.modal-contenido select{
width:100%;
padding:8px;
margin:6px 0;
border:none;
border-radius:6px;
}

.modal-contenido label{
font-size:14px;
color:#c8a96a;
}

.modal-contenido button{
width:100%;
padding:10px;
margin-top:10px;
background:#c8a96a;
border:none;
border-radius:6px;
cursor:pointer;
font-weight:bold;
}

.oculto{display:none;}
.confirmacion{text-align:center;}
</style>
</head>

<body>

<header>
<div class="logo-container">
<img src="imagenes/logotipo.png"Logo KRONOS LAB" class="logo-circular">
</div>
<h1>KRONOS LAB</h1>
</header>

<nav>
<a href="#anillos">Anillos</a>
<a href="#collares">Collares</a>
<a href="#pulseras">Pulseras</a>
<a href="#relojes">Relojes</a>
</nav>

<!-- FUNCION PARA CREAR PRODUCTOS -->
<script>
function productoHTML(nombre, precio, img){
return `
<div class="card">
<img src="${img}">
<h3>${nombre}</h3>
<div class="precio">$${precio.toLocaleString()} MXN</div>
<button onclick="comprar('${nombre}',${precio})">Comprar</button>
</div>
`;
}
</script>

<!-- ANILLOS -->
<section id="anillos">
<h2>Anillos</h2>
<div class="productos" id="anillosContainer"></div>
</section>

<!-- COLLARES -->
<section id="collares">
<h2>Collares</h2>
<div class="productos" id="collaresContainer"></div>
</section>

<!-- PULSERAS -->
<section id="pulseras">
<h2>Pulseras</h2>
<div class="productos" id="pulserasContainer"></div>
</section>

<!-- RELOJES -->
<section id="relojes">
<h2>Relojes</h2>
<div class="productos" id="relojesContainer"></div>
</section>

<!-- MODAL -->
<div class="modal" id="modalCompra">
<div class="modal-contenido">

<div id="formularioPago">
<h3 id="productoSeleccionado"></h3>

<h4>Datos Personales</h4>
<input type="text" id="nombreCompleto" placeholder="Nombre completo">
<input type="email" id="correo" placeholder="Correo electrónico">
<input type="text" id="telefono" placeholder="Teléfono">
<input type="text" id="direccion" placeholder="Dirección completa">

<label>Subir INE (obligatorio)</label>
<input type="file" id="ine" accept="image/*">

<label>Subir comprobante de domicilio</label>
<input type="file" id="comprobante" accept="image/*">

<h4>Método de Pago</h4>

<select id="metodoPago" onchange="mostrarMetodo()">
<option value="">Selecciona método</option>
<option value="tarjeta">Tarjeta</option>
<option value="transferencia">Transferencia</option>
<option value="efectivo">Efectivo</option>
<option value="boutique">Pago en Boutique</option>
</select>

<div id="tarjeta" class="oculto">
<input type="text" id="numeroTarjeta" placeholder="Número de tarjeta (16 dígitos)">
<input type="text" placeholder="Nombre del titular">
<input type="text" placeholder="MM/AA">
<input type="text" id="cvv" placeholder="CVV (3 dígitos)">
</div>

<div id="transferencia" class="oculto">
<p>CLABE: 012345678901234567</p>
<input type="text" id="clabe" placeholder="Ingresa tu CLABE (18 dígitos)">
</div>

<div id="efectivo" class="oculto">
<p>Pago contra entrega.</p>
</div>

<div id="boutique" class="oculto">
<select>
<option>Plaza San Luis</option>
<option>El Dorado</option>
<option>Citadella</option>
</select>
</div>

<button onclick="validarDatos()">Continuar</button>
<button onclick="cerrarModal()">Cancelar</button>
</div>

<div id="confirmacionCompra" class="oculto">
<h3>¿Estás seguro de realizar esta compra?</h3>
<button onclick="finalizarCompra()">Sí, confirmar</button>
<button onclick="cerrarModal()">Cancelar</button>
</div>

<div id="compraExitosa" class="confirmacion oculto">
<h3>¡Compra Exitosa!</h3>
<p>Tu pago está en proceso de verificación.</p>
<button onclick="cerrarModal()">Cerrar</button>
</div>

</div>
</div>

<script>
/* PRODUCTOS */
document.getElementById("anillosContainer").innerHTML =
productoHTML("Anillo Diamante Imperial",76000,"https://images.unsplash.com/photo-1603561591411-07134e71a2a9")+
productoHTML("Anillo Oro Blanco",55000,"imagenes/Anillo de diamantes.png")+
productoHTML("Anillo Esmeralda",68000,"imagenes/Anillo de esmeralda.png")+
productoHTML("Anillo Rubí",70000,"https://media-photos.depop.com/b1/349516628/2543944271_64b9fe2e29964622bb787ab48ebdcf95/P0.jpg");

document.getElementById("collaresContainer").innerHTML =
productoHTML("Collar Diamante",100000,"https://media.istockphoto.com/id/493610369/es/foto/collar-de-diamantes-toma-contra-un-fondo-negro.jpg?s=612x612&w=0&k=20&c=D6ldtpCEZgExQzWJ1o0CPKgulOcx_yQjDWYMF9ft1Bg=")+
productoHTML("Collar Perlas",64000,"https://i.pinimg.com/originals/9a/c3/14/9ac314028ae25edbd0b8d5a5fb7ec5d9.jpg")+
productoHTML("Collar Zafiro",89000,"https://i.etsystatic.com/8048631/r/il/54dfa9/3258106466/il_1080xN.3258106466_nw8u.jpg")+
productoHTML("Collar de Oro ",80000,"https://cdn.awsli.com.br/800x800/1207/1207329/produto/171479832/321f5881c4.jpg");

document.getElementById("pulserasContainer").innerHTML =
productoHTML("Pulsera Oro",20000,"imagenes/3.png")+
productoHTML("Pulsera Diamante",80500,"imagenes/2.png")+
productoHTML("Pulsera Plata",11000,"https://images.unsplash.com/photo-1588449668365-d15e397f6787")+
productoHTML("Pulsera Esmeralda",21000,"imagenes/1.png");

document.getElementById("relojesContainer").innerHTML =
productoHTML("Reloj Gold Prestige",65000,"imagenes/reloj4.png")+
productoHTML("Reloj Silver Luxury",44000,"imagenes/reloj3.png")+
productoHTML("Reloj Diamond Edition",89000,"imagenes/reloj2.png")+
productoHTML("Reloj Black Royal",40000,"imagenes/reloj1.png");

/* SISTEMA DE COMPRA */
let productoActual="",precioActual=0;

function comprar(nombre,precio){
productoActual=nombre;
precioActual=precio;
document.getElementById("productoSeleccionado").innerText=
nombre+" - $"+precio.toLocaleString()+" MXN";
document.getElementById("modalCompra").style.display="flex";
}

function cerrarModal(){
document.getElementById("modalCompra").style.display="none";
document.getElementById("formularioPago").classList.remove("oculto");
document.getElementById("confirmacionCompra").classList.add("oculto");
document.getElementById("compraExitosa").classList.add("oculto");
}

function mostrarMetodo(){
let metodo=document.getElementById("metodoPago").value;
["tarjeta","transferencia","efectivo","boutique"].forEach(id=>{
document.getElementById(id).classList.add("oculto");
});
if(metodo){document.getElementById(metodo).classList.remove("oculto");}
}

function validarDatos(){
if(
document.getElementById("nombreCompleto").value===""||
document.getElementById("correo").value===""||
document.getElementById("telefono").value===""||
document.getElementById("direccion").value===""){
alert("Completa todos los datos personales");return;}

if(document.getElementById("ine").files.length===0){
alert("Debes subir tu INE");return;}

let metodo=document.getElementById("metodoPago").value;
if(!metodo){alert("Selecciona método de pago");return;}

if(metodo==="tarjeta"){
if(document.getElementById("numeroTarjeta").value.length!=16||
document.getElementById("cvv").value.length!=3){
alert("Datos de tarjeta inválidos");return;}
}

if(metodo==="transferencia"){
if(document.getElementById("clabe").value.length!=18){
alert("CLABE inválida");return;}
}

document.getElementById("formularioPago").classList.add("oculto");
document.getElementById("confirmacionCompra").classList.remove("oculto");
}

function finalizarCompra(){
document.getElementById("confirmacionCompra").classList.add("oculto");
document.getElementById("compraExitosa").classList.remove("oculto");
}
</script>

</body>
</html>
