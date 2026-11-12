<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Zahlen-Alarm – Kopfrechnen in Bewegung (Copy)</title>
<style>
:root{
  --bg:#0b1220; --panel:#16213a; --panel2:#1f2d4d; --text:#f8fafc; --muted:#cbd5e1;
  --a:#f87171; --b:#60a5fa; --c:#fbbf24; --ok:#4ade80; --warn:#fb923c; --term:#fde047;
  --r:18px;
}
*{box-sizing:border-box}
html,body{margin:0;height:100%}
body{background:var(--bg);color:var(--text);font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;line-height:1.35;display:flex;flex-direction:column;min-height:100vh}
button,select,input{font:inherit;color:inherit}
button{cursor:pointer}
:focus-visible{outline:4px solid var(--term);outline-offset:2px}
[hidden]{display:none!important}

/* Kopfleiste */
.topbar{display:flex;align-items:center;gap:.5rem;padding:.5rem 1rem;background:#0a1020;border-bottom:2px solid var(--panel2)}
.brand{font-weight:800;letter-spacing:.04em;font-size:1.1rem;margin-right:auto}
.brand span{color:var(--term)}
.ib{background:var(--panel);border:2px solid var(--panel2);border-radius:12px;padding:.45rem .8rem;font-size:1rem;min-height:44px}
.ib:hover{border-color:var(--muted)}
.ib[aria-pressed="false"]{opacity:.6}

main{flex:1;display:flex;flex-direction:column;padding:1rem;max-width:1500px;width:100%;margin:0 auto}

/* Buttons */
.btn{background:var(--panel2);border:3px solid var(--muted);border-radius:var(--r);padding:.8rem 1.4rem;font-weight:800;font-size:1.15rem;min-height:52px}
.btn:hover{background:#2a3d68}
.btn.primary{background:var(--ok);color:#052e16;border-color:#bbf7d0;font-size:clamp(1.3rem,3vw,2rem);padding:.9rem 2rem}
.btn.primary:hover{background:#86efac}
.btn.danger{background:#f87171;color:#450a0a;border-color:#fecaca}
.btn.small{font-size:.95rem;padding:.4rem .9rem;min-height:44px}

/* Setup */
.setup h1{font-size:clamp(2.2rem,6vw,4.2rem);margin:.2em 0 0;line-height:1}
.setup h1 span{color:var(--term)}
.setup .lead{font-size:clamp(1.1rem,2.2vw,1.5rem);color:var(--muted);margin:.4rem 0 1.2rem}
.cols{display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:1rem}
fieldset{border:2px solid var(--panel2);border-radius:var(--r);background:var(--panel);padding:1rem 1.1rem;margin:0}
legend{font-weight:800;padding:0 .5rem;color:var(--term);font-size:1.05rem}
.opt{display:flex;gap:.7rem;align-items:flex-start;padding:.45rem .3rem;border-radius:10px;cursor:pointer}
.opt:hover{background:var(--panel2)}
.opt input{width:1.4rem;height:1.4rem;margin-top:.15rem;accent-color:var(--term);flex:none}
.opt small{display:block;color:var(--muted)}
.row{display:flex;gap:.8rem;align-items:center;flex-wrap:wrap;margin:.4rem 0}
select{background:var(--panel2);border:2px solid var(--muted);border-radius:10px;padding:.4rem .6rem;min-height:44px}
input[type=range]{width:min(100%,360px);accent-color:var(--term);height:32px}
.actions{display:flex;gap:.8rem;flex-wrap:wrap;margin:1.2rem 0;align-items:center}
details{background:var(--panel);border:2px solid var(--panel2);border-radius:var(--r);padding:.8rem 1.1rem;margin-top:.5rem}
summary{cursor:pointer;font-weight:800;font-size:1.1rem}
details ol,details ul{margin:.5rem 0 0;padding-left:1.4rem}
details li{margin:.3rem 0}

/* Spielscreens */
.screen{flex:1;display:flex;flex-direction:column;gap:1rem;justify-content:center}
.hud{display:flex;gap:.8rem;align-items:center;flex-wrap:wrap}
.pill{background:var(--panel);border:2px solid var(--panel2);border-radius:999px;padding:.4rem 1rem;font-weight:800;font-size:clamp(1rem,2vw,1.4rem)}
.pill.chaos{background:#4c1d95;border-color:#c4b5fd}
.pill.move{background:#134e4a;border-color:#5eead4}
.spacer{flex:1}
.tnum{font-variant-numeric:tabular-nums;font-weight:900;font-size:clamp(2.4rem,7vw,5.5rem);line-height:1;min-width:2ch;text-align:right}
.track{height:20px;background:var(--panel);border-radius:999px;overflow:hidden;border:2px solid var(--panel2)}
.fill{height:100%;width:100%;background:var(--ok);border-radius:999px}
.question{font-size:clamp(1.8rem,5.2vw,4.4rem);font-weight:800;line-height:1.15;text-align:center;padding:1rem .5rem;text-wrap:balance}
.question.small{font-size:clamp(1.3rem,3vw,2.4rem)}
mark.term{background:transparent;color:var(--term);border-bottom:.1em solid var(--term)}
.cards{display:grid;grid-template-columns:repeat(3,1fr);gap:1rem}
.card{--c:var(--a);background:var(--panel);border:6px solid var(--c);border-radius:24px;padding:1rem;text-align:center;display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:clamp(150px,26vh,320px)}
.card.A{--c:var(--a)}.card.B{--c:var(--b)}.card.C{--c:var(--c)}
.card .tag{font-size:clamp(1.4rem,3vw,2.6rem);font-weight:900;color:var(--c)}
.card .val{font-size:clamp(2.6rem,9vw,7.5rem);font-weight:900;line-height:1.05;font-variant-numeric:tabular-nums}
.card.right{background:#14532d;border-color:var(--ok);box-shadow:0 0 0 8px rgba(74,222,128,.35)}
.card.dim{opacity:.35}
.big{font-size:clamp(2.6rem,10vw,8rem);font-weight:900;line-height:1;text-align:center}
.center{text-align:center}
.muted{color:var(--muted)}
.box{background:var(--panel);border:3px solid var(--panel2);border-radius:var(--r);padding:1rem 1.3rem;font-size:clamp(1.1rem,2.2vw,1.7rem)}
.box.word{border-color:var(--term)}
.box.fit{border-color:var(--warn)}
.box.chaosbox{border-color:#c4b5fd;background:#2e1065}
.box b{color:var(--term)}
.shout{font-size:clamp(1.6rem,4vw,3rem);font-weight:900;color:var(--term);letter-spacing:.04em}
.two{display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:1rem}
.teams{display:flex;gap:.8rem;flex-wrap:wrap;justify-content:center}
.team{--tc:#f472b6;display:flex;align-items:center;gap:.6rem;background:var(--panel);border:4px solid var(--tc);border-radius:16px;padding:.4rem .8rem;font-weight:800;font-size:1.2rem}
.team strong{font-size:2rem;min-width:2ch;text-align:center;font-variant-numeric:tabular-nums}
.team button{background:var(--panel2);border:2px solid var(--muted);border-radius:10px;min-width:48px;min-height:48px;font-weight:900;font-size:1.3rem}

.urgent .tnum{color:var(--a)}
.urgent .fill{background:var(--a)}
.urgent{animation:pulse .5s ease-in-out infinite alternate}
@keyframes pulse{from{box-shadow:inset 0 0 0 0 rgba(248,113,113,0)}to{box-shadow:inset 0 0 0 14px rgba(248,113,113,.45)}}
.stopscreen .big{color:var(--a)}
@media (max-width:760px){.cards{grid-template-columns:1fr}.card{min-height:110px;flex-direction:row;gap:1rem;justify-content:space-between;padding:.6rem 1.2rem}}
@media (prefers-reduced-motion:reduce){*{animation:none!important;transition:none!important}}

/* Overlay */
#overlay{position:fixed;inset:0;background:rgba(2,6,23,.92);z-index:50;overflow:auto;padding:1rem}
.sheet{max-width:1100px;margin:0 auto;background:var(--panel);border:3px solid var(--panel2);border-radius:var(--r);padding:1rem 1.3rem}
.sheet h2{margin:.2rem 0 .8rem}
.gl{display:grid;grid-template-columns:repeat(auto-fill,minmax(300px,1fr));gap:.8rem}
.gl div{background:var(--panel2);border-radius:14px;padding:.7rem .9rem}
.gl b{color:var(--term);font-size:1.25rem}
.gl i{color:var(--muted);display:block;margin-top:.2rem}
.signs{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:1rem}
.sign{--c:var(--a);border:10px solid var(--c);border-radius:28px;background:#fff;color:#111;text-align:center;padding:1rem;aspect-ratio:1/1.15;display:flex;flex-direction:column;justify-content:center}
.sign.A{--c:#ef4444}.sign.B{--c:#3b82f6}.sign.C{--c:#f59e0b}
.sign .l{font-size:clamp(6rem,22vw,13rem);font-weight:900;line-height:1;color:var(--c)}
.sign .s{font-size:clamp(3rem,10vw,6rem);color:var(--c);line-height:1}
.sr{position:absolute;left:-9999px}
</style>
</head>
<body>
<header class="topbar">
  <div class="brand">ZAHLEN-<span>ALARM</span></div>
  <button class="ib" id="btnSound" data-act="sound" aria-pressed="true" title="Ton an/aus">🔊 Ton</button>
  <button class="ib" data-act="glossary" title="Mathe-Wortschatz anzeigen">📖 Wortschatz</button>
  <button class="ib" data-act="full" title="Vollbild">⛶ Vollbild</button>
  <button class="ib" id="btnHome" data-act="home" hidden>🏠 Beenden</button>
</header>
<main id="app"></main>
<div id="overlay" role="dialog" aria-modal="true" aria-label="Zusatzansicht" hidden></div>

<script>
"use strict";
/*GEN-START*/
/* ===== Aufgabengenerator (Mathe-Vokabular + Rechenarten) ===== */
const rnd=(a,b)=>Math.floor(Math.random()*(b-a+1))+a;
const pick=a=>a[rnd(0,a.length-1)];
const shuffle=a=>{a=a.slice();for(let i=a.length-1;i>0;i--){const j=rnd(0,i);[a[i],a[j]]=[a[j],a[i]];}return a;};
const f=n=>n.toLocaleString('de-DE');
const m=t=>`<mark class="term">${t}</mark>`;

const GLOSSARY={
  summe:{w:'Summe',d:'Das Ergebnis einer Addition (Plusaufgabe).',ex:'3 + 4 = 7 → 7 ist die Summe.'},
  summand:{w:'Summand',d:'Eine Zahl, die bei einer Addition zusammengezählt wird.',ex:'In 3 + 4 sind 3 und 4 die Summanden.'},
  addieren:{w:'addieren',d:'Zusammenzählen, plus rechnen.',ex:'5 addieren und 2 addieren = 7.'},
  differenz:{w:'Differenz',d:'Das Ergebnis einer Subtraktion (Minusaufgabe).',ex:'9 − 4 = 5 → 5 ist die Differenz.'},
  subtrahieren:{w:'subtrahieren',d:'Abziehen, minus rechnen.',ex:'Subtrahiere 4 von 9 → 5.'},
  produkt:{w:'Produkt',d:'Das Ergebnis einer Multiplikation (Malaufgabe).',ex:'3 × 4 = 12 → 12 ist das Produkt.'},
  faktor:{w:'Faktor',d:'Eine Zahl, die bei einer Multiplikation mit einer anderen malgenommen wird.',ex:'In 3 × 4 sind 3 und 4 die Faktoren.'},
  multiplizieren:{w:'multiplizieren',d:'Malnehmen.',ex:'Multipliziere 3 mit 4 → 12.'},
  quotient:{w:'Quotient',d:'Das Ergebnis einer Division (Geteiltaufgabe).',ex:'12 : 4 = 3 → 3 ist der Quotient.'},
  dividieren:{w:'dividieren',d:'Teilen, geteilt rechnen.',ex:'Dividiere 12 durch 4 → 3.'},
  doppelte:{w:'das Doppelte',d:'Eine Zahl mal 2 genommen.',ex:'Das Doppelte von 6 ist 12.'},
  haelfte:{w:'die Hälfte',d:'Eine Zahl durch 2 geteilt.',ex:'Die Hälfte von 12 ist 6.'},
  dreifache:{w:'das Dreifache',d:'Eine Zahl mal 3 genommen.',ex:'Das Dreifache von 5 ist 15.'},
  nachfolger:{w:'Nachfolger',d:'Die Zahl direkt nach einer Zahl (eins mehr).',ex:'Der Nachfolger von 19 ist 20.'},
  vorgaenger:{w:'Vorgänger',d:'Die Zahl direkt vor einer Zahl (eins weniger).',ex:'Der Vorgänger von 20 ist 19.'},
  gerade:{w:'gerade',d:'Eine Zahl, die sich ohne Rest durch 2 teilen lässt. Sie endet auf 0, 2, 4, 6 oder 8.',ex:'14 ist gerade, 15 nicht.'},
  ungerade:{w:'ungerade',d:'Eine Zahl, die sich nicht ohne Rest durch 2 teilen lässt. Sie endet auf 1, 3, 5, 7 oder 9.',ex:'15 ist ungerade.'},
  zwischen:{w:'zwischen',d:'Größer als die kleinere und kleiner als die größere Zahl.',ex:'12 liegt zwischen 11 und 13.'},
  vielfaches:{w:'Vielfaches',d:'Ein Ergebnis aus der Einmaleins-Reihe einer Zahl.',ex:'Vielfache von 3: 3, 6, 9, 12 …'},
  quersumme:{w:'Quersumme',d:'Die Summe aller Ziffern einer Zahl.',ex:'Quersumme von 47: 4 + 7 = 11.'}
};

/* Falsche Antworten: nahe liegende, aber falsche Zahlen */
function pickDistractors(ans,extras,near){
  const set=new Set();
  const add=x=>{ if(Number.isInteger(x)&&x>=0&&x!==ans) set.add(x); };
  (near||[1,2,10]).forEach(d=>{ if(d===100&&ans<200)return; add(ans-d); add(ans+d); });
  (extras||[]).forEach(add);
  let c=shuffle([...set]);
  let k=3;
  while(c.length<2){ const x=ans+k++; if(!c.includes(x)&&x!==ans) c.push(x); }
  return c.slice(0,2);
}
function finish(kind,terms,html,ans,explain,distractors){
  const opts=shuffle([ans,...distractors]);
  return {kind,terms,html,ans,explain,options:opts.map(f),correct:opts.indexOf(ans)};
}
function numQ(kind,terms,html,ans,explain,extras,near){
  return finish(kind,terms,html,ans,explain,pickDistractors(ans,extras,near));
}
function sumNums(L){
  let a,b;
  if(L===1){a=rnd(1,19);b=rnd(1,20-a);}
  else if(L===2){a=rnd(10,80);b=rnd(5,100-a);}
  else if(L===3){a=rnd(20,150);b=rnd(20,150);}
  else {a=rnd(100,700);b=rnd(50,999-a);}
  return [a,b];
}
function facNums(L){
  if(L===1) return pick([[2,rnd(2,10)],[5,rnd(2,4)],[10,2]]);
  if(L===2) return [pick([2,3,4,5,10]),rnd(2,10)];
  if(L===3) return [rnd(2,10),rnd(2,10)];
  return [rnd(3,12),rnd(4,12)];
}

const G={
  sum(L){
    const [a,b]=sumNums(L), s=a+b, v=rnd(0,3);
    const html=[`Die ${m('Summe')} aus ${f(a)} und ${f(b)}`,
      `${m('Addiere')} ${f(a)} und ${f(b)}`,
      `Die ${m('Summanden')} sind ${f(a)} und ${f(b)}.<br>Wie groß ist die ${m('Summe')}?`,
      `Berechne die ${m('Summe')}:<br>${f(a)} + ${f(b)}`][v];
    const terms=[['summe'],['addieren'],['summand','summe'],['summe']][v];
    return numQ('sum',terms,html,s,`${f(a)} + ${f(b)} = ${f(s)}`,[s-a,s+b-1]);
  },
  diff(L){
    let a,b;
    if(L===1){a=rnd(3,20);b=rnd(1,a-1);}
    else if(L===2){a=rnd(20,100);b=rnd(5,a-5);}
    else if(L===3){a=rnd(60,300);b=rnd(20,a-10);}
    else {a=rnd(200,999);b=rnd(50,a-20);}
    const d=a-b, v=rnd(0,2);
    const html=[`Die ${m('Differenz')} von ${f(a)} und ${f(b)}`,
      `${m('Subtrahiere')} ${f(b)} von ${f(a)}`,
      `Bilde die ${m('Differenz')}:<br>${f(a)} − ${f(b)}`][v];
    const terms=[['differenz'],['subtrahieren'],['differenz']][v];
    return numQ('diff',terms,html,d,`${f(a)} − ${f(b)} = ${f(d)}`,[a+b]);
  },
  prod(L){
    let a,b;
    if(L<=3){[a,b]=facNums(L); if(Math.random()<.5)[a,b]=[b,a];}
    else if(Math.random()<.5){a=rnd(3,12);b=rnd(6,12);}
    else if(Math.random()<.5){a=rnd(11,99);b=10;}
    else {a=rnd(2,25);b=100;}
    const p=a*b, v=rnd(0,2);
    const html=[`Das ${m('Produkt')} aus ${f(a)} und ${f(b)}`,
      `${m('Multipliziere')} ${f(a)} mit ${f(b)}`,
      `Die ${m('Faktoren')} sind ${f(a)} und ${f(b)}.<br>Wie groß ist das ${m('Produkt')}?`][v];
    const terms=[['produkt'],['multiplizieren'],['faktor','produkt']][v];
    return numQ('prod',terms,html,p,`${f(a)} × ${f(b)} = ${f(p)}`,[p+a,p-a,p+b,p-b,a+b]);
  },
  quot(L){
    let d,q,n;
    if(L===2){d=pick([2,5,10]);q=rnd(2,10);}
    else if(L===3){d=rnd(2,10);q=rnd(2,10);}
    else if(Math.random()<.5){d=rnd(3,12);q=rnd(4,12);}
    else {d=10;q=rnd(11,99);}
    n=d*q; const v=rnd(0,2);
    const html=[`Der ${m('Quotient')} aus ${f(n)} und ${f(d)}`,
      `${m('Dividiere')} ${f(n)} durch ${f(d)}`,
      `Bilde den ${m('Quotienten')}:<br>${f(n)} : ${f(d)}`][v];
    const terms=[['quotient'],['dividieren'],['quotient']][v];
    return numQ('quot',terms,html,q,`${f(n)} : ${f(d)} = ${f(q)}`,[q+1,q-1,d]);
  },
  dbl(L){
    const x=[0,rnd(1,10),rnd(6,50),rnd(20,150),rnd(100,499)][L];
    return numQ('dbl',['doppelte'],`Das ${m('Doppelte')} von ${f(x)}`,2*x,`2 × ${f(x)} = ${f(2*x)}`,[x,2*x+x%10+1]);
  },
  half(L){
    const x=2*[0,rnd(1,10),rnd(3,50),rnd(15,150),rnd(60,499)][L];
    return numQ('half',['haelfte'],`Die ${m('Hälfte')} von ${f(x)}`,x/2,`${f(x)} : 2 = ${f(x/2)}`,[x*2,x/2+1]);
  },
  triple(L){
    const x=[0,0,rnd(2,20),rnd(5,70),rnd(30,300)][L];
    return numQ('triple',['dreifache'],`Das ${m('Dreifache')} von ${f(x)}`,3*x,`3 × ${f(x)} = ${f(3*x)}`,[2*x,x+3]);
  },
  nach(L){
    let x;
    if(L===1) x=rnd(1,19);
    else if(L===2) x=Math.random()<.3?rnd(1,9)*10+9:rnd(10,98);
    else if(L===3) x=Math.random()<.3?rnd(1,9)*100+99:rnd(90,998);
    else x=Math.random()<.3?rnd(1,9)*1000+999:rnd(900,9998);
    return numQ('nach',['nachfolger'],`Der ${m('Nachfolger')} von ${f(x)}`,x+1,`${f(x)} → ${f(x+1)}`,[x,x-1],[1,2,10]);
  },
  vor(L){
    let x;
    if(L===1) x=rnd(2,20);
    else if(L===2) x=Math.random()<.3?rnd(2,10)*10:rnd(11,100);
    else if(L===3) x=Math.random()<.3?rnd(1,9)*100:rnd(100,999);
    else x=Math.random()<.3?rnd(2,9)*1000:rnd(1000,9999);
    return numQ('vor',['vorgaenger'],`Der ${m('Vorgänger')} von ${f(x)}`,x-1,`${f(x-1)} → ${f(x)}`,[x,x+1],[1,2,10]);
  },
  missSum(L){
    const [a,b]=sumNums(L), c=a+b;
    const first=Math.random()<.5;
    const html=`Welcher ${m('Summand')} fehlt?<br>${first?`${f(a)} + ? = ${f(c)}`:`? + ${f(b)} = ${f(c)}`}`;
    const ans=first?b:a;
    return numQ('missSum',['summand'],html,ans,`${f(a)} + ${f(b)} = ${f(c)}`,[c,a+b-ans+1]);
  },
  missFac(L){
    const [a,b]=facNums(L), c=a*b;
    return numQ('missFac',['faktor'],`Welcher ${m('Faktor')} fehlt?<br>${f(a)} × ? = ${f(c)}`,b,`${f(a)} × ${f(b)} = ${f(c)}`,[b+1,b-1,a],[1,2]);
  },
  gerade(L){
    const [lo,hi]=[null,[1,20],[10,99],[100,999],[1000,9999]][L];
    const wantEven=Math.random()<.6;
    const nums=new Set();
    while(nums.size<3){ nums.add(rnd(lo,hi)); }
    let arr=[...nums];
    const want=wantEven?0:1;
    // genau eine passende, zwei nicht passende Zahlen erzwingen
    const fit=x=>x%2===want;
    let good=arr.filter(fit), bad=arr.filter(x=>!fit(x));
    const fix=(x,w)=>{ let y=x; while(y%2!==w||nums.has(y)&&y!==x){ y=rnd(lo,hi);} return y; };
    while(good.length!==1||bad.length!==2){
      arr=[];
      const used=new Set();
      const mk=w=>{ let y; do{ y=rnd(lo,hi);}while(y%2!==w||used.has(y)); used.add(y); return y; };
      arr=[mk(want),mk(1-want),mk(1-want)];
      good=arr.filter(fit); bad=arr.filter(x=>!fit(x));
    }
    const ans=good[0];
    const t=wantEven?'gerade':'ungerade';
    return finish('gerade',[t],`Welche Zahl ist ${m(t)}?`,ans,`${f(ans)} ist ${t} (Endziffer ${ans%10}).`,bad);
  },
  zwischen(L){
    let lo=[0,rnd(1,17),rnd(10,97),rnd(100,997),rnd(1000,9997)][L];
    if(L>=2&&Math.random()<.35) lo=rnd(1,9)*[0,0,10,100,1000][L]-1;
    const hi=lo+2, ans=lo+1;
    const d=shuffle([lo-1,lo,hi,hi+1].filter(x=>x>=0)).slice(0,2);
    return finish('zwischen',['zwischen'],`Welche Zahl liegt ${m('zwischen')} ${f(lo)} und ${f(hi)}?`,ans,`${f(lo)} < ${f(ans)} < ${f(hi)}`,d);
  },
  vielfaches(L){
    const k=L===2?pick([2,3,4,5,10]):L===3?rnd(2,9):rnd(6,12);
    const mult=rnd(2,L===4?12:10), c=k*mult;
    const cand=shuffle([c-1,c+1,c-2,c+2,c+k-1,c-k+1].filter(x=>x>0&&x%k!==0));
    const d=[...new Set(cand)].slice(0,2);
    return finish('vielfaches',['vielfaches'],`Welche Zahl ist ein ${m('Vielfaches')} von ${k}?`,c,`${f(c)} = ${k} × ${mult}`,d);
  },
  quersumme(L){
    const n=L===3?rnd(10,999):rnd(100,9999);
    const dg=String(n).split('').map(Number), q=dg.reduce((s,x)=>s+x,0);
    return numQ('quersumme',['quersumme'],`Die ${m('Quersumme')} von ${f(n)}`,q,`${dg.join(' + ')} = ${q}`,[],[1,2,3]);
  }
};
const BASE_KINDS=['sum','diff','prod','quot','dbl','half','triple','nach','vor','missSum','missFac','gerade','zwischen','vielfaches'];
const KINDS={
  1:['sum','diff','prod','dbl','half','nach','vor','missSum','gerade','zwischen'],
  2:BASE_KINDS,
  3:[...BASE_KINDS,'quersumme'],
  4:[...BASE_KINDS,'quersumme']
};
function genQuestion(L,history,lastKind){
  let q=null;
  for(let i=0;i<30;i++){
    const kinds=KINDS[L].filter(k=>k!==lastKind);
    q=G[pick(kinds)](L);
    if(!history.has(q.html)) break;
  }
  history.add(q.html);
  return q;
}
/*GEN-END*/

/* ===== Spiel-Konfiguration ===== */
const BASE={1:12,2:10,3:9,4:9};
const MOVES=[
  {id:'sprint',e:'🏃',n:'Sprint',h:'So schnell wie möglich laufen',b:0,on:true},
  {id:'frosch',e:'🐸',n:'Froschhüpfer',h:'Beidbeinig hüpfen',b:3,on:true},
  {id:'storch',e:'🦩',n:'Einbeinhüpfen',h:'Auf einem Bein, Wechsel erlaubt',b:3,on:true},
  {id:'seit',e:'↔️',n:'Seitgalopp',h:'Seitlich galoppieren',b:1,on:true},
  {id:'knie',e:'🦵',n:'Kniehebelauf',h:'Knie hoch!',b:1,on:true},
  {id:'schleich',e:'🥷',n:'Schleichen',h:'Auf Zehenspitzen, ganz leise',b:2,on:true},
  {id:'krebs',e:'🦀',n:'Krebsgang',h:'Im Rückenstütz, Bauch nach oben',b:5,on:true},
  {id:'baer',e:'🐻',n:'Bärengang',h:'Auf Händen und Füßen',b:4,on:true},
  {id:'rueck',e:'⏪',n:'Rückwärts',h:'Rückwärts laufen, Blick über die Schulter (nur bei viel Platz)',b:2,on:false}
];
/* Bewegungsarten, bei denen keine Partner-Runde (an der Hand) möglich ist */
const NO_PARTNER=new Set(['storch','krebs','baer','rueck']);
const CHAOS={
  blitz:{e:'⚡',n:'Blitzrunde',d:'Nur 60 % der Zeit!'},
  stille:{e:'🔇',n:'Stille Runde',d:'Kein Ticken – fühlt die Zeit selbst!'},
  partner:{e:'🤝',n:'Partner-Runde',d:'Zu zweit an der Hand: Einigt euch auf eine Ecke!'},
  doppelt:{e:'✨',n:'Doppelte Punkte',d:'Jeder Treffer zählt 2 Punkte.'}
};
const FITNESS=['Kniebeugen','Hampelmänner','Skipping auf der Stelle','Strecksprünge','Ausfallschritte (abwechselnd)','Anfersen auf der Stelle','Hocke-Streck-Sprünge'];
const TEAMCOL=['#f472b6','#34d399','#fb923c','#a78bfa'];
const LETTERS=[['A','▲'],['B','■'],['C','●']];
const LEVELS=[
  [1,'Stufe 1','Klasse 1–2 · Zahlen bis 20 · plus, minus, doppelt, halb'],
  [2,'Stufe 2','Klasse 2–3 · Zahlen bis 100 · kleines Einmaleins, teilen'],
  [3,'Stufe 3','Klasse 3–4 · volles Einmaleins · Zahlen bis ca. 300'],
  [4,'Stufe 4','Klasse 5–6 · Zahlen bis 1000 · mal 10/100, Quersumme']
];

const S={level:2,rounds:12,base:10,ramp:true,chaos:true,sound:true,bonus:true,reps:5,teams:0,
  moves:Object.fromEntries(MOVES.map(x=>[x.id,x.on])),
  phase:'setup',round:0,history:new Set(),seen:[],scores:[],cur:null,move:null,chaosId:null,lastKind:null,lastMove:null,fit:'',time:10};

try{
  const saved=JSON.parse(localStorage.getItem('zahlenalarm')||'null');
  if(saved){ for(const k of ['level','rounds','base','ramp','chaos','sound','bonus','reps','teams']) if(saved[k]!==undefined) S[k]=saved[k];
    if(saved.moves) Object.assign(S.moves,saved.moves); }
}catch(e){}
function save(){ try{ const {level,rounds,base,ramp,chaos,sound,bonus,reps,teams,moves}=S; localStorage.setItem('zahlenalarm',JSON.stringify({level,rounds,base,ramp,chaos,sound,bonus,reps,teams,moves})); }catch(e){} }

/* ===== Ton ===== */
let AC=null;
function ac(){
  if(!S.sound) return null;
  try{ if(!AC) AC=new (window.AudioContext||window.webkitAudioContext)(); if(AC.state==='suspended') AC.resume(); }catch(e){ AC=null; }
  return AC;
}
function beep(freq,dur,vol=.15,type='sine',when=0){
  const c=ac(); if(!c) return;
  try{
    const t=c.currentTime+when, o=c.createOscillator(), g=c.createGain();
    o.type=type; o.frequency.value=freq;
    g.gain.setValueAtTime(vol,t); g.gain.exponentialRampToValueAtTime(.0001,t+dur);
    o.connect(g); g.connect(c.destination); o.start(t); o.stop(t+dur+.02);
  }catch(e){}
}
const sfx={
  go(){ beep(523,.12,.18,'triangle'); beep(784,.2,.18,'triangle',.13); },
  tick(){ beep(620,.05,.09,'triangle'); },
  urgent(){ beep(1150,.12,.2,'square'); },
  stop(){ beep(196,.7,.28,'sawtooth'); },
  win(){ [523,659,784,1047].forEach((x,i)=>beep(x,.18,.16,'triangle',i*.1)); }
};

/* ===== Hilfsfunktionen ===== */
const $=s=>document.querySelector(s);
const app=$('#app');
function roundTime(){
  let b=S.base;
  if(S.ramp) b=Math.max(4,b-Math.floor((S.round-1)/3));
  if(S.chaosId==='blitz') b=Math.max(3,Math.round(b*.6));
  return b+S.move.b;
}
const hudLeft=()=>`<span class="pill">Runde ${S.round} / ${S.rounds}</span>`+
  `<span class="pill move">${S.move.e} ${S.move.n}</span>`+
  (S.chaosId?`<span class="pill chaos">${CHAOS[S.chaosId].e} ${CHAOS[S.chaosId].n}</span>`:'');
const cardsHtml=(q,cls=[])=>`<div class="cards">${q.options.map((o,i)=>
  `<div class="card ${LETTERS[i][0]} ${cls[i]||''}"><div class="tag">${LETTERS[i][1]} ${LETTERS[i][0]}</div><div class="val">${o}</div></div>`).join('')}</div>`;
function teamsHtml(withButtons){
  if(!S.teams) return '';
  return `<div class="teams" aria-label="Punktestand">`+S.scores.map((s,i)=>
    `<div class="team" style="--tc:${TEAMCOL[i]}"><span>Team ${i+1}</span>`+
    (withButtons?`<button data-act="score" data-i="${i}" data-d="-1" aria-label="Team ${i+1} minus 1">−</button>`:'')+
    `<strong id="sc${i}">${s}</strong>`+
    (withButtons?`<button data-act="score" data-i="${i}" data-d="1" aria-label="Team ${i+1} plus ${S.chaosId==='doppelt'?2:1}">+${S.chaosId==='doppelt'?2:1}</button>`:'')+
    `</div>`).join('')+`</div>`;
}

/* ===== Ansichten ===== */
const VIEWS={
  setup(){
    return `<section class="setup">
<h1>ZAHLEN-<span>ALARM</span></h1>
<p class="lead">Kopfrechnen unter Zeitdruck – kombiniert mit Laufen, Hüpfen und Schleichen. Die Klasse steht in der Mitte, die Antworten hängen in den Ecken A, B und C.</p>
<div class="cols">
<fieldset><legend>Stufe</legend>
${LEVELS.map(([v,n,d])=>`<label class="opt"><input type="radio" name="level" value="${v}" ${S.level===v?'checked':''}><span><b>${n}</b><small>${d}</small></span></label>`).join('')}
</fieldset>
<fieldset><legend>Spielablauf</legend>
<div class="row"><label for="rounds">Runden:</label>
<select id="rounds" name="rounds">${[8,12,16,20].map(n=>`<option ${S.rounds===n?'selected':''}>${n}</option>`).join('')}</select></div>
<div class="row"><label for="base">Zeit pro Aufgabe (ohne Laufbonus):</label><output id="baseOut">${S.base} s</output></div>
<input type="range" id="base" min="4" max="30" step="1" value="${S.base}">
<label class="opt"><input type="checkbox" name="ramp" ${S.ramp?'checked':''}><span><b>Stress-Rampe</b><small>Alle 3 Runden 1 Sekunde weniger Zeit (mindestens 4 s)</small></span></label>
<label class="opt"><input type="checkbox" name="chaos" ${S.chaos?'checked':''}><span><b>Chaos-Karten</b><small>Zufällig: Blitzrunde, Stille Runde, Partner-Runde, doppelte Punkte</small></span></label>
<label class="opt"><input type="checkbox" name="sound" ${S.sound?'checked':''}><span><b>Ton / Ticken</b><small>Countdown-Ticken, in den letzten 3 Sekunden schneller</small></span></label>
</fieldset>
<fieldset><legend>Teams &amp; Fitness-Bonus</legend>
<div class="row"><label for="teams">Punkte zählen für:</label>
<select id="teams" name="teams">${[[0,'Keine Teams'],[2,'2 Teams'],[3,'3 Teams'],[4,'4 Teams']].map(([v,t])=>`<option value="${v}" ${S.teams===v?'selected':''}>${t}</option>`).join('')}</select></div>
<label class="opt"><input type="checkbox" name="bonus" ${S.bonus?'checked':''}><span><b>Fitness-Bonus</b><small>Wer danebenlag, macht eine kurze Übung und zählt laut mit</small></span></label>
<div class="row"><label for="reps">Wiederholungen:</label>
<select id="reps" name="reps">${[3,5,8,10].map(n=>`<option ${S.reps===n?'selected':''}>${n}</option>`).join('')}</select></div>
</fieldset>
<fieldset><legend>Bewegungsarten (Weg zur Ecke)</legend>
${MOVES.map(x=>`<label class="opt"><input type="checkbox" name="move" value="${x.id}" ${S.moves[x.id]?'checked':''}><span><b>${x.e} ${x.n}</b><small>${x.h} · +${x.b} s</small></span></label>`).join('')}
</fieldset>
</div>
<div class="actions">
<button class="btn primary" data-act="start" data-primary>▶ Spiel starten</button>
<button class="btn" data-act="signs">🔺 Ecken-Schilder</button>
<button class="btn" data-act="glossary">📖 Wortschatz</button>
</div>
<details><summary>Hallenaufbau &amp; Ablauf (60 Sekunden lesen)</summary>
<ol>
<li>Mitte der Halle markieren (Reifen oder Matte). Dort steht die Klasse zu Beginn jeder Runde.</li>
<li>Drei Ecken mit den Schildern A ▲, B ■ und C ● kennzeichnen (Schilder-Ansicht). Abstand zur Mitte etwa 8–12 m, hinter den Ecken mindestens 2 m Auslauf zur Wand.</li>
<li>Bildschirm oder Beamer so stellen, dass man ihn aus der Mitte lesen kann.</li>
<li>Pro Runde: Bewegungsart ansagen, „Aufgabe zeigen“ drücken, die Kinder rechnen und bewegen sich in die gewählte Ecke. Bei Ablauf der Zeit gilt „Stopp – einfrieren!“.</li>
<li>Dann „Lösung zeigen“: Kinder in der richtigen Ecke jubeln und rufen das Wort der Runde. Kinder in anderen Ecken machen den Fitness-Bonus.</li>
<li>Zurück zur Mitte gehen (Gehen, nicht rennen), dann „Nächste Runde“.</li>
</ol>
<p class="muted">Tipp: Wenn die Kinder die Ecken kaum erreichen, die Zeit erhöhen oder langsame Bewegungsarten abwählen. Das Spiel passt automatisch pro Bewegungsart Sekunden dazu.</p>
</details>
</section>`;
  },

  prep(){
    const c=S.chaosId?CHAOS[S.chaosId]:null;
    return `<section class="screen center">
<div class="hud">${hudLeft()}<span class="spacer"></span><span class="pill">⏱ ${roundTime()} s</span></div>
<div class="big">${S.move.e}<br>${S.move.n}</div>
<p class="box">${S.move.h}<br>Alle in die Mitte! Gleich erscheint die Aufgabe. Rechnen, dann in die Ecke <b>A ▲</b>, <b>B ■</b> oder <b>C ●</b>.</p>
${c?`<div class="box chaosbox"><b>${c.e} ${c.n}:</b> ${c.d}</div>`:''}
${teamsHtml(false)}
<div class="actions" style="justify-content:center"><button class="btn primary" data-act="show" data-primary>▶ Aufgabe zeigen</button></div>
</section>`;
  },

  play(){
    return `<section class="screen" id="play">
<div class="hud">${hudLeft()}<span class="spacer"></span><span class="tnum" id="tnum" role="timer" aria-label="Restzeit"></span></div>
<div class="track" aria-hidden="true"><div class="fill" id="tbar"></div></div>
<div class="question">${S.cur.html}</div>
${cardsHtml(S.cur)}
<div class="hud"><button class="btn small" data-act="pause" id="btnPause">⏸ Pause</button><button class="btn small" data-act="stopnow">⏹ Jetzt stoppen</button></div>
</section>`;
  },

  stop(){
    return `<section class="screen center stopscreen">
<div class="hud">${hudLeft()}</div>
<div class="big">✋ STOPP!</div>
<p class="box">Einfrieren! Alle bleiben genau dort stehen, wo sie sind. Wer ist in welcher Ecke?</p>
<div class="question small">${S.cur.html}</div>
<div class="actions" style="justify-content:center"><button class="btn primary" data-act="reveal" data-primary>✔ Lösung zeigen</button></div>
</section>`;
  },

  reveal(){
    const q=S.cur, cls=q.options.map((_,i)=>i===q.correct?'right':'dim');
    const words=q.terms.map(t=>GLOSSARY[t]);
    const first=words[0].w.replace(/^(das|die|der) /,'').toUpperCase();
    return `<section class="screen">
<div class="hud">${hudLeft()}</div>
<div class="question small">${q.html}</div>
${cardsHtml(q,cls)}
<div class="center big" style="font-size:clamp(1.8rem,5vw,3.6rem)">✅ ${LETTERS[q.correct][1]} ${LETTERS[q.correct][0]}: ${q.explain}</div>
<div class="two">
<div class="box word">📖 <b>Wort der Runde</b><br>${words.map(w=>`<b>${w.w}</b>: ${w.d}<br><span class="muted">${w.ex}</span>`).join('<br><br>')}
<div class="shout">📣 Ruft gemeinsam: ${first}!</div></div>
${S.bonus?`<div class="box fit">💪 <b>Fitness-Bonus</b> für alle, die in einer anderen Ecke standen:<br><b>${S.reps} × ${S.fit}</b> – laut mitzählen!</div>`:
 `<div class="box fit">🎉 Richtige Ecke? Jubel-Pose und High-Five mit dem Nachbarn!</div>`}
</div>
${teamsHtml(true)}
<div class="actions" style="justify-content:center"><button class="btn primary" data-act="next" data-primary>${S.round>=S.rounds?'🏁 Zur Auswertung':'➡ Nächste Runde (zurück zur Mitte gehen)'}</button></div>
</section>`;
  },

  end(){
    const words=S.seen.map(k=>GLOSSARY[k]);
    let win='';
    if(S.teams){
      const mx=Math.max(...S.scores), w=S.scores.map((s,i)=>s===mx?`Team ${i+1}`:null).filter(Boolean);
      win=`<div class="box word center"><b>🏆 ${w.length>1?'Gleichstand':'Sieger'}: ${w.join(' &amp; ')}</b> mit ${mx} Punkten</div>`;
    }
    return `<section class="screen">
<div class="big" style="font-size:clamp(2.4rem,7vw,5rem)">🏁 Geschafft!</div>
${win}${teamsHtml(false)}
<div class="box"><b>Diese Wörter habt ihr heute gelernt:</b>
<div class="gl" style="margin-top:.6rem">${words.map(w=>`<div><b>${w.w}</b><i>${w.d}</i></div>`).join('')}</div></div>
<p class="box">🧘 Cool-down: Eine Runde locker gehen, tief durchatmen, Arme ausschütteln.</p>
<div class="actions" style="justify-content:center">
<button class="btn primary" data-act="again" data-primary>🔁 Nochmal spielen</button>
<button class="btn" data-act="toSetup">⚙ Einstellungen</button></div>
</section>`;
  }
};

function render(){
  document.body.dataset.phase=S.phase;
  app.innerHTML=VIEWS[S.phase]();
  $('#btnHome').hidden=S.phase==='setup';
  const p=app.querySelector('[data-primary]');
  if(p&&S.phase!=='setup') p.focus({preventScroll:true});
  window.scrollTo(0,0);
}

/* ===== Timer ===== */
const T={left:0,total:0,last:0,paused:false,raf:0,lastSec:null};
function startTimer(sec){
  T.total=sec*1000; T.left=T.total; T.paused=false; T.lastSec=null; T.last=performance.now();
  cancelAnimationFrame(T.raf);
  const step=now=>{
    if(S.phase!=='play') return;
    const dt=Math.min(now-T.last,250); T.last=now;
    if(!T.paused) T.left-=dt;
    const sec=Math.max(0,Math.ceil(T.left/1000));
    const fr=Math.max(0,T.left/T.total);
    const num=$('#tnum'), bar=$('#tbar'), box=$('#play');
    if(num) num.textContent=sec;
    if(bar){ bar.style.width=(fr*100)+'%'; if(sec>3) bar.style.background=`hsl(${Math.round(120*fr)},75%,52%)`; else bar.style.background=''; }
    if(box) box.classList.toggle('urgent',sec<=3&&sec>0&&!T.paused);
    if(sec!==T.lastSec){
      T.lastSec=sec;
      if(sec>0&&!T.paused&&S.chaosId!=='stille') (sec<=3?sfx.urgent:sfx.tick)();
    }
    if(T.left<=0){ toStop(); return; }
    T.raf=requestAnimationFrame(step);
  };
  T.raf=requestAnimationFrame(step);
}
function toStop(){ cancelAnimationFrame(T.raf); S.phase='stop'; sfx.stop(); render(); }

/* ===== Spielfluss ===== */
function startGame(){
  if(!MOVES.some(x=>S.moves[x.id])) S.moves.sprint=true;
  save();
  S.round=0; S.history=new Set(); S.seen=[]; S.lastKind=null; S.lastMove=null;
  S.scores=Array(S.teams).fill(0);
  ac(); sfx.go();
  nextRound();
}
function nextRound(){
  S.round++;
  if(S.round>S.rounds){ S.phase='end'; sfx.win(); render(); return; }
  const avail=MOVES.filter(x=>S.moves[x.id]&&x.id!==S.lastMove);
  S.move=pick(avail.length?avail:MOVES.filter(x=>S.moves[x.id]));
  S.lastMove=S.move.id;
  S.chaosId=null;
  if(S.chaos&&S.round>1&&Math.random()<.25){
    S.chaosId=pick(Object.keys(CHAOS).filter(k=>(k!=='doppelt'||S.teams>0)&&(k!=='partner'||!NO_PARTNER.has(S.move.id))));
  }
  S.cur=genQuestion(S.level,S.history,S.lastKind);
  S.lastKind=S.cur.kind;
  S.fit=pick(FITNESS);
  S.phase='prep'; render();
}
const ACTS={
  start:startGame,
  show(){ S.phase='play'; sfx.go(); render(); startTimer(roundTime()); },
  pause(b){ T.paused=!T.paused; b.textContent=T.paused?'▶ Weiter':'⏸ Pause'; if(!T.paused) T.last=performance.now(); },
  stopnow(){ toStop(); },
  reveal(){
    S.cur.terms.forEach(t=>{ if(!S.seen.includes(t)) S.seen.push(t); });
    S.phase='reveal'; sfx.win(); render();
  },
  next:nextRound,
  again:startGame,
  toSetup(){ S.phase='setup'; render(); },
  score(b){
    const i=+b.dataset.i, d=+b.dataset.d, pts=d>0&&S.chaosId==='doppelt'?2:1;
    S.scores[i]=Math.max(0,S.scores[i]+d*pts);
    const out=$('#sc'+i); if(out) out.textContent=S.scores[i];
  },
  sound(b){ S.sound=!S.sound; b.setAttribute('aria-pressed',S.sound); b.textContent=S.sound?'🔊 Ton':'🔇 Ton'; save(); },
  full(){ try{ if(document.fullscreenElement) document.exitFullscreen(); else document.documentElement.requestFullscreen().catch(()=>{}); }catch(e){} },
  glossary(){ openOverlay(`<h2>📖 Mathe-Wortschatz</h2><div class="gl">${Object.values(GLOSSARY).map(w=>`<div><b>${w.w}</b><i>${w.d}</i><i>${w.ex}</i></div>`).join('')}</div>`); },
  signs(){ openOverlay(`<h2>Ecken-Schilder</h2><p class="muted">Zum Aufhängen: Bildschirmfoto machen und ausdrucken, oder Buchstabe, Form und Farbe groß auf Papier malen.</p><div class="signs">${LETTERS.map(([l,s])=>`<div class="sign ${l}"><div class="s">${s}</div><div class="l">${l}</div></div>`).join('')}</div>`); },
  home(){ openOverlay(`<h2>Spiel beenden?</h2><p>Der aktuelle Stand geht verloren.</p><div class="actions"><button class="btn danger" data-act="confirmHome">Ja, beenden</button><button class="btn" data-act="close">Weiterspielen</button></div>`); },
  confirmHome(){ cancelAnimationFrame(T.raf); closeOverlay(); S.phase='setup'; render(); },
  close:closeOverlay
};

/* ===== Overlay ===== */
let lastFocus=null;
function openOverlay(html){
  const o=$('#overlay'); lastFocus=document.activeElement;
  if(S.phase==='play'&&!T.paused){ T.paused=true; const b=$('#btnPause'); if(b) b.textContent='▶ Weiter'; }
  o.innerHTML=`<div class="sheet">${html}<div class="actions"><button class="btn" data-act="close" id="ovClose">✖ Schließen</button></div></div>`;
  o.hidden=false; $('#ovClose').focus();
}
function closeOverlay(){
  const o=$('#overlay'); o.hidden=true; o.innerHTML='';
  if(lastFocus&&lastFocus.focus) try{ lastFocus.focus(); }catch(e){}
}

/* ===== Events ===== */
document.addEventListener('click',e=>{
  const b=e.target.closest('[data-act]'); if(!b) return;
  const fn=ACTS[b.dataset.act]; if(fn) fn(b);
});
app.addEventListener('change',e=>{
  const t=e.target; if(S.phase!=='setup') return;
  if(t.name==='level'){ S.level=+t.value; S.base=BASE[S.level]; $('#base').value=S.base; $('#baseOut').textContent=S.base+' s'; }
  else if(t.name==='rounds') S.rounds=+t.value;
  else if(t.name==='ramp') S.ramp=t.checked;
  else if(t.name==='chaos') S.chaos=t.checked;
  else if(t.name==='sound'){ S.sound=t.checked; const b=$('#btnSound'); b.setAttribute('aria-pressed',S.sound); b.textContent=S.sound?'🔊 Ton':'🔇 Ton'; }
  else if(t.name==='bonus') S.bonus=t.checked;
  else if(t.name==='reps') S.reps=+t.value;
  else if(t.name==='teams') S.teams=+t.value;
  else if(t.name==='move') S.moves[t.value]=t.checked;
  save();
});
app.addEventListener('input',e=>{
  if(e.target.id==='base'){ S.base=+e.target.value; $('#baseOut').textContent=S.base+' s'; }
});
document.addEventListener('keydown',e=>{
  if(e.key==='Escape'&&!$('#overlay').hidden){ closeOverlay(); return; }
  if((e.key===' '||e.key==='Enter')&&e.target===document.body&&$('#overlay').hidden){
    const p=app.querySelector('[data-primary]'); if(p){ e.preventDefault(); p.click(); }
  }
});

$('#btnSound').setAttribute('aria-pressed',S.sound);
$('#btnSound').textContent=S.sound?'🔊 Ton':'🔇 Ton';
render();
</script>
</body>
</html>
