<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Happy Birthday Shahid 🎂</title>

<!-- Tailwind CSS -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Fonts -->
<link href="https://fonts.googleapis.com/css2?family=Playfair+Display:wght@500;700&display=swap" rel="stylesheet">
<link href="https://api.fontshare.com/v2/css?f[]=satoshi@400,500,700&display=swap" rel="stylesheet">

<!-- Libraries -->
<script src="https://cdn.jsdelivr.net/npm/three@0.160.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/gsap@3.12.5/dist/gsap.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.9.3/dist/confetti.browser.min.js"></script>

<style>
body{
  margin:0;
  background:#0c0a09;
  font-family:'Satoshi',sans-serif;
  color:#e5e5e5;
  overflow:hidden;
}

/* Aurora background */
.aurora{
  position:fixed;
  inset:0;
  background:
    radial-gradient(circle at 20% 30%, hsla(333,70%,55%,.4), transparent 40%),
    radial-gradient(circle at 80% 20%, hsla(282,82%,54%,.3), transparent 45%),
    radial-gradient(circle at 50% 80%, hsla(210,89%,60%,.3), transparent 45%),
    radial-gradient(circle at 70% 70%, hsla(35,90%,60%,.3), transparent 50%);
  animation: aurora 20s ease-in-out infinite alternate;
  z-index:-3;
}
@keyframes aurora{
  0%{filter:blur(80px);transform:scale(1);}
  100%{filter:blur(120px);transform:scale(1.2);}
}

/* Glass card */
.card{
  background:rgba(15,15,15,.6);
  backdrop-filter:blur(25px);
  border:1px solid rgba(255,255,255,.12);
  box-shadow:0 25px 60px rgba(0,0,0,.7);
}

