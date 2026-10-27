[[ORBIT-EO ideas]]
You are given a task to integrate an existing React component in the codebase

The codebase should support:
- shadcn project structure  
- Tailwind CSS
- Typescript

If it doesn't, provide instructions on how to setup project via shadcn CLI, install Tailwind or Typescript.

Determine the default path for components and styles. 
If default path for components is not /components/ui, provide instructions on why it's important to create this folder
Copy-paste this component to /components/ui folder:
```tsx
finance-bento.tsx
"use client";
// @ts-nocheck
/* The charts are drawn with plain DOM + SVG inside an effect, so this file
   is checked as JavaScript. No dependencies. */

import * as React from "react";

/* ------------------------------------------------------------------ */
/*  Finance Bento: a pro personal-finance dashboard for fiat, stocks  */
/*  and crypto. Net-worth stream, allocation gauge, KPI tiles, cash   */
/*  flow, crypto candles and weekly stocks. Responsive, dark.         */
/* ------------------------------------------------------------------ */

export interface FinanceBentoData {
  /** Month-end balances by asset class, oldest first (24+ months for "All"). */
  history: { date: string; fiat: number; stocks: number; crypto: number }[];
  payments: { name: string; date: string; amount: number }[];
  goals: { name: string; progress: number; target: number }[];
  /** Six months of income and spending, in thousands; first row is the latest year. */
  cashflow: { months: string[]; income: number[][]; spending: number[][] };
  /** Each asset: share of the crypto pot, 24h change and 7 daily candles [open, high, low, close]. */
  crypto: { name: string; symbol: string; share: number; change: string; ohlc: number[][] }[];
  /** Weekly trading volume, in thousands. */
  stocks: { days: string[]; values: number[] };
}

export interface FinanceBentoProps {
  data: FinanceBentoData;
  title?: string;
  subtitle?: string;
  avatar?: string;
  avatarAlt?: string;
  /** The "by vasnev.art" badge under the grid. */
  credit?: boolean;
  className?: string;
  style?: React.CSSProperties;
}

const CSS = `.fpb{font-family:Geist,ui-sans-serif,system-ui,-apple-system,sans-serif;-webkit-font-smoothing:antialiased;color:#fff;width:100%}
.fpb *{box-sizing:border-box}
.fpb .fpb-c{border-radius:28px}
.fpb .vbrow{display:flex;justify-content:flex-end;margin-top:16px}
.fpb{--acc:#E8FF1F;--t1:#fff;--t2:rgba(255,255,255,.56);--t3:rgba(255,255,255,.38);--hair:rgba(255,255,255,.14);--ease:cubic-bezier(.32,.72,0,1);
    --fiat:#FFFFFF;--stocks:#A9B2FF;--crypto:#FFB37A}
.fpb *{box-sizing:border-box}
.fpb .fpb-c{position:relative;padding:20px 22px 22px;background:#000;color:#fff}
.fpb .top{display:flex;align-items:center;gap:14px;height:44px;margin-bottom:16px}
.fpb .top h1{margin:0;font-size:28px;font-weight:700;letter-spacing:-.03em;flex:1}
.fpb .top h1 span{font-weight:500;color:var(--t3);font-size:15px;letter-spacing:-.01em;margin-left:12px}
.fpb .seg{display:flex;padding:3px;gap:2px;border-radius:14px;background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.12)}
.fpb .seg button{border:0;height:28px;min-width:44px;padding:0 12px;border-radius:11px;background:transparent;color:var(--t2);font:600 12.5px Geist,sans-serif;cursor:pointer;transition:background .25s,color .25s}
.fpb .seg button[aria-pressed="true"]{background:#fff;color:#0b0b0c}
.fpb .seg button:focus-visible{outline:2px solid var(--acc);outline-offset:2px}
.fpb .ctrls{display:flex;gap:10px}
.fpb .ava{width:40px;height:40px;border-radius:50%;object-fit:cover;box-shadow:0 0 0 2px rgba(255,255,255,.18);flex:0 0 auto}
.fpb .grid{display:grid;grid-template-columns:repeat(12,minmax(0,1fr));gap:14px}
.fpb .h1{height:376px}
.fpb .h2{height:168px}
.fpb .h3{height:330px}
.fpb #nw{grid-column:span 8}
.fpb #alloc{grid-column:span 4}
.fpb .kpi{grid-column:span 3}
.fpb #flow{grid-column:span 5}
.fpb #crypto{grid-column:span 4}
.fpb #stocks{grid-column:span 3}
@media (max-width:1199px){.fpb #nw{grid-column:span 12}
.fpb #alloc{grid-column:span 6;grid-row:span 2;height:auto}
.fpb .kpi{grid-column:span 6}
.fpb #flow{grid-column:span 12}
.fpb #crypto,.fpb #stocks{grid-column:span 6}}
@media (max-width:767px){.fpb .fpb-c{padding:16px 14px}
.fpb .top{flex-wrap:wrap;height:auto;gap:12px}
.fpb .top h1{flex:1;font-size:24px}
.fpb .top h1 span{display:block;margin:4px 0 0}
.fpb .ctrls{order:3;flex:1 0 100%;justify-content:space-between}
.fpb .ava{order:2}
.fpb #nw{height:420px}
.fpb #nw .legend{flex:1 0 100%;order:3;margin-top:8px}
.fpb #nw .hrow{flex-wrap:wrap}
.fpb #nw .nrow{flex-wrap:wrap;gap:6px 16px}
.fpb #alloc{grid-column:span 12;grid-row:auto;height:376px}
.fpb .kpi{grid-column:span 6}
.fpb #crypto,.fpb #stocks{grid-column:span 12}
.fpb .grid{gap:12px}}
@media (max-width:520px){.fpb .kpi{grid-column:span 12}}
.fpb .card{position:relative;border-radius:28px;overflow:hidden;opacity:0;transform:translateY(14px) scale(.985);transition:opacity .7s ease,transform .9s var(--ease)}
.fpb .in .card{opacity:1;transform:none}
.fpb .card .rim{position:absolute;inset:0;border-radius:inherit;box-shadow:inset 0 1px 0 rgba(255,255,255,.2),inset 0 0 0 1px rgba(255,255,255,.06);pointer-events:none;z-index:9}
.fpb .g{position:absolute;inset:0}
.fpb .core{position:absolute;left:10%;right:10%;top:16%;bottom:-6%;border-radius:90px;filter:blur(60px)}
.fpb .glass{background:rgba(255,255,255,.06);border:1px solid rgba(255,255,255,.12);backdrop-filter:blur(20px)}
.fpb .glass .rim{display:none}
.fpb .solid{background:var(--acc);color:#0b0b0c}
.fpb .solid .rim{box-shadow:inset 0 1px 0 rgba(255,255,255,.5)}
.fpb .in-c{position:absolute;inset:0;padding:22px 24px;z-index:2}
.fpb .ttl{font-size:18px;font-weight:600;letter-spacing:-.02em;display:flex;align-items:center;gap:8px}
.fpb .cap{font-size:11.5px;font-weight:600;letter-spacing:.06em;text-transform:uppercase;color:var(--t2)}
.fpb .solid .cap{color:rgba(0,0,0,.55)}
.fpb .big{font-size:40px;font-weight:200;letter-spacing:-.04em;line-height:1;font-variant-numeric:tabular-nums}
.fpb .delta{font:600 12.5px "Geist Mono",monospace;color:var(--acc);letter-spacing:-.02em}
.fpb .muted{font-size:12.5px;color:var(--t3);font-weight:500;letter-spacing:-.01em}
.fpb .chip{display:inline-flex;align-items:center;height:22px;padding:0 9px;border-radius:11px;background:rgba(0,0,0,.85);color:#fff;font:600 11px "Geist Mono",monospace;letter-spacing:-.02em}
.fpb .legend{display:flex;gap:16px;font-size:12px;font-weight:500;color:var(--t2)}
.fpb .legend span{display:inline-flex;align-items:center;gap:6px}
.fpb .legend i{width:8px;height:8px;border-radius:50%}
.fpb .row{display:flex;align-items:center;gap:12px}
.fpb .mono{font-family:"Geist Mono",monospace;font-variant-numeric:tabular-nums}
.fpb .dots circle{transition:opacity .26s,transform .26s;transform-box:fill-box;transform-origin:center}
.fpb .tip{position:fixed;z-index:50;pointer-events:none;background:rgba(14,14,16,.92);color:#fff;border:1px solid rgba(255,255,255,.12);border-radius:14px;padding:9px 11px;font-size:12px;
    backdrop-filter:blur(16px);box-shadow:0 12px 30px -10px rgba(0,0,0,.6);opacity:0;transform:translateY(4px);transition:opacity .15s,transform .15s;white-space:nowrap;min-width:150px}
.fpb .tip.on{opacity:1;transform:none}
.fpb .tip .t{color:var(--t2);margin-bottom:5px;font-weight:500}
.fpb .tip .r{display:flex;justify-content:space-between;gap:14px;align-items:center;line-height:19px}
.fpb .tip .r span{display:inline-flex;align-items:center;gap:6px;color:var(--t2)}
.fpb .tip i{width:7px;height:7px;border-radius:50%}
.fpb .tip b{font:500 12px "Geist Mono",monospace}
.fpb .vb-badge{--vb-on:#6a5cff;display:inline-flex;align-items:center;gap:9px;height:34px;padding:0 12px 0 11px;border-radius:17px;background:#111113;border:1px solid rgba(255,255,255,.1);color:#fff;text-decoration:none;font:500 13px/1 Geist,sans-serif;letter-spacing:-.02em;box-shadow:0 8px 24px -14px rgba(0,0,0,.25);transition:border-color .25s}
.fpb .vb-badge:hover{border-color:color-mix(in oklab,var(--vb-on) 45%,transparent)}
.fpb .vb-mark{display:grid;grid-template-columns:repeat(5,3px);grid-template-rows:repeat(3,3px);gap:2px}
.fpb .vb-mark i{width:3px;height:3px;border-radius:50%;background:rgba(255,255,255,.16);transition:background .3s}
.fpb .vb-mark i.on{background:var(--vb-on)}
.fpb .vb-badge:hover .vb-mark i.on{animation:vbw .9s cubic-bezier(.3,1.4,.4,1) calc(var(--x)*60ms) 1}
.fpb .vb-badge:hover .vb-mark i:not(.on){background:color-mix(in oklab,var(--vb-on) 22%,transparent)}
@keyframes vbw{0%,100%{transform:none}40%{transform:translateY(-2px) scale(1.35)}}
.fpb .vb-by{color:rgba(255,255,255,.5)}
.fpb .vb-arr{height:12px;width:0;margin-left:-9px;opacity:0;transform:translate(-4px,4px);transition:all .3s cubic-bezier(.2,.9,.25,1)}
.fpb .vb-badge:hover .vb-arr{width:12px;margin-left:0;opacity:1;transform:none}
@media (prefers-reduced-motion: reduce){.fpb .card{transition:none}}`;

const esc = (v) => String(v ?? "").replace(/[&<>"']/g, (c) => ({ "&": "&amp;", "<": "&lt;", ">": "&gt;", '"': "&quot;", "'": "&#39;" })[c]);
const markup = (o) => `<div class="fpb-c canvas">
      <div class="top">
        <h1>${esc(o.title)}<span id="asof">${esc(o.subtitle)}</span></h1>
        <div class="ctrls"><div class="seg" id="period" role="group" aria-label="Period"></div>
        <div class="seg" id="ccy" role="group" aria-label="Currency"></div></div>
        ${o.avatar?`<img class="ava" alt="${esc(o.avatarAlt)}" src="${esc(o.avatar)}">`:""}
      </div>
      <div class="grid" id="grid">
        <!-- net worth -->
        <section class="card h1" id="nw" aria-label="Net worth"></section>
        <!-- allocation -->
        <section class="card h1" id="alloc" aria-label="Allocation"></section>
        <!-- KPI row -->
        <section class="card h2 kpi" id="ret" aria-label="Portfolio return"></section>
        <section class="card glass h2 kpi" id="cash" aria-label="Free cash"></section>
        <section class="card glass h2 kpi" id="pay" aria-label="Upcoming payments"></section>
        <section class="card glass h2 kpi" id="goals" aria-label="Goals"></section>
        <!-- bottom row -->
        <section class="card h3" id="flow" aria-label="Cash flow"></section>
        <section class="card h3" id="crypto" aria-label="Crypto"></section>
        <section class="card h3" id="stocks" aria-label="Stocks"></section>
      </div>
    </div>
${o.credit?`<div class="vbrow"><a class="vb-badge" href="https://vasnev.art" target="_blank" rel="noopener" aria-label="Made by vasnev.art"><span class="vb-mark" id="vbm" aria-hidden="true"></span><span><span class="vb-by">by</span> vasnev.art</span><svg class="vb-arr" viewBox="0 0 12 12" aria-hidden="true"><path d="M3.5 8.5l5-5M4.5 3.5h4v4" fill="none" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg></a></div>`:""}
<div class="tip" id="tip"></div>`;

function mount(root, data) {
  let alive = true;
  const frames = new Set();
  const RAF = (cb) => { if (!alive) return 0; const id = requestAnimationFrame((t) => { frames.delete(id); if (alive) cb(t); }); frames.add(id); return id; };
  const $id = (id) => root.querySelector("#" + id);

const reduce=window.matchMedia('(prefers-reduced-motion: reduce)').matches;
const ACC='#E8FF1F',T2='rgba(255,255,255,.56)',T3='rgba(255,255,255,.38)',HAIR='rgba(255,255,255,.14)';
const COL={fiat:'#FFFFFF',stocks:'#A9B2FF',crypto:'#FFB37A'};
const ease=t=>1-Math.pow(1-t,3);
const NS='http://www.w3.org/2000/svg';
const el=(t,a={},p)=>{const e=document.createElementNS(NS,t);for(const k in a)e.setAttribute(k,a[k]);p&&p.appendChild(e);return e};
let uid=0;

/* ---------- state ---------- */
const S={period:'1Y',ccy:'USD'};
const RATE={USD:1,EUR:.92},SYM={USD:'$',EUR:'€'};
const money=(v,{dec=0,compact=false}={})=>{v*=RATE[S.ccy];const s=SYM[S.ccy];
  if(compact&&Math.abs(v)>=1000)return s+(v/1000).toFixed(Math.abs(v)>=10000?0:1)+'K';
  return (v<0?'−':'')+s+Math.abs(v).toLocaleString('en-US',{minimumFractionDigits:dec,maximumFractionDigits:dec})};

/* ---------- data: 36 months of fiat / stocks / crypto ---------- */
const H=data.history.map(h=>({d:new Date(h.date),fiat:h.fiat,stocks:h.stocks,crypto:h.crypto}));
function series(){
  if(S.period==='All')return H.map(h=>({...h,l:h.d.toLocaleDateString('en-US',{month:'short',year:'2-digit'})}));
  if(S.period==='1Y')return H.slice(-12).map(h=>({...h,l:h.d.toLocaleDateString('en-US',{month:'short'})}));
  const n=S.period==='3M'?13:5,src=H.slice(S.period==='3M'?-4:-2),out=[];
  for(let k=0;k<n;k++){const f=k/(n-1)*(src.length-1),i=Math.min(src.length-2,Math.floor(f)),t=f-i,a=src[i],b=src[i+1],w=Math.sin(k*1.7)*.012;
    const d=new Date(H[H.length-1].d.getTime()-(n-1-k)*7*864e5);out.push({d,l:d.toLocaleDateString('en-US',{month:'short',day:'numeric'}),fiat:(a.fiat+(b.fiat-a.fiat)*t)*(1+w*.3),stocks:(a.stocks+(b.stocks-a.stocks)*t)*(1+w),crypto:(a.crypto+(b.crypto-a.crypto)*t)*(1+w*2.4)})}
  return out;
}
const total=p=>p.fiat+p.stocks+p.crypto;

/* ---------- helpers ---------- */
function bg({top,base,bottom,core}){return `<div class="g" style="background:linear-gradient(180deg,${top} 0%,${base} 36%,${base} 74%,${bottom} 100%)"></div><div class="core" style="background:${core}"></div>`}
const GR={
  blue:{top:'#b9a6ff',base:'#2e43ff',bottom:'#6d7dff',core:'#05082a'},
  graph:{top:'#9a9aa8',base:'#3a3a44',bottom:'#55555f',core:'#060608'},
  pink:{top:'#ffb3d6',base:'#ff2a85',bottom:'#ff6aa8',core:'#3a031c'},
  copper:{top:'#f6bf96',base:'#c2683a',bottom:'#dc8a5c',core:'#1d0b05'},
  green:{top:'#a6e8c8',base:'#36956f',bottom:'#58b58c',core:'#061d15'},
  lime:{top:'#f4ffb8',base:'#a3c400',bottom:'#c9e03a',core:'#121700'},
};
function smoothPath(P,move=true){let d=(move?'M':'L')+`${P[0][0]},${P[0][1]}`;for(let i=0;i<P.length-1;i++){const p0=P[i-1]||P[i],p1=P[i],p2=P[i+1],p3=P[i+2]||p2,k=.17;
  d+=`C${p1[0]+(p2[0]-p0[0])*k},${p1[1]+(p2[1]-p0[1])*k} ${p2[0]-(p3[0]-p1[0])*k},${p2[1]-(p3[1]-p1[1])*k} ${p2[0]},${p2[1]}`}return d}
const P2=(r,a)=>[r*Math.cos(a*Math.PI/180),r*Math.sin(a*Math.PI/180)];
const F={'0':['01110','10001','10011','10101','11001','10001','01110'],'1':['00100','01100','00100','00100','00100','00100','01110'],'2':['01110','10001','00001','00010','00100','01000','11111'],'3':['11111','00010','00100','00010','00001','10001','01110'],'4':['00010','00110','01010','10010','11111','00010','00010'],'5':['11111','10000','11110','00001','00001','10001','01110'],'6':['00110','01000','10000','11110','10001','10001','01110'],'7':['11111','00001','00010','00100','01000','01000','01000'],'8':['01110','10001','10001','01110','10001','10001','01110'],'9':['01110','10001','10001','01111','00001','00010','01100'],'$':['00100','01111','10100','01110','00101','11110','00100'],'€':['00111','01000','11110','01000','11110','01000','00111'],',':['0','0','0','0','0','1','1'],' ':['00','00','00','00','00','00','00']};
function DotNumber(host,pitch){
  const cols=48,svg=el('svg',{class:'dots','aria-hidden':'true'},host),cells=[];
  for(let c=0;c<cols;c++)for(let r=0;r<7;r++){const ci=el('circle',{cx:c*pitch+pitch/2,cy:r*pitch+pitch/2,r:pitch*.36,fill:'#fff',opacity:0},svg);ci.style.transitionDelay=(c*7)+'ms';cells.push([c,r,ci])}
  const set=t=>{const on=new Set();let x=0;for(const ch of t){const g=F[ch]||F[' '];const w=g[0].length;for(let r=0;r<7;r++)for(let k=0;k<w;k++)if(g[r][k]==='1')on.add((x+k)*8+r);x+=w+1}
    const used=Math.max(1,x-1);for(const [c,r,ci] of cells){const v=on.has(c*8+r);ci.setAttribute('opacity',v?1:(c<used?.08:0));ci.style.transform=v?'none':'scale(.55)'}
    svg.setAttribute('width',used*pitch);svg.setAttribute('height',7*pitch);svg.setAttribute('viewBox',`0 0 ${used*pitch} ${7*pitch}`)};
  let raf=0,cur=0;
  return {count(to,dur=900){cancelAnimationFrame(raf);const from=cur,t0=performance.now();let last='';
    const f=now=>{const t=reduce?1:Math.min(1,(now-t0)/dur);cur=from+(to-from)*ease(t);const s=SYM[S.ccy]+Math.round(cur*RATE[S.ccy]).toLocaleString('en-US');if(s!==last){set(s);last=s}if(t<1)raf=RAF(f)};raf=RAF(f)}};
}
function tween(node,to,fmt,dur=900){const from=+node.dataset.v||0,t0=performance.now();node.dataset.v=to;
  (function f(now){const t=reduce?1:Math.min(1,(now-t0)/dur);node.textContent=fmt(from+(to-from)*ease(t));if(t<1)RAF(f)})(t0)}
const tip=$id('tip');
function showTip(x,y,title,rows){tip.innerHTML=`<div class="t">${title}</div>`+rows.map(([k,v,c])=>`<div class="r"><span>${c?`<i style="background:${c}"></i>`:''}${k}</span><b>${v}</b></div>`).join('');
  tip.classList.add('on');const r=tip.getBoundingClientRect();let L=x+16,T=y-r.height-14;if(L+r.width>innerWidth-8)L=x-r.width-16;if(T<8)T=y+16;tip.style.left=L+'px';tip.style.top=T+'px'}
const hideTip=()=>tip.classList.remove('on');

/* ================= cards ================= */

/* ---- Net worth: a stream of the three asset classes ---- */
const nw=$id('nw');
nw.innerHTML=`${bg(GR.blue)}<div class="in-c">
  <div class="row hrow" style="justify-content:space-between"><div class="ttl">Net worth</div>
    <div class="legend"><span><i style="background:${COL.fiat}"></i>Fiat</span><span><i style="background:${COL.stocks}"></i>Stocks</span><span><i style="background:${COL.crypto}"></i>Crypto</span></div></div>
  <div class="row nrow" style="margin-top:16px;align-items:flex-end;gap:16px"><div class="nwn"></div>
    <div style="padding-bottom:6px"><div class="delta nwd"></div><div class="muted nwc" style="margin-top:3px"></div></div></div>
  <svg class="nws" style="position:absolute;left:0;right:0;bottom:0;display:block" aria-hidden="true"></svg></div><div class="rim"></div>`;
const nwNum=DotNumber(nw.querySelector('.nwn'),innerWidth<520?5.4:7);
let nwGrow=0,nwRaf=0,nwData=null,nwPrev=null;
function drawNW(){
  const svg=nw.querySelector('.nws'),W=nw.clientWidth,Hh=226,top=26,bot=26,mid=top+(Hh-top-bot)/2;
  svg.setAttribute('width',W);svg.setAttribute('height',Hh);svg.setAttribute('viewBox',`0 0 ${W} ${Hh}`);
  const D=nwData,n=D.length,tots=D.map(total),mx=Math.max(...tots),mn=Math.min(...tots),floor=mn-(mx-mn)*.9,half=(Hh-top-bot)/2;
  const k=p=>((total(p)-floor)/(mx-floor))*half*2/total(p);
  const X=i=>-20+i/(n-1)*(W+40);
  // grow from the centre line
  const g=ease(nwGrow);
  const lay=D.map(p=>{const q=k(p)*g/2,f=p.fiat*q,s=p.stocks*q,c=p.crypto*q;return {f,fs:f+s,fsc:f+s+c}});
  const band=k=>{const T=D.map((p,i)=>[X(i),mid-lay[i][k]]),B=D.map((p,i)=>[X(i),mid+lay[i][k]]).reverse();return smoothPath(T)+smoothPath(B,false)+'Z'};
  const marks=[Math.round((n-1)*.18),Math.round((n-1)*.55),n-1];
  svg.innerHTML=`
    <defs><linearGradient id="nwf" x1="0" x2="1"><stop offset="0" stop-color="#fff" stop-opacity=".0"/><stop offset=".25" stop-color="#fff" stop-opacity="1"/></linearGradient>
      <mask id="nwm"><rect width="${W}" height="${Hh}" fill="url(#nwf)"/></mask></defs>
    <g mask="url(#nwm)"><path d="${band('fsc')}" fill="${COL.crypto}" opacity=".95"/><path d="${band('fs')}" fill="${COL.stocks}"/><path d="${band('f')}" fill="${COL.fiat}" opacity=".95"/></g>
    ${marks.map((i,k)=>{const x=Math.min(W-36,Math.max(36,X(i)));const t=total(D[i]);return `<g style="opacity:${g>.6?1:0};transition:opacity .4s ${k*.08}s">
      <line x1="${x}" x2="${x}" y1="${top-4}" y2="${Hh-bot+4}" stroke="rgba(0,0,0,.55)" stroke-width="1.5"/>
      <rect x="${x-34}" y="${top-14}" width="68" height="22" rx="11" fill="#000"/><text x="${x}" y="${top+1}" text-anchor="middle" font-size="11" font-weight="600" fill="#fff" font-family="Geist Mono">${money(t,{compact:true})}</text>
      <rect x="${x-30}" y="${Hh-bot-8}" width="60" height="22" rx="11" fill="#000"/><text x="${x}" y="${Hh-bot+7}" text-anchor="middle" font-size="10.5" font-weight="500" fill="rgba(255,255,255,.7)" font-family="Geist Mono">${D[i].l}</text></g>`}).join('')}
    <line class="hl" y1="${top}" y2="${Hh-bot}" stroke="#fff" stroke-width="1.5" opacity="0"/>
    <rect width="${W}" height="${Hh}" fill="transparent" class="hit"/>`;
  const hit=svg.querySelector('.hit'),hl=svg.querySelector('.hl');
  hit.onpointermove=e=>{const r=svg.getBoundingClientRect(),z=r.width/W,x=(e.clientX-r.left)/z;const i=Math.max(0,Math.min(n-1,Math.round((x+20)/(W+40)*(n-1))));const p=D[i];
    hl.setAttribute('x1',X(i));hl.setAttribute('x2',X(i));hl.setAttribute('opacity',1);
    showTip(r.left+X(i)*z,r.top+(mid-lay[i].fsc)*z,p.l,[['Crypto',money(p.crypto),COL.crypto],['Stocks',money(p.stocks),COL.stocks],['Fiat',money(p.fiat),COL.fiat],['Total',money(total(p))]])};
  hit.onpointerleave=()=>{hl.setAttribute('opacity',0);hideTip()};
}
function updateNW(){
  nwData=series();const D=nwData,a=total(D[0]),b=total(D[D.length-1]);
  nwNum.count(b);
  nw.querySelector('.nwd').textContent=(b>=a?'↑ ':'↓ ')+Math.abs((b-a)/a*100).toFixed(1)+'%';
  nw.querySelector('.nwc').textContent=(b>=a?'+':'−')+money(Math.abs(b-a))+' '+({'1M':'this month','3M':'in 3 months','1Y':'this year','All':'in 3 years'})[S.period];
  cancelAnimationFrame(nwRaf);const t0=performance.now();nwGrow=0;
  const f=now=>{nwGrow=reduce?1:Math.min(1,(now-t0)/1100);drawNW();if(nwGrow<1)nwRaf=RAF(f)};nwRaf=RAF(f);
}

/* ---- Allocation: a half gauge of tick marks ---- */
const al=$id('alloc');
al.innerHTML=`${bg(GR.graph)}<div class="in-c">
  <div class="row" style="justify-content:space-between"><div class="ttl">Allocation</div><span class="chip" style="background:rgba(255,255,255,.12)">Target 60/40</span></div>
  <svg class="alg" width="300" height="168" viewBox="-150 -150 300 168" style="position:absolute;left:50%;margin-left:-150px;top:70px" aria-hidden="true"></svg>
  <div class="all" style="position:absolute;left:24px;right:24px;bottom:20px"></div></div><div class="rim"></div>`;
function updateAlloc(){
  const p=nwData[nwData.length-1],t=total(p),fr={stocks:p.stocks/t,crypto:p.crypto/t,fiat:p.fiat/t};
  const n=56,svg=al.querySelector('.alg');let s='';
  for(let i=0;i<=n;i++){const f=i/n,a=180+f*180,cls=f<=fr.stocks?'stocks':f<=fr.stocks+fr.crypto?'crypto':'fiat';const [x1,y1]=P2(136,a),[x2,y2]=P2(i%7===0?112:118,a);
    s+=`<line x1="${x1.toFixed(1)}" y1="${y1.toFixed(1)}" x2="${x2.toFixed(1)}" y2="${y2.toFixed(1)}" stroke="${COL[cls]}" stroke-opacity="${cls==='fiat'?.35:1}" stroke-width="3.2" stroke-linecap="round" style="opacity:0;animation:tk .4s ${i*12}ms forwards"/>`}
  const inv=Math.round((fr.stocks+fr.crypto)*100);
  s+=`<text y="-22" text-anchor="middle" font-family="Geist" font-weight="200" font-size="52" fill="#fff" letter-spacing="-2">${inv}<tspan font-size="17" dy="-24" fill="${T2}">%</tspan></text><text y="0" text-anchor="middle" font-size="12" font-weight="500" fill="${T3}" font-family="Geist">invested</text>`;
  svg.innerHTML=`<style>@keyframes tk{to{opacity:1}}</style>`+s;
  al.querySelector('.all').innerHTML=[['Stocks',fr.stocks,p.stocks,'stocks'],['Crypto',fr.crypto,p.crypto,'crypto'],['Fiat',fr.fiat,p.fiat,'fiat']].map(([k,f,v,c],i)=>
    `<div class="row" style="height:30px;${i?'border-top:1px solid rgba(255,255,255,.1)':''}"><i style="width:8px;height:8px;border-radius:50%;background:${COL[c]}"></i><span style="flex:1;font-size:13px;font-weight:600">${k}</span><span class="mono muted" style="width:44px;text-align:right">${Math.round(f*100)}%</span><span class="mono" style="width:76px;text-align:right;font-size:13px">${money(v)}</span></div>`).join('');
}

/* ---- KPI: return (solid accent tile) ---- */
const ret=$id('ret');
ret.innerHTML=`${bg(GR.lime)}<div class="in-c"><div class="row" style="justify-content:space-between"><div class="cap">Portfolio return</div><span class="chip rtc">+$0</span></div>
  <div style="position:absolute;left:22px;bottom:32px;display:flex;align-items:center;gap:8px"><svg width="30" height="36" viewBox="0 0 34 40" aria-hidden="true"><path d="M17 37V5M5 17L17 5l12 12" fill="none" stroke="#fff" stroke-width="2.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
    <span class="big rtv" style="font-size:52px">0%</span></div>
  <div class="muted rtp" style="position:absolute;left:24px;bottom:14px;font-size:11.5px"></div></div><div class="rim"></div>`;
function updateRet(){const D=nwData,inv0=D[0].stocks+D[0].crypto,inv1=D[D.length-1].stocks+D[D.length-1].crypto,r=(inv1-inv0)/inv0*100;
  tween(ret.querySelector('.rtv'),r,v=>v.toFixed(1)+'%');ret.querySelector('.rtc').textContent='+'+money(inv1-inv0,{compact:true});
  ret.querySelector('.rtp').textContent='Stocks + crypto · '+({'1M':'last month','3M':'last 3 months','1Y':'last 12 months','All':'last 3 years'})[S.period]}

/* ---- KPI: free cash with runway ticks ---- */
const cash=$id('cash');
cash.innerHTML=`<div class="in-c"><div class="row" style="justify-content:space-between"><div class="cap">Free cash</div><span class="muted">Runway</span></div>
  <div class="row" style="margin-top:14px;align-items:baseline;justify-content:space-between"><span class="big csv" style="font-size:38px">0</span><span class="mono" style="font-size:20px;font-weight:500"><span class="crw">0</span><span class="muted" style="font-size:12px"> mo</span></span></div>
  <svg class="cst" width="100%" height="22" style="position:absolute;left:24px;right:24px;bottom:22px;width:calc(100% - 48px)" aria-hidden="true"></svg></div>`;
function updateCash(){const v=nwData[nwData.length-1].fiat*.42,mo=v/2100;tween(cash.querySelector('.csv'),v,x=>money(x));tween(cash.querySelector('.crw'),mo,x=>x.toFixed(1));
  const svg=cash.querySelector('.cst'),W=svg.getBoundingClientRect().width||220,n=36;let s='';
  for(let i=0;i<n;i++){const x=1+i*(W-2)/(n-1),on=i/(n-1)<=mo/12;s+=`<line x1="${x}" x2="${x}" y1="${i%6===0?2:8}" y2="20" stroke="${on?ACC:'rgba(255,255,255,.28)'}" stroke-width="1.6" stroke-linecap="round" style="transition:stroke .2s ${i*14}ms"/>`}
  svg.innerHTML=s+`<text x="${W}" y="0" dy="-4" text-anchor="end" font-size="10" fill="${T3}" font-family="Geist Mono">12 mo</text>`}

/* ---- KPI: upcoming payments ---- */
const pay=$id('pay');
const PAY=data.payments.map(p=>[p.name,p.date,p.amount,p.name.charAt(0)]);
function updatePay(){pay.innerHTML=`<div class="in-c" style="padding-bottom:14px"><div class="row" style="justify-content:space-between"><div class="cap">Next payments</div><span class="mono muted">${money(PAY.reduce((a,p)=>a+p[2],0))}</span></div>
  <div style="margin-top:10px">${PAY.map(([n,d,v,l],i)=>`<div class="row" style="height:34px;${i?'border-top:1px solid rgba(255,255,255,.08)':''}"><i style="width:24px;height:24px;border-radius:8px;background:rgba(255,255,255,.1);display:grid;place-items:center;font:600 11px Geist;font-style:normal">${l}</i><span style="flex:1;font-size:13px;font-weight:600;white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${n}</span><span class="muted mono" style="font-size:11.5px">${d}</span><span class="mono" style="width:62px;text-align:right;font-size:13px">${money(v)}</span></div>`).join('')}</div></div>`}

/* ---- KPI: goals ---- */
const goals=$id('goals');
const GO=data.goals.map(g=>[g.name,g.progress,g.target]);
function updateGoals(){goals.innerHTML=`<div class="in-c"><div class="row" style="justify-content:space-between"><div class="cap">Goals</div><span class="muted">2 active</span></div>
  ${GO.map(([n,f,v],k)=>`<div style="margin-top:${k?14:16}px"><div class="row" style="justify-content:space-between;font-size:13px;font-weight:600"><span>${n}</span><span class="mono" style="font-weight:500">${Math.round(f*100)}%<span class="muted"> of ${money(v,{compact:true})}</span></span></div>
    <svg width="100%" height="12" style="margin-top:8px;display:block" class="gl" data-f="${f}" aria-hidden="true"></svg></div>`).join('')}</div>`;
  RAF(()=>goals.querySelectorAll('.gl').forEach((svg,k)=>{const W=svg.getBoundingClientRect().width,n=40,f=+svg.dataset.f;let s='';
    for(let i=0;i<n;i++){const x=1+i*(W-2)/(n-1),on=i/(n-1)<=f;s+=`<line x1="${x}" x2="${x}" y1="${i%8===0?0:4}" y2="12" stroke="${on?ACC:'rgba(255,255,255,.28)'}" stroke-width="1.6" stroke-linecap="round" style="opacity:0;animation:tk2 .3s ${300+k*150+i*12}ms forwards"/>`}
    svg.innerHTML='<style>@keyframes tk2{to{opacity:1}}</style>'+s}))}

/* ---- Cash flow: mirrored waves, income above, spending below ---- */
const flow=$id('flow');
flow.innerHTML=`${bg(GR.pink)}<div class="in-c">
  <div class="row" style="justify-content:space-between"><div><div class="ttl">Cash flow</div><div class="muted" style="margin-top:4px">Net <span class="mono" style="color:${ACC}">+$3,240</span> a month</div></div>
    <div class="legend" style="flex-direction:column;gap:4px;align-items:flex-end"><span><i style="background:#fff"></i>Income</span><span><i style="background:#FFD0E6"></i>Spending</span></div></div>
  <svg class="fls" style="position:absolute;left:0;bottom:12px" aria-hidden="true"></svg></div><div class="rim"></div>`;
const MLAB=data.cashflow.months;
const INC=data.cashflow.income;
const SPN=data.cashflow.spending;
let flowRaf=0;
function updateFlow(instant){
  const svg=flow.querySelector('.fls'),W=flow.clientWidth,Hh=246,L=44,R=W-36,mid=Hh/2,lab=16,amp=mid-lab-8,max=7;
  svg.setAttribute('width',W);svg.setAttribute('height',Hh);svg.setAttribute('viewBox',`0 0 ${W} ${Hh}`);
  const X=i=>L+i/(MLAB.length-1)*(R-L);
  cancelAnimationFrame(flowRaf);const t0=instant?-1e6:performance.now();
  const frame=now=>{const g=ease(reduce?1:Math.min(1,(now-t0)/1200));
    const path=(arr,dir)=>smoothPath(arr.map((v,i)=>[X(i),mid+dir*(lab+v/max*amp*g)]));
    let s='';
    [0,2,4,6].forEach(v=>{const u=v/RATE[S.ccy],yT=mid-lab-u/max*amp,yB=mid+lab+u/max*amp;
      s+=`<line x1="${L}" x2="${R}" y1="${yT}" y2="${yT}" stroke="${HAIR}"/><line x1="${L}" x2="${R}" y1="${yB}" y2="${yB}" stroke="${HAIR}"/>
        <text x="${L-12}" y="${yT+3.5}" text-anchor="end" font-size="10" fill="${T3}" font-family="Geist Mono">${v?SYM[S.ccy]+v+'K':'0'}</text><text x="${L-12}" y="${yB+3.5}" text-anchor="end" font-size="10" fill="${T3}" font-family="Geist Mono">${v?SYM[S.ccy]+v+'K':'0'}</text>
        <line x1="${R+8}" x2="${R+14}" y1="${yT}" y2="${yT}" stroke="${T3}"/><line x1="${R+8}" x2="${R+14}" y1="${yB}" y2="${yB}" stroke="${T3}"/>`});
    MLAB.forEach((m,i)=>s+=`<text x="${X(i)}" y="${mid+4}" text-anchor="middle" font-size="10.5" font-weight="500" fill="rgba(255,255,255,.75)" font-family="Geist Mono">${m}</text>`);
    INC.slice().reverse().forEach((a,k)=>{const j=2-k;s+=`<path d="${path(a,-1)}" fill="none" stroke="#fff" stroke-opacity="${[1,.55,.3][j]}" stroke-width="${j?1.6:2.4}" stroke-linecap="round"/>`});
    SPN.slice().reverse().forEach((a,k)=>{const j=2-k;s+=`<path d="${path(a,1)}" fill="none" stroke="#FFD0E6" stroke-opacity="${[1,.55,.3][j]}" stroke-width="${j?1.6:2.4}" stroke-linecap="round"/>`});
    s+=`<line class="hl" y1="10" y2="${Hh-10}" stroke="#fff" stroke-opacity=".5" opacity="0"/><rect class="hit" x="${L-10}" width="${R-L+20}" height="${Hh}" fill="transparent"/>`;
    svg.innerHTML=s;
    const hit=svg.querySelector('.hit'),hl=svg.querySelector('.hl');
    hit.onpointermove=e=>{const r=svg.getBoundingClientRect(),z=r.width/W,x=(e.clientX-r.left)/z,i=Math.max(0,Math.min(5,Math.round((x-L)/(R-L)*5)));
      hl.setAttribute('x1',X(i));hl.setAttribute('x2',X(i));hl.setAttribute('opacity',1);
      showTip(r.left+X(i)*z,r.top+(mid-lab-INC[0][i]/max*amp)*z,MLAB[i]+' 2026',[['Income',money(INC[0][i]*1000),'#fff'],['Spending',money(SPN[0][i]*1000),'#FFD0E6'],['Saved',money((INC[0][i]-SPN[0][i])*1000)]])};
    hit.onpointerleave=()=>{hl.setAttribute('opacity',0);hideTip()};
    if(g<1)flowRaf=RAF(frame)};
  flowRaf=RAF(frame);
}

/* ---- Crypto: a coin rising, assets with sparklines ---- */
const cr=$id('crypto');
const CA=data.crypto.map(a=>[a.name,a.symbol,a.share,'',[]]);
cr.innerHTML=`${bg(GR.copper)}<div class="in-c"><div class="row" style="justify-content:space-between"><div class="ttl">Crypto</div><span class="muted">7 days</span></div>
  <div class="row" style="margin-top:14px;align-items:flex-end;justify-content:space-between"><span class="big crv">0</span><span style="padding-bottom:4px"><span class="delta crd"></span><span class="muted"> 24h</span></span></div>
  <div class="crb" style="margin-top:16px"></div>
  <div class="crl" style="position:absolute;left:24px;right:24px;bottom:14px"></div></div><div class="rim"></div>`;
/* 7-day close line with a soft wash; each asset keeps its own shape */
const OHLC=Object.fromEntries(data.crypto.map(a=>[a.symbol,a.ohlc]));
function candles(t){const d=OHLC[t].map(x=>x[3]),W=96,H=30,lo=Math.min(...d),hi=Math.max(...d),pad=(hi-lo)*.15||1,Y=v=>H-3-(v-lo+pad)/(hi-lo+2*pad)*(H-6);
  const P=d.map((v,i)=>[2+i/(d.length-1)*(W-6),Y(v)]),e=P[P.length-1],up=d[d.length-1]>=d[0],gid='sg'+t;
  return `<svg width="${W}" height="${H}" viewBox="0 0 ${W} ${H}" aria-hidden="true"><defs><linearGradient id="${gid}" x1="0" x2="0" y1="0" y2="1"><stop offset="0" stop-color="#fff" stop-opacity=".16"/><stop offset="1" stop-color="#fff" stop-opacity="0"/></linearGradient></defs>
    <path d="${smoothPath(P)}L${e[0]},${H}L${P[0][0]},${H}Z" fill="url(#${gid})"/><path d="${smoothPath(P)}" fill="none" stroke="#fff" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
    <circle cx="${e[0]}" cy="${e[1]}" r="3" fill="${up?ACC:'#fff'}" stroke="rgba(0,0,0,.35)" stroke-width="1"/></svg>`}
const CHG=data.crypto.map(a=>a.change);
function updateCrypto(){const v=nwData[nwData.length-1].crypto;tween(cr.querySelector('.crv'),v,x=>money(x));cr.querySelector('.crd').textContent='↑ 2.4%';
  /* one thin segmented bar shows the split; tones of white, no colour dots */
  cr.querySelector('.crb').innerHTML=`<div style="display:flex;gap:3px;height:6px">${CA.map(([n,s,f],i)=>`<div style="flex:0 0 calc(${f*100}% - 2px);border-radius:3px;background:rgba(255,255,255,${[.95,.6,.32][i]});transform-origin:left;transform:scaleX(0);transition:transform .8s var(--ease) ${.1+i*.1}s" class="crs"></div>`).join('')}</div>
    <div style="display:flex;gap:3px;margin-top:7px">${CA.map(([n,s,f])=>`<span class="mono muted" style="flex:0 0 calc(${f*100}% - 2px);font-size:11px;white-space:nowrap">${s} ${Math.round(f*100)}%</span>`).join('')}</div>`;
  RAF(()=>RAF(()=>cr.querySelectorAll('.crs').forEach(b=>b.style.transform='scaleX(1)')));
  cr.querySelector('.crl').innerHTML=CA.map(([n,s,f,c,sp],i)=>`<div class="row" style="height:50px;${i?'border-top:1px solid rgba(255,255,255,.1)':''}">
    <div style="flex:1;min-width:0"><div style="font-size:13.5px;font-weight:600">${n}</div><div class="mono muted" style="font-size:11px">${s} · 7d</div></div>${candles(s)}
    <div style="width:80px;text-align:right"><div class="mono" style="font-size:13px">${money(v*f)}</div><div class="mono" style="font-size:11px;color:${CHG[i].startsWith('+')?ACC:T3}">${CHG[i]}</div></div></div>`).join('')}

/* ---- Stocks: capsule columns per weekday ---- */
const st=$id('stocks');
const WK=data.stocks.days,WV=data.stocks.values;
st.innerHTML=`${bg(GR.green)}<div class="in-c"><div class="ttl">Stocks</div>
  <div style="margin-top:14px"><span class="big stv" style="font-size:34px">0</span></div><div class="row" style="margin-top:6px;gap:8px"><span class="delta">↑ 1.8%</span><span class="muted">this week</span></div>
  <svg class="sts" style="position:absolute;left:18px;bottom:14px" aria-hidden="true"></svg></div><div class="rim"></div>`;
function updateStocks(instant){const v=nwData[nwData.length-1].stocks;if(!instant)tween(st.querySelector('.stv'),v,x=>money(x));
  const svg=st.querySelector('.sts'),W=st.clientWidth-36,Hh=160,pb=18,bw=22,max=5,gap=(W-24-bw*5)/4;svg.setAttribute('width',W);svg.setAttribute('height',Hh);
  const Y=x=>(Hh-pb)*(1-x/max);let s='';
  [0,2,4].forEach(t=>s+=`<text x="0" y="${Y(t)+3.5}" font-size="9.5" fill="${T3}" font-family="Geist Mono">${t}K</text>`);
  WV.forEach((x,i)=>{const X=24+i*(bw+gap);const today=i===WV.length-1;
    s+=`<rect x="${X}" y="${Y(max)}" width="${bw}" height="${Hh-pb-Y(max)}" rx="${bw/2}" fill="rgba(0,0,0,.28)"/>
      <rect class="sb" x="${X}" y="${Hh-pb}" width="${bw}" height="0" rx="${bw/2}" fill="${today?ACC:'#fff'}" data-y="${Y(x)}" data-h="${Hh-pb-Y(x)}" style="transition:y .8s var(--ease) ${i*70}ms,height .8s var(--ease) ${i*70}ms"/>
      <text x="${X+bw/2}" y="${Hh-3}" text-anchor="middle" font-size="10" fill="${today?'#fff':T3}" font-family="Geist Mono">${WK[i]}</text>
      <rect class="hit" x="${X-4}" y="0" width="${bw+8}" height="${Hh}" fill="transparent" data-i="${i}"/>`});
  svg.innerHTML=s;
  RAF(()=>RAF(()=>svg.querySelectorAll('.sb').forEach(b=>{b.setAttribute('y',b.dataset.y);b.setAttribute('height',b.dataset.h)})));
  svg.querySelectorAll('.hit').forEach(h=>{h.onpointermove=e=>{const i=+h.dataset.i;showTip(e.clientX,e.clientY,WK[i]+' · trading volume',[['Bought',money(WV[i]*1000*.64),'#fff'],['Sold',money(WV[i]*1000*.36)]])};h.onpointerleave=hideTip});
}

/* ---------- controls ---------- */
function seg(host,items,key){host.innerHTML=items.map(k=>`<button type="button" aria-pressed="${S[key]===k}">${k}</button>`).join('');
  host.querySelectorAll('button').forEach(b=>b.onclick=()=>{S[key]=b.textContent;host.querySelectorAll('button').forEach(x=>x.setAttribute('aria-pressed',x===b));updateAll()})}
seg($id('period'),['1M','3M','1Y','All'],'period');
seg($id('ccy'),['USD','EUR'],'ccy');
function updateAll(){updateNW();updateAlloc();updateRet();updateCash();updatePay();updateGoals();updateFlow();updateCrypto();updateStocks()}

let rz;const ro=new ResizeObserver(()=>{clearTimeout(rz);rz=setTimeout(()=>{if(!nwData)return;cancelAnimationFrame(nwRaf);nwGrow=1;drawNW();updateFlow(true);updateStocks(true);updateCash();updateGoals()},120)});
['nw','flow','stocks','cash'].forEach(id=>ro.observe($id(id)));
const vbm=$id('vbm');const ON=new Set(['0,0','4,0','1,1','3,1','2,2']);
if(vbm)for(let i=0;i<15;i++){const x=i%5,y=Math.floor(i/5),e=document.createElement('i');if(ON.has(x+','+y))e.className='on';e.style.setProperty('--x',x);vbm.appendChild(e)}
(document.fonts?document.fonts.ready:Promise.resolve()).then(()=>{if(!alive)return;$id('grid').classList.add('in');
  root.querySelectorAll('.card').forEach((c,i)=>c.style.transitionDelay=(i*60)+'ms');updateAll()});

  return () => { alive = false; frames.forEach((f) => cancelAnimationFrame(f)); ro.disconnect(); clearTimeout(rz); };
}

export function FinanceBento({ data, title = "Overview", subtitle = "", avatar, avatarAlt = "Profile", credit = true, className = "", style }) {
  const ref = React.useRef(null);
  React.useEffect(() => {
    const el = ref.current;
    if (!el) return;
    el.innerHTML = markup({ title, subtitle, avatar, avatarAlt, credit });
    const stop = mount(el, data);
    return () => { stop(); el.innerHTML = ""; };
  }, [data, title, subtitle, avatar, avatarAlt, credit]);
  return (
    <>
      <style>{CSS}</style>
      <div ref={ref} className={`fpb ${className}`} style={style} />
    </>
  );
}

export default FinanceBento;


demo.tsx
"use client";

import * as React from "react";
import { FinanceBento } from "@/components/ui/finance-bento";

/* Geist + Geist Mono for the demo */
function useFonts() {
  React.useEffect(() => {
    const id = "geist-fonts";
    if (document.getElementById(id)) return;
    const l = document.createElement("link");
    l.id = id;
    l.rel = "stylesheet";
    l.href = "https://fonts.googleapis.com/css2?family=Geist:wght@200;300;400;500;600;700&family=Geist+Mono:wght@400;500;600&display=swap";
    document.head.appendChild(l);
  }, []);
}

/* 36 month-end balances that grow with some honest wobble */
const history = Array.from({ length: 36 }, (_, i) => {
  const t = i / 35;
  const d = new Date(2026, 9 - (35 - i), 1);
  const noise = (Math.sin(i * 12.9898 + 156.466) * 43758.5453) % 1;
  return {
    date: d.toISOString(),
    fiat: 14000 + t * 7000 + Math.sin(i * 0.9) * 900,
    stocks: 12000 + t * 19000 + Math.sin(i * 0.55) * 1600 + noise * 800,
    crypto: Math.max(2500, 4000 + t * 12000 + Math.sin(i * 0.8 + 1) * 3200 + Math.cos(i * 0.33) * 1400),
  };
});

const data = {
  history,
  payments: [
    { name: "Rent", date: "Nov 1", amount: 1800 },
    { name: "Index fund · auto-buy", date: "Nov 3", amount: 500 },
    { name: "Insurance", date: "Nov 9", amount: 96 },
  ],
  goals: [
    { name: "House deposit", progress: 0.42, target: 60000 },
    { name: "Japan, spring", progress: 0.76, target: 6000 },
  ],
  cashflow: {
    months: ["May", "Jun", "Jul", "Aug", "Sep", "Oct"],
    income: [[2.4, 3.6, 5.4, 6.4, 5.0, 3.4], [2.0, 3.0, 4.4, 5.2, 4.2, 2.8], [1.6, 2.4, 3.4, 4.0, 3.3, 2.2]],
    spending: [[1.2, 2.2, 3.8, 4.6, 3.4, 1.8], [1.0, 1.8, 3.0, 3.8, 2.8, 1.5], [0.8, 1.4, 2.4, 3.0, 2.2, 1.2]],
  },
  crypto: [
    { name: "Bitcoin", symbol: "BTC", share: 0.62, change: "+3.1%", ohlc: [[66.1, 67.0, 65.8, 66.8], [66.8, 67.6, 66.5, 67.4], [67.4, 67.9, 66.9, 67.1], [67.1, 68.2, 67.0, 68.0], [68.0, 68.9, 67.8, 68.7], [68.7, 69.1, 68.1, 68.4], [68.4, 69.6, 68.3, 69.4]] },
    { name: "Ethereum", symbol: "ETH", share: 0.26, change: "−0.8%", ohlc: [[3.52, 3.58, 3.44, 3.47], [3.47, 3.55, 3.4, 3.53], [3.53, 3.56, 3.38, 3.41], [3.41, 3.5, 3.36, 3.48], [3.48, 3.51, 3.39, 3.43], [3.43, 3.49, 3.37, 3.46], [3.46, 3.52, 3.42, 3.44]] },
    { name: "Solana", symbol: "SOL", share: 0.12, change: "+6.4%", ohlc: [[142, 149, 139, 147], [147, 151, 141, 143], [143, 156, 142, 154], [154, 158, 147, 149], [149, 162, 148, 160], [160, 166, 155, 157], [157, 171, 156, 168]] },
  ],
  stocks: { days: ["Mon", "Tue", "Wed", "Thu", "Fri"], values: [2.1, 1.2, 3.4, 1.9, 4.6] },
};

const AVATAR = "data:image/jpeg;base64,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";

export default function FinanceBentoDemo() {
  useFonts();
  return (
    <div className="min-h-screen w-full bg-black px-4 py-6 sm:px-6 sm:py-8">
      <FinanceBento
        className="mx-auto max-w-[1320px]"
        data={data}
        title="Overview"
        subtitle="Updated today, 09:41"
        avatar={AVATAR}
        avatarAlt="Roman Vasnev"
      />
    </div>
  );
}

```


![[Screenshot 2026-10-25 at 2.55.12 PM.png]]

https://21st.dev/community/components?preview=%2F%40romanvasnev%2Fcomponents%2Ffinance-bento