[index (2).html](https://github.com/user-attachments/files/35173731/index.2.html)
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Agendita</title>
<link rel="apple-touch-icon" href="icon-180.png">
<link rel="icon" type="image/png" href="icon-192.png">
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#1b2a4a">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-title" content="Agendita">
<meta name="apple-mobile-web-app-status-bar-style" content="default">

<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:opsz,wght@12..96,400;12..96,600;12..96,800&display=swap" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
<style>
:root{--bg:#faf6fb;--card:#fff;--ink:#2a1a3a;--mut:#75677f;--line:#e8dcee;--acc:#d63a78;--accInk:#fff;--soft:#f6d9e6;--mint:#c4ead9;--mintInk:#14533a}
@media (prefers-color-scheme:dark){:root{--bg:#1a1124;--card:#261a33;--ink:#f3eaf8;--mut:#a795b4;--line:#3b2b4b;--acc:#ff6fa3;--accInk:#2a1a3a;--soft:#4a2a3f;--mint:#1f4b3a;--mintInk:#b9efd6}}
*{box-sizing:border-box}
body{margin:0;background:var(--bg);color:var(--ink);font-family:'Bricolage Grotesque',system-ui,sans-serif;font-size:16px;line-height:1.45}
.wrap{max-width:720px;margin:0 auto;padding:16px}
header{display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:14px}
h1{font-size:22px;font-weight:800;margin:0}h1 small{display:block;font-size:13px;font-weight:400;color:var(--mut)}
h2{font-size:20px;margin:18px 0 10px}
.card{background:var(--card);border:1px solid var(--line);border-radius:14px;padding:14px}
.opt{display:flex;justify-content:space-between;align-items:center;width:100%;text-align:left;background:var(--card);color:var(--ink);border:1px solid var(--line);border-radius:12px;padding:14px;margin-bottom:8px;font:inherit;cursor:pointer}
.opt:hover,.opt:focus-visible{border-color:var(--acc);outline:none}.opt span{color:var(--mut);font-size:14px}
.chips{display:flex;flex-wrap:wrap;gap:8px}
.chip{border:1px solid var(--line);background:var(--card);color:var(--ink);border-radius:10px;padding:9px 13px;font:inherit;cursor:pointer;min-width:64px;text-align:center}
.chip:hover,.chip:focus-visible{border-color:var(--acc);outline:none}.chip:disabled{opacity:.35;cursor:not-allowed}
.chip.on{border-color:var(--acc);background:var(--soft)}
.day{min-width:62px;line-height:1.2}.day b{display:block;font-size:18px}
.btn{border:0;background:var(--acc);color:var(--accInk);font:inherit;font-weight:600;padding:12px 18px;border-radius:12px;cursor:pointer}
.btn.ghost{background:var(--soft);color:var(--ink)}.btn.sm{padding:7px 11px;font-size:13px;border-radius:9px}
.btn:disabled{opacity:.5}
input{width:100%;font:inherit;padding:12px;border:1px solid var(--line);border-radius:10px;background:var(--card);color:var(--ink);margin-bottom:10px}
label{font-size:14px;color:var(--mut);display:block;margin-bottom:4px}
.sum{background:var(--soft);border-radius:12px;padding:12px;margin-bottom:12px;font-size:15px}
.back{background:none;border:0;color:var(--mut);font:inherit;cursor:pointer;padding:0;margin-bottom:4px}
.ok{background:var(--mint);color:var(--mintInk);border-radius:14px;padding:18px}.ok h2{margin-top:0}
.cols{display:grid;gap:12px;grid-template-columns:repeat(auto-fit,minmax(200px,1fr))}
.col h3{margin:0 0 8px;font-size:16px}
.ap{border-left:4px solid var(--acc);background:var(--card);border-radius:8px;padding:10px;margin-bottom:8px;font-size:14px}
.ap .r{display:flex;gap:6px;margin-top:8px;flex-wrap:wrap}
.empty{color:var(--mut);font-size:14px;padding:8px 0}.err{color:var(--acc);font-size:14px}
.stats{display:flex;gap:10px;margin:12px 0;align-items:stretch}.stats .card{flex:1}.stats b{font-size:22px;display:block}.stats span{font-size:13px;color:var(--mut)}
.scroll{overflow-x:auto;padding-bottom:4px}
.foot{font-size:13px;color:var(--mut);margin-top:26px;text-align:center}.foot a{color:var(--mut)}
</style>
</head>
<body>
<div class="wrap">
<header><h1 id="ttl">Agendita<small id="sub"></small></h1><button class="btn ghost sm" id="out" style="display:none" onclick="logout()">Cerrar sesión</button></header>
<div id="app"><p class="empty">Cargando…</p></div>
<p class="foot" id="foot"></p>
</div>
<script>
const sb=supabase.createClient('https://mljkqzhihuisqojncjau.supabase.co','sb_publishable_5PJBrEVPioYb6GtBzWmY8g_JRNj-wKt');
const SLUG='bella-estetica';
const DN=['dom','lun','mar','mié','jue','vie','sáb'],MN=['ene','feb','mar','abr','may','jun','jul','ago','sep','oct','nov','dic'];
const DAYS=[...Array(8)].map((_,i)=>{const d=new Date();d.setHours(0,0,0,0);d.setDate(d.getDate()+i);return d});
const tm=m=>Math.floor(m/60)+':'+String(m%60).padStart(2,'0');
const tmd=d=>d.getHours()+':'+String(d.getMinutes()).padStart(2,'0');
const dl=d=>DN[d.getDay()]+' '+d.getDate()+' '+MN[d.getMonth()];
const esc=t=>String(t).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
let salon,SV=[],ST=[],busy=[],user=null,appts=[];
let s={owner:false,svc:null,sty:null,day:0,time:null,done:null,oday:0,loginErr:'',msg:''};
const sv=id=>SV.find(x=>x.id==id),st=id=>ST.find(x=>x.id==id);
const isAdmin=()=>location.hash=='#admin';
async function init(){
 const {data:sa,error}=await sb.from('salons').select('*').eq('slug',SLUG).single();
 if(error||!sa){document.getElementById('app').innerHTML='<p class="err">No se pudo conectar con la base de datos. Revisa que el proyecto de Supabase esté activo.</p>';return}
 salon=sa;
 SV=(await sb.from('services').select('*').eq('salon_id',salon.id).order('price')).data||[];
 ST=(await sb.from('stylists').select('*').eq('salon_id',salon.id).order('name')).data||[];
 const {data:{session}}=await sb.auth.getSession();user=session?session.user:null;
 await loadBusy();if(isAdmin()&&user)await loadAppts();render()}
async function loadBusy(){
 const to=new Date(DAYS[7]);to.setDate(to.getDate()+1);
 const {data}=await sb.rpc('busy_times',{p_salon:salon.id,p_from:DAYS[0].toISOString(),p_to:to.toISOString()});
 busy=(data||[]).map(b=>({sty:b.stylist_id,s:new Date(b.starts_at),e:new Date(b.ends_at)}))}
async function loadAppts(){
 const a=new Date(DAYS[s.oday]),b=new Date(a);b.setDate(b.getDate()+1);
 const {data}=await sb.from('appointments').select('*').eq('salon_id',salon.id).gte('starts_at',a.toISOString()).lt('starts_at',b.toISOString()).order('starts_at');
 appts=data||[]}
function slots(di,styId,svcId){
 const d=DAYS[di],t=st(styId),dur=sv(svcId).minutes,out=[],now=new Date();
 if(t.days_off.includes(d.getDay()))return out;
 for(let x=600;x+dur<=1140;x+=30){
  const a=new Date(d);a.setHours(0,x,0,0);const e=new Date(a.getTime()+dur*60000);
  if(a<=now)continue;
  if(busy.some(b=>b.sty==styId&&a<b.e&&e>b.s))continue;
  out.push(x)}
 return out}
function dayBtns(cur,fn,styId){return DAYS.map((d,i)=>{const n=styId?slots(i,styId,s.svc).length:1;return `<button class="chip day ${cur==i?'on':''}" ${n?'':'disabled'} onclick="${fn}(${i})">${DN[d.getDay()]}<b>${d.getDate()}</b></button>`}).join('')}
function pickDay(i){s.day=i;s.time=null;render()}
function pickSty(id){s.sty=id;const i=DAYS.findIndex((_,k)=>slots(k,id,s.svc).length);s.day=i<0?0:i;render()}
function client(){
 if(s.done){const a=s.done;return `<div class="ok"><h2>${s.owner?'Cita agregada':'¡Listo, tu cita está confirmada!'}</h2><p><b>${esc(sv(a.svc).name)}</b> con ${esc(st(a.sty).name)}<br>${dl(DAYS[a.di])} a las ${tm(a.start)}</p><p>${s.owner?'Ya aparece en la agenda.':'Guarda esta pantalla. Si necesitas cambiar tu cita, avísale a la estética por WhatsApp.'}</p><button class="btn" onclick="${s.owner?'backOwner()':'reset()'}">${s.owner?'Volver a la agenda':'Agendar otra cita'}</button></div>`}
 if(!s.svc)return `<h2>${s.owner?'Nueva cita: ¿qué servicio?':esc(salon.name)+' · ¿qué servicio quieres?'}</h2>`+SV.map(v=>`<button class="opt" onclick="s.svc='${v.id}';render()"><div><b>${esc(v.name)}</b><br><span>${v.minutes} min</span></div><b>$${v.price}</b></button>`).join('')+(s.owner?'<button class="back" onclick="backOwner()">‹ Volver a la agenda</button>':'');
 if(!s.sty)return `<button class="back" onclick="s.svc=null;render()">‹ Cambiar servicio</button><h2>¿Con quién?</h2>`+ST.map(t=>`<button class="opt" onclick="pickSty('${t.id}')"><b>${esc(t.name)}</b><span>${DAYS.some((_,i)=>slots(i,t.id,s.svc).length)?'Ver horarios':'Sin horarios'}</span></button>`).join('');
 const sl=slots(s.day,s.sty,s.svc);
 if(s.time==null)return `<button class="back" onclick="s.sty=null;render()">‹ Cambiar estilista</button><h2>Elige día y hora con ${esc(st(s.sty).name)}</h2><div class="scroll"><div class="chips" style="flex-wrap:nowrap">${dayBtns(s.day,'pickDay',s.sty)}</div></div><h2 style="font-size:16px">${dl(DAYS[s.day])}</h2>${sl.length?`<div class="chips">${sl.map(x=>`<button class="chip" onclick="s.time=${x};render()">${tm(x)}</button>`).join('')}</div>`:'<p class="empty">No hay horarios libres este día. Prueba con otro.</p>'}`;
 return `<button class="back" onclick="s.time=null;render()">‹ Cambiar horario</button><h2>Tus datos</h2><div class="sum"><b>${esc(sv(s.svc).name)}</b> · $${sv(s.svc).price}<br>${esc(st(s.sty).name)} · ${dl(DAYS[s.day])} a las ${tm(s.time)}</div><label for="nm">${s.owner?'Nombre de la clienta':'Tu nombre'}</label><input id="nm" autocomplete="name"><label for="ph">Teléfono (WhatsApp, 10 dígitos)</label><input id="ph" type="tel" inputmode="tel" autocomplete="tel"><p id="er" class="err"></p><button class="btn" id="bk" onclick="book()">Confirmar cita</button>`}
async function book(){
 const n=document.getElementById('nm').value.trim(),p=document.getElementById('ph').value.replace(/\D/g,''),er=document.getElementById('er');
 if(!n||p.length<10){er.textContent='Escribe el nombre y un teléfono de 10 dígitos.';return}
 document.getElementById('bk').disabled=true;
 const a=new Date(DAYS[s.day]);a.setHours(0,s.time,0,0);const e=new Date(a.getTime()+sv(s.svc).minutes*60000);
 const {error}=await sb.from('appointments').insert({salon_id:salon.id,stylist_id:s.sty,service_id:s.svc,starts_at:a.toISOString(),ends_at:e.toISOString(),client_name:n,client_phone:p});
 if(error){await loadBusy();if(error.code=='23P01'){s.time=null;s.msg='Ese horario se acaba de ocupar. Elige otro.';render();return}
  er.textContent='No se pudo guardar la cita. Intenta de nuevo.';document.getElementById('bk').disabled=false;return}
 await loadBusy();s.done={svc:s.svc,sty:s.sty,start:s.time,di:s.day};render()}
function reset(){s={...s,svc:null,sty:null,time:null,done:null};render()}
async function backOwner(){s={...s,owner:false,svc:null,sty:null,time:null,done:null};await loadAppts();render()}
function login(){return `<h2>Acceso para la dueña</h2><div class="card"><label for="em">Correo</label><input id="em" type="email" autocomplete="username"><label for="pw">Contraseña</label><input id="pw" type="password" autocomplete="current-password"><p class="err">${esc(s.loginErr)}</p><button class="btn" onclick="doLogin()">Entrar</button></div>`}
async function doLogin(){
 const {data,error}=await sb.auth.signInWithPassword({email:document.getElementById('em').value.trim(),password:document.getElementById('pw').value});
 if(error){s.loginErr='Correo o contraseña incorrectos.';render();return}
 user=data.user;s.loginErr='';await loadAppts();render()}
async function logout(){await sb.auth.signOut();user=null;render()}
function owner(){
 const mine=salon.owner_id&&user&&salon.owner_id==user.id;
 if(!mine)return `<div class="card"><b>Tu usuario todavía no está asignado a esta estética.</b><p class="empty">Falta un paso en Supabase para darte permiso de ver las citas.</p></div>`;
 const inc=appts.reduce((t,a)=>t+(sv(a.service_id)?sv(a.service_id).price:0),0);
 return `<div class="scroll"><div class="chips" style="flex-wrap:nowrap">${dayBtns(s.oday,'ownDay')}</div></div>
 <div class="stats"><div class="card"><b>${appts.length}</b><span>citas</span></div><div class="card"><b>$${inc}</b><span>ingreso estimado</span></div><button class="btn" onclick="s={...s,owner:true,svc:null,sty:null,time:null,done:null};render()">+ Nueva cita</button></div>
 <div class="cols">${ST.map(t=>{const l=appts.filter(a=>a.stylist_id==t.id);const off=t.days_off.includes(DAYS[s.oday].getDay());return `<div class="col"><h3>${esc(t.name)}</h3>${off?'<p class="empty">No trabaja este día</p>':l.length?l.map(a=>{const st0=new Date(a.starts_at),en=new Date(a.ends_at);return `<div class="ap"><b>${tmd(st0)}–${tmd(en)}</b><br>${esc(a.client_name)}<br><span style="color:var(--mut)">${sv(a.service_id)?esc(sv(a.service_id).name):''}</span><div class="r"><button class="btn sm" onclick="wa('${a.id}')">Enviar recordatorio</button><button class="btn sm ghost" onclick="cancel('${a.id}')">Cancelar</button></div></div>`}).join(''):'<p class="empty">Sin citas. Horario libre.</p>'}</div>`}).join('')}</div>`}
async function ownDay(i){s.oday=i;await loadAppts();render()}
async function cancel(id){if(!confirm('¿Cancelar esta cita?'))return;await sb.from('appointments').delete().eq('id',id);await loadBusy();await loadAppts();render()}
function wa(id){const a=appts.find(x=>x.id==id),d=new Date(a.starts_at);
 const m=`Hola ${a.client_name.split(' ')[0]}, te recordamos tu cita en ${salon.name}: ${sv(a.service_id).name} con ${st(a.stylist_id).name}, ${dl(d)} a las ${tmd(d)}. ¿Nos confirmas? Responde SÍ para confirmar.`;
 window.open('https://wa.me/52'+a.client_phone+'?text='+encodeURIComponent(m),'_blank')}
function render(){
 const adm=isAdmin();
 document.getElementById('sub').textContent=adm?'Agenda de '+(salon?salon.name:''):'Reserva en línea';
 document.getElementById('out').style.display=adm&&user?'':'none';
 let h;
 if(adm&&!user)h=login();else if(adm&&!s.owner)h=owner();else h=(s.msg?`<p class="err">${esc(s.msg)}</p>`:'')+client();
 s.msg='';
 document.getElementById('app').innerHTML=h;
 document.getElementById('foot').innerHTML=adm?'<a href="#" onclick="location.hash=\'\';location.reload()">Ir a la página de reservas</a>':'<a href="#admin" onclick="setTimeout(()=>location.reload(),50)">Acceso dueña</a>'}
window.addEventListener('hashchange',()=>{if(salon)init()});
init();
</script>
</body>
</html>