/* Gradient heading */
.grad-text{
  background:linear-gradient(90deg,#ffd1dc,#ffffff);
  -webkit-background-clip:text;
  color:transparent;
  font-family:'Playfair Display',serif;
}

/* Buttons */
.btn{
  background:linear-gradient(135deg,#ff4d6d,#ff1f4b);
  box-shadow:0 0 30px rgba(255,77,109,.7);
  transition:.3s;
}
.btn:hover{
  transform:translateY(-3px) scale(1.05);
  box-shadow:0 0 45px rgba(255,77,109,1);
}
.btn:active{
  transform:scale(.96);
}

/* Progress bar */
#progressTrack{
  position:fixed;
  top:16px;
  left:50%;
  transform:translateX(-50%);
  width:60%;
  height:6px;
  background:rgba(255,255,255,.15);
  backdrop-filter:blur(10px);
  border-radius:10px;
  z-index:50;
}
#progressBar{
  height:100%;
  width:0%;
  background:linear-gradient(90deg,#ff4d6d,#ff1f4b);
  border-radius:10px;
  transition:width .6s ease;
}

/* Polaroid */
.polaroid{
  background:white;
  padding:10px 10px 30px;
  transform:rotate(-5deg);
  transition:.4s;
}
.polaroid:hover{
  transform:rotate(0deg) scale(1.05);
}
.polaroid img{
  width:100%;
  display:block;
}
</style>
</head>

<body>
<div class="aurora"></div>
<div id="three-bg" class="fixed inset-0 -z-10"></div>

<div id="progressTrack"><div id="progressBar"></div></div>

<div class="min-h-screen flex items-center justify-center px-4">
  <div id="steps" class="w-full max-w-xl text-center space-y-6"></div>
</div>

<script>
/* -------- THREE.JS FLOATING HEARTS -------- */
const scene=new THREE.Scene();
const camera=new THREE.PerspectiveCamera(75,innerWidth/innerHeight,0.1,1000);
camera.position.z=8;

const renderer=new THREE.WebGLRenderer({alpha:true});
renderer.setSize(innerWidth,innerHeight);
document.getElementById("three-bg").appendChild(renderer.domElement);

const hearts=[];
const shape=new THREE.Shape();
shape.moveTo(0,0);
shape.bezierCurveTo(0,0,-.5,.5,0,1);
shape.bezierCurveTo(.5,1,1,.5,0,0);

const geometry=new THREE.ExtrudeGeometry(shape,{depth:.3,bevelEnabled:true});

for(let i=0;i<23;i++){
  const material=new THREE.MeshStandardMaterial({
    color:new THREE.Color(`hsl(${340+Math.random()*20},80%,60%)`),
    metalness:.4,
    roughness:.3,
    transparent:true
  });
  const mesh=new THREE.Mesh(geometry,material);
  mesh.position.set((Math.random()-0.5)*10,(Math.random()-0.5)*6,(Math.random()-0.5)*5);
  mesh.rotation.set(Math.random(),Math.random(),Math.random());
  scene.add(mesh);
  hearts.push(mesh);
}

scene.add(new THREE.AmbientLight(0xffffff,.8));
const light=new THREE.PointLight(0xffffff,1);
light.position.set(5,5,5);
scene.add(light);

function animate(){
  requestAnimationFrame(animate);
  hearts.forEach((h,i)=>{
    h.rotation.y+=0.002;
    h.position.y+=Math.sin(Date.now()*0.001+i)*0.0005;
  });
  renderer.render(scene,camera);
}
animate();

/* -------- STEPS CONTENT (BRO STYLE) -------- */
const steps=[
`<div class="card p-8 rounded-3xl space-y-6">
<div class="text-6xl animate-pulse">❤️</div>
<h1 class="grad-text text-4xl">Yo Shahid!</h1>
<p>Bro, I didn’t just wish you like everyone else.  
I built this whole thing just for you.  
Real ones deserve real effort.</p>
<button class="btn px-8 py-3 rounded-full text-white font-bold" onclick="next()">Let’s Go 🔥</button>
</div>`,

`<div class="card p-8 rounded-3xl space-y-6">
<div class="text-6xl">🎉</div>
<h1 class="grad-text text-4xl">Happy Birthday, Legend</h1>
<p>Another year older, another year stronger.  
Through laughs, struggles, and madness — you’ve always been solid.  
Proud to call you my brother.</p>
<button class="btn px-8 py-3 rounded-full text-white font-bold" onclick="next()">Keep Going →</button>
</div>`,

`<div class="card p-8 rounded-3xl space-y-6">
<h1 class="grad-text text-3xl">Why You’re That Guy 💪</h1>
<div class="grid grid-cols-2 gap-4 text-left">
<div class="col-span-2 card p-4 rounded-xl">
<h3>✨ Loyalty</h3>
<p>You stand by your people no matter what.  
Rare quality. Real respect.</p>
</div>
<div class="card p-4 rounded-xl">
<h3>😎 That Attitude</h3>
<p>Calm. Confident. Unshakeable.</p>
</div>
<div class="col-span-2 card p-4 rounded-xl">
<h3>🔥 Brother Energy</h3>
<p>No fake vibes. Just real moments and memories.</p>
</div>
</div>
<button class="btn px-8 py-3 rounded-full text-white font-bold" onclick="next()">You Remember This 😏</button>
</div>`,

`<div class="card p-8 rounded-3xl space-y-6">
<h1 class="grad-text text-3xl">That One Memory…</h1>
<div class="polaroid mx-auto w-64">
<img src="https://i.ibb.co/WWQ7qDf5/image.jpg" alt="Shahid Memory">
<p class="text-black text-sm mt-2">OG moment.</p>
</div>
<p>This wasn’t just a photo.  
This was brotherhood.  
Moments we’ll laugh about forever.</p>
<button class="btn px-8 py-3 rounded-full text-white font-bold" onclick="next()">One Last Thing 👊</button>
</div>`,

`<div class="card p-8 rounded-3xl space-y-6">
<div class="text-6xl">🎂</div>
<h1 class="grad-text text-4xl">My Wish For You, Bro</h1>
<p>May this year bring you success, peace, and big wins.  
May you level up in life, money, and happiness.</p>
<p id="finalText" class="opacity-0 text-2xl grad-text"></p>
<button id="celebrateBtn" class="btn px-8 py-3 rounded-full text-white font-bold" onclick="celebrate()">Celebrate Like a King 👑</button>
</div>`
];

let i=0;
const container=document.getElementById("steps");

function render(){
  container.innerHTML=steps[i];
  gsap.from(container.children[0],{y:40,opacity:0,duration:.8});
  document.getElementById("progressBar").style.width=(i/4*100)+"%";
}
function next(){i++;render();}
render();

/* -------- GRAND FINALE -------- */
function celebrate(){
  document.getElementById("celebrateBtn").style.display="none";
  const t=document.getElementById("finalText");
  t.innerText="Happy Birthday, My Brother Shahid 💙🔥";
  gsap.to(t,{opacity:1,y:-10,duration:1});

  const end=Date.now()+5000;
  (function frame(){
    confetti({particleCount:6,spread:70,origin:{x:0,y:1}});
    confetti({particleCount:6,spread:70,origin:{x:1,y:1}});
    if(Date.now()<end) requestAnimationFrame(frame);
  })();

  hearts.forEach(h=>{
    gsap.to(h.position,{y:10,z:10,duration:3});
    gsap.to(h.material,{opacity:0,duration:3});
  });
}
</script>
</body>
</html>
