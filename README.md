<!doctype html>
<html lang="fa">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>گردونه شانس 🎁</title>
<style>
:root { --size: 360px; }
body { font-family: "Tahoma", sans-serif; direction: rtl; display:flex; align-items:center; justify-content:center; min-height:100vh; background:#f4f7fb; margin:0; }
.container { text-align:center; }
h1 { color:#1b7a3a; margin-bottom:12px; }
.wheel-wrap { position:relative; width:var(--size); height:var(--size); margin:0 auto; }
.wheel { width:100%; height:100%; border-radius:50%; border:12px solid #ff5a7a; box-shadow:0 6px 18px rgba(0,0,0,0.08); transition: transform 5s cubic-bezier(.2,.9,.2,1); background: conic-gradient(#ffd77a 0deg 60deg,#fff0d9 60deg 120deg,#ffd77a 120deg 180deg,#fff0d9 180deg 240deg,#ffd77a 240deg 300deg,#fff0d9 300deg 360deg); display:flex; align-items:center; justify-content:center; position:relative; overflow:hidden;}
.segment-label { position:absolute; width:50%; left:50%; transform-origin:0 50%; text-align:left; padding-left:8px; font-weight:700; }
.pointer { position:absolute; left:50%; top:-18px; transform:translateX(-50%); width:0;height:0;border-left:18px solid transparent;border-right:18px solid transparent;border-bottom:26px solid #ff5a7a; z-index:2; }
.center { position:absolute; width:110px; height:110px; border-radius:50%; background:#ff4f6b; display:flex; align-items:center; justify-content:center; color:#fff; font-weight:800; cursor:pointer; z-index:2; box-shadow:0 6px 14px rgba(0,0,0,0.12);}
.result { margin-top:18px; font-size:18px; min-height:28px; color:#333; }
.coupon { margin-top:8px; padding:6px 10px; background:#fff; display:inline-block; border-radius:6px; box-shadow:0 2px 6px rgba(0,0,0,0.06); font-weight:700;}
@media (max-width:420px){ :root{--size:320px;} .center{width:92px;height:92px;} }
</style>
</head>
<body>
<div class="container">
  <h1>گردونه شانس 🎁</h1>
  <div class="wheel-wrap">
    <div class="pointer"></div>
    <div id="wheel" class="wheel" aria-hidden="true"></div>
    <div id="spinBtn" class="center">بزن</div>
  </div>
  <div class="result" id="resultText"></div>
</div>

<script>
// کد دعوت شما
const REF_CODE = "milli-ylhwk";

// جوایز
const prizes = [
  { label: "۵۰% تخفیف", type:"discount", value:50 },
  { label: "کارت هدیه", type:"gift", value:100000 },
  { label: "هیچ! دوباره شانس بزن", type:"none" },
  { label: "۲۰% تخفیف", type:"discount", value:20 },
  { label: "پک طلایی", type:"gift", value:"پک طلایی" },
  { label: "۱۰% تخفیف", type:"discount", value:10 }
];

const wheel = document.getElementById('wheel');
const spinBtn = document.getElementById('spinBtn');
const resultText = document.getElementById('resultText');

// رندر برچسب‌ها
function renderLabels(){
  const segCount = prizes.length;
  for(let i=0;i<segCount;i++){
    const angle = (360/segCount) * i;
    const div = document.createElement('div');
    div.className = 'segment-label';
    div.style.transform = `rotate(${angle}deg) translateY(-2px)`;
    div.style.top = '50%';
    div.style.height = '1.2em';
    div.innerHTML = `<span style="display:inline-block; transform:rotate(${90 - (360/segCount)/2}deg);">${prizes[i].label}</span>`;
    wheel.appendChild(div);
  }
}
renderLabels();

let spinning = false;
let currentRotation = 0;

function spinWheel(){
  if(spinning) return;
  spinning = true;
  resultText.textContent = "در حال گردش...";
  const chosenIndex = Math.floor(Math.random() * prizes.length);
  const segments = prizes.length;
  const segmentAngle = 360 / segments;
  const rounds = 6 + Math.floor(Math.random()*4);
  const targetAngle = rounds*360 + (360 - (chosenIndex * segmentAngle) - segmentAngle/2);
  const extraJitter = (Math.random()-0.5) * (segmentAngle*0.6);
  const finalRotation = targetAngle + extraJitter;

  wheel.style.transition = 'transform 5s cubic-bezier(.2,.9,.2,1)';
  currentRotation = (currentRotation + finalRotation) % 3600;
  wheel.style.transform = `rotate(${currentRotation}deg)`;

  setTimeout(()=>{
    spinning = false;
    showResult(prizes[chosenIndex]);
  }, 5100);
}

function showResult(prize){
  if(prize.type === 'discount' || prize.type === 'gift'){
    const code = generateCouponCode() + "-" + REF_CODE;
    resultText.innerHTML = `🎉 ${prize.label} دریافت کردی! <span class="coupon">${code}</span>`;
  } else {
    resultText.innerHTML = `😅 این بار برنده نشدی — دوباره تلاش کن!`;
  }
}

function generateCouponCode(){
  const parts = [];
  const chars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
  for(let i=0;i<6;i++) parts.push(chars.charAt(Math.floor(Math.random()*chars.length)));
  return parts.join('');
}

spinBtn.addEventListener('click', spinWheel);
</script>
</body>
</html># wheel-lucky-web
