---
name: rest-set-drills
description: Build phone-friendly micro-learning drill decks to do between weight-lifting sets, with mixed tap-only question formats, published as an artifact. Use when the user asks for a micro learning exercise, quiz or drills between sets or rest periods.
---

# Rest-set drills

Builds a one-page phone quiz the user works through during rest periods at the gym: one drill per rest, laid out as exercises × sets. Every format is answerable by tapping, with no typing, and works while breathing hard.

## 1. Settle the parameters

Take these from the request. Don't ask about anything that has a default.

- **Topic**, and any **source** (a book, an author, a framework). If a source is named, it's the primary authority. Stay faithful to its real concepts and terms, and don't invent quotes.
- **Exercises × sets.** Default: 6 exercises × 3 sets = 18 drills. If the user gives a number of rest periods, use that as the total.
- **Focus.** Concepts and behaviors (default), vocabulary, a step-by-step method, or prep for a real situation (an interview or a meeting). Situational decks get a short case banner and drills set in that scenario.
- **Timer.** Off. The user handles rest timing.

If the topic involves current facts (markets, companies, prices, recent data), research them first with web search. Use only figures you've verified, and name the source in the drill's explanation. List sources in the reply.

## 2. Write the drills

Each drill teaches one idea. Order the deck so it builds: basics first, applications later. For a method, follow the method's real order and name each step in the `tag`. End with a synthesis drill, usually a "say it out loud" one-breath summary.

**Format mix.** Use all seven types. Never use the same type twice in a row. For 18 drills, aim roughly for: mc 5–6, multi 2–3, tf 2, order 2, match 1–2, sort 2, recall 2–3. Choose the format the content fits:
- `order` for real sequences: steps, loops, rankings. Never invent an order.
- `match` for terms ↔ definitions, or concepts ↔ examples. 4–5 pairs, each side under about 10 words.
- `sort` for two-way classifications. 5–6 items.
- `multi` for "which of these…" questions with 2–4 correct answers out of 6.
- `recall` for things the user should be able to say unprompted: frameworks, scripts, a summary.
- `tf` for common misconceptions. Make roughly half of them false.

**Writing rules** (the user reads these mid-workout):
- Prompt: one or two short sentences.
- Options: under about 15 words each, similar in length, and every wrong option plausible. Don't let the correct answer be the longest one every time.
- Ordering: write options, sort items and match pairs in any order. The page shuffles the display order of `mc`, `multi`, `sort`, `match` and `order` per drill and per session, so the answer position never follows a pattern. Indexes in `a` and sort bucket numbers always refer to the order as written. True/false buttons stay fixed.
- `why`: 1–3 plain sentences explaining the reason. Where it helps, add a "Use it:" example sentence or a source detail.
- `lock`: a takeaway of 10 words or fewer to say out loud. Each lock is distinct, since the end screen lists them all.
- Plain language. No em-dash asides, no "not X but Y" constructions.

**Data schema** (one object per drill, in the `DRILLS` array):
```js
{type:"mc",    tag, prompt, opts:[...3], a:index, why, lock}
{type:"tf",    tag, prompt /* a statement */, a:true|false, why, lock}
{type:"multi", tag, prompt, opts:[...6], a:[indices], why, lock}
{type:"order", tag, prompt, items:[ /* in the correct order; the page shuffles them */ ], why, lock}
{type:"match", tag, prompt, pairs:[[left,right],...], why, lock}
{type:"sort",  tag, prompt, buckets:[A,B], items:[[text, 0|1],...], why, lock}
{type:"recall",tag, prompt, answer:"text" | ["point",...], why, lock}
```
`tag` is a short topic label, for example "Step 3 · Draw the boundary".

## 3. Build the page

1. Save the drills as `drills.js` in the scratchpad, starting with `const DRILLS=[`.
2. Copy the template below to a new `.html` file in the working directory and replace every placeholder:
   - `__TITLE__`: a 2–4 word name. `__H1__`: a short display name.
   - `__SRC__`: for example `Source: <i>Book</i>, Author`.
   - `__CASE__`: one sentence such as `<b>Case:</b> …` or `<b>Prep for:</b> …`. For a pure concept deck, give one line on what the deck covers.
   - `__ACC__` / `__ACCD__`: an accent color for light and dark mode. Give each deck its own, so decks are easy to tell apart. Pairs that work: `#E2561B/#FF7A3D` orange, `#0E7C83/#3CC4C9` teal, `#A85F00/#F2A93B` amber, `#2456A6/#86AEF5` blue, `#7A3B8F/#CDA0DD` plum, `#B0305A/#F290B0` rose, `#3F6B2A/#9CCB7A` moss.
   - `__KEY__`: a unique storage key, for example `drills-topic-v1`.
   - `__EX__`, `__SETS__`: numbers.
   - `__ENDTITLE__`: the heading for the end-screen list, for example "Your 18 takeaways".
   - `__DRILLS__`: the contents of `drills.js`.
3. Validate before publishing. Check that no `__` placeholders remain. Load the DRILLS array in node and check: the count equals EX×SETS; every `mc` `a` is within the options; every `multi` index is valid; every `sort` item's bucket is 0 or 1. Then parse the page's `<script>` with `new Function`.
4. Publish with the Artifact tool (icon `timer`, plus a one-sentence description). To revise a deck, republish the same file path so the link stays the same. Bump `__KEY__` when the drills change shape, so stale progress isn't loaded.

## 4. Reply

In 3–6 sentences or a short list: what the deck covers, how the formats are spread, and anything it draws on (sources). Don't paste the URL; the artifact card shows it. Offer to tailor the deck if a detail would sharpen it, such as a company name.

## Template

```html
<title>__TITLE__</title>
<meta name="claude:viewport-fit-sides" content="cover">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Barlow+Condensed:wght@600;800&family=Atkinson+Hyperlegible:wght@400;700&display=swap">
<style>
/* Layout: one-thumb drill card — 6×3 plate grid for progress, one drill per rest, seven question formats */
:root{
  --bg:#EDF0F3; --surface:#FFFFFF; --ink:#16202B; --muted:#5A6774; --line:#D3DAE1;
  --accent:__ACC__; --good:#1F8A5B; --bad:#C23A2B; --accent-ink:#FFFFFF;
  --display:"Barlow Condensed","Arial Narrow",Arial,sans-serif;
  --body:"Atkinson Hyperlegible",system-ui,-apple-system,"Segoe UI",sans-serif;
}
@media (prefers-color-scheme: dark){:root:not([data-theme="light"]){
  --bg:#12171D; --surface:#1B232C; --ink:#E8EDF2; --muted:#9AA7B4; --line:#2C3743;
  --accent:__ACCD__; --good:#4CC28B; --bad:#FF6B5B; --accent-ink:#12171D; color-scheme:dark}}
:root[data-theme="dark"]{
  --bg:#12171D; --surface:#1B232C; --ink:#E8EDF2; --muted:#9AA7B4; --line:#2C3743;
  --accent:__ACCD__; --good:#4CC28B; --bad:#FF6B5B; --accent-ink:#12171D; color-scheme:dark}
*{box-sizing:border-box}
body{background:var(--bg);color:var(--ink);font-family:var(--body);font-size:17px;line-height:1.45;
  padding-left:max(16px,env(safe-area-inset-left,0px));padding-right:max(16px,env(safe-area-inset-right,0px));padding-block:16px 32px}
.wrap{max-width:560px;margin:0 auto;display:flex;flex-direction:column;gap:16px}
header{display:flex;justify-content:space-between;align-items:baseline;gap:12px;flex-wrap:wrap}
h1{font-family:var(--display);font-weight:800;font-size:28px;letter-spacing:.02em;text-transform:uppercase;margin:0}
.src{color:var(--muted);font-size:13px}
.case{margin:0;background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:12px 14px;font-size:16px}
.grid{display:grid;grid-template-columns:auto repeat(var(--sets),1fr);gap:6px;align-items:center}
.grid .lbl{font-family:var(--display);font-weight:600;font-size:13px;letter-spacing:.08em;text-transform:uppercase;color:var(--muted);padding-right:6px}
.plate{height:26px;border-radius:13px;border:2px solid var(--line);background:transparent;cursor:pointer;font:600 13px var(--display);color:var(--muted)}
.plate.ok{background:var(--ink);border-color:var(--ink);color:var(--bg)}
.plate.miss{background:transparent;border-color:var(--bad);color:var(--bad)}
.plate.cur{outline:3px solid var(--accent);outline-offset:1px}
button:focus-visible{outline:3px solid var(--accent);outline-offset:2px}
.score{font-family:var(--display);font-weight:600;font-size:15px;letter-spacing:.06em;text-transform:uppercase;color:var(--muted);font-variant-numeric:tabular-nums}
.card{display:flex;flex-direction:column;gap:14px}
.meta{display:flex;gap:8px;flex-wrap:wrap;align-items:center}
.tag{font-family:var(--display);font-weight:600;font-size:14px;letter-spacing:.1em;text-transform:uppercase;color:var(--accent)}
.kind{font:700 12px var(--display);letter-spacing:.12em;text-transform:uppercase;color:var(--accent-ink);background:var(--accent);border-radius:4px;padding:2px 7px}
.prompt{font-size:21px;font-weight:700;margin:0;text-wrap:balance}
.hint{color:var(--muted);font-size:15px;margin:-6px 0 0}
.opts{display:flex;flex-direction:column;gap:10px}
.opt{appearance:none;text-align:left;background:var(--surface);color:var(--ink);border:2px solid var(--line);border-radius:12px;padding:13px 14px;font:400 17px var(--body);cursor:pointer;min-height:52px;width:100%}
.opt[disabled]{cursor:default}
.opt.sel{border-color:var(--accent);box-shadow:inset 6px 0 0 var(--accent)}
.opt.right{border-color:var(--good);box-shadow:inset 6px 0 0 var(--good)}
.opt.wrong{border-color:var(--bad);opacity:.75}
.opt.missed{border-style:dashed;border-color:var(--good)}
.opt.used{opacity:.35}
.tf{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.tf .opt{text-align:center;font-weight:700;font-size:20px;min-height:64px}
.olist{display:flex;flex-direction:column;gap:8px;min-height:20px}
.olist .opt{display:flex;gap:10px;align-items:baseline}
.num{font:800 18px var(--display);color:var(--accent);min-width:1.2ch}
.match{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.match .col{display:flex;flex-direction:column;gap:8px;min-width:0}
.match .opt{font-size:15px;padding:10px;min-height:48px}
.opt.flash{border-color:var(--bad)}
.srow{background:var(--surface);border:2px solid var(--line);border-radius:12px;padding:10px 12px;display:flex;flex-direction:column;gap:8px}
.srow.right{border-color:var(--good)} .srow.wrong{border-color:var(--bad)}
.bk{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.bk button{appearance:none;border:2px solid var(--line);background:transparent;color:var(--ink);border-radius:8px;padding:8px;font:600 15px var(--body);cursor:pointer}
.bk button[aria-pressed="true"]{background:var(--ink);border-color:var(--ink);color:var(--bg)}
.bk button[disabled]{cursor:default}
.ansbox{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:14px}
.ansbox ul,.ansbox ol{margin:0;padding-left:20px;display:flex;flex-direction:column;gap:4px}
.btn{appearance:none;border:0;border-radius:10px;padding:14px 18px;font:700 17px var(--body);cursor:pointer;background:var(--accent);color:var(--accent-ink)}
.btn[disabled]{opacity:.4;cursor:default}
.btn.ghost{background:transparent;color:var(--ink);border:2px solid var(--line)}
.row{display:flex;gap:10px} .row .btn{flex:1}
.reveal{background:var(--surface);border:1px solid var(--line);border-radius:12px;padding:14px;display:flex;flex-direction:column;gap:10px}
.verdict{font-weight:700}
.verdict.r{color:var(--good)} .verdict.w{color:var(--bad)}
.lock{font-family:var(--display);font-weight:600;font-size:20px;letter-spacing:.02em;border-top:1px dashed var(--line);padding-top:10px}
.lock small{display:block;font:700 12px var(--display);letter-spacing:.12em;text-transform:uppercase;color:var(--muted)}
.finish ul{margin:0;padding-left:0;list-style:none;display:flex;flex-direction:column;gap:8px}
.finish li{display:flex;gap:8px;align-items:baseline}
.mk{font:700 11px var(--display);letter-spacing:.1em;text-transform:uppercase;border-radius:4px;padding:1px 6px;flex:none}
.mk.ok{background:var(--good);color:var(--bg)} .mk.rv{border:1px solid var(--bad);color:var(--bad)}
</style>

<div class="wrap">
  <header>
    <h1>__H1__</h1>
    <span class="src">__SRC__</span>
  </header>
  <p class="case">__CASE__</p>
  <div class="grid" id="grid" style="--sets:__SETS__" aria-label="Progress by exercise and set"></div>
  <div class="score" id="score"></div>
  <section class="card" id="card" aria-live="polite"></section>
</div>

<script>
__DRILLS__

const KEY="__KEY__",EX=__EX__,SETS=__SETS__;
const KIND={mc:"Pick one",tf:"True or false",multi:"Select all that apply",order:"Put in order",match:"Match the pairs",sort:"Sort it",recall:"Say it out loud"};
const newSeed=()=>Math.floor(Math.random()*2147483647);
let state={i:0,res:{},seed:newSeed()};
try{const s=JSON.parse(localStorage.getItem(KEY)||"null");if(s&&typeof s.i==="number"&&s.res)state=Object.assign({seed:newSeed()},s)}catch(e){}
const save=()=>{try{localStorage.setItem(KEY,JSON.stringify(state))}catch(e){}};
const grid=document.getElementById("grid"),card=document.getElementById("card"),scoreEl=document.getElementById("score");
const esc=s=>String(s).replace(/[&<>"]/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;"}[c]));
/* Display order is shuffled per drill and per session; answers are stored by original index. */
function rng(seed){let t=seed>>>0;return()=>{t=(t+0x6D2B79F5)>>>0;let r=Math.imul(t^(t>>>15),1|t);r^=r+Math.imul(r^(r>>>7),61|r);return((r^(r>>>14))>>>0)/4294967296}}
function shuffled(arr,salt){const a=arr.map((v,k)=>k),rand=rng(state.seed+state.i*7919+salt*104729);
  for(let k=a.length-1;k>0;k--){const j=Math.floor(rand()*(k+1));[a[k],a[j]]=[a[j],a[k]]}
  if(a.length>1&&a.every((v,k)=>v===k))a.push(a.shift());return a}
const el=(h)=>{const t=document.createElement("template");t.innerHTML=h.trim();return t.content.firstChild};

function renderGrid(){
  grid.innerHTML="";
  for(let ex=0;ex<EX;ex++){
    grid.appendChild(el(`<span class="lbl">Ex ${ex+1}</span>`));
    for(let r=0;r<SETS;r++){
      const idx=ex*SETS+r,res=state.res[idx];
      const b=el(`<button class="plate${res?(res.ok?" ok":" miss"):""}${idx===state.i?" cur":""}" aria-label="Exercise ${ex+1}, set ${r+1}">${r+1}</button>`);
      b.onclick=()=>{state.i=idx;save();render()};grid.appendChild(b);
    }
  }
  const done=Object.keys(state.res).length,ok=Object.values(state.res).filter(r=>r.ok).length;
  scoreEl.textContent=`${done} of ${DRILLS.length} done · ${ok} solid`;
}

function finish(ok,resp){state.res[state.i]={ok,resp};save();showReveal();renderGrid()}
function showReveal(){
  const d=DRILLS[state.i],r=state.res[state.i];if(!r)return;
  const box=card.querySelector("#rev");
  const good=d.type==="recall"?"Locked in.":"Correct.",bad=d.type==="recall"?"Marked for review.":"Not quite.";
  box.innerHTML=`<div class="reveal"><span class="verdict ${r.ok?"r":"w"}">${r.ok?good:bad}</span>
    <span>${esc(d.why)}</span><div class="lock"><small>Say it once out loud</small>${esc(d.lock)}</div></div>`;
}

const R={};
R.mc=(d,box,done)=>{
  const tf=d.type==="tf",opts=tf?["True","False"]:d.opts,a=tf?(d.a?0:1):d.a;
  const wrap=el(`<div class="${tf?"tf":"opts"}"></div>`);box.appendChild(wrap);
  const bs=opts.map((o,k)=>{const b=el(`<button class="opt" id="o${state.i}-${k}">${esc(o)}</button>`);
    b.onclick=()=>{paint(k);finish(k===a,k)};return b});
  (tf?opts.map((o,k)=>k):shuffled(opts,1)).forEach(k=>wrap.appendChild(bs[k]));
  function paint(p){bs.forEach((b,k)=>{b.disabled=true;if(k===a)b.classList.add("right");else if(k===p)b.classList.add("wrong")})}
  if(done)paint(done.resp);
};
R.tf=R.mc;
R.multi=(d,box,done)=>{
  const sel=new Set(),wrap=el(`<div class="opts"></div>`);box.appendChild(wrap);
  const bs=d.opts.map((o,k)=>{const b=el(`<button class="opt" aria-pressed="false" id="o${state.i}-${k}">${esc(o)}</button>`);
    b.onclick=()=>{sel.has(k)?sel.delete(k):sel.add(k);b.classList.toggle("sel");b.setAttribute("aria-pressed",sel.has(k))};return b});
  shuffled(d.opts,2).forEach(k=>wrap.appendChild(bs[k]));
  const chk=el(`<button class="btn">Check</button>`);box.appendChild(chk);
  function paint(p){const ps=new Set(p);chk.hidden=true;bs.forEach((b,k)=>{b.disabled=true;b.classList.remove("sel");
    const inA=d.a.includes(k);if(inA&&ps.has(k))b.classList.add("right");else if(!inA&&ps.has(k))b.classList.add("wrong");else if(inA)b.classList.add("missed")})}
  chk.onclick=()=>{const p=[...sel].sort();const ok=p.length===d.a.length&&d.a.every(k=>sel.has(k));paint(p);finish(ok,p)};
  if(done)paint(done.resp);
};
R.order=(d,box,done)=>{
  const order=shuffled(d.items,3),picked=[];
  box.appendChild(el(`<p class="hint">Tap the items in the right order.</p>`));
  const out=el(`<div class="olist"></div>`),pool=el(`<div class="opts"></div>`);box.appendChild(out);box.appendChild(pool);
  const clr=el(`<button class="btn ghost">Clear</button>`);box.appendChild(clr);
  const bs=order.map(k=>{const b=el(`<button class="opt" id="o${state.i}-${k}">${esc(d.items[k])}</button>`);
    b.onclick=()=>{if(picked.includes(k))return;picked.push(k);b.classList.add("used");b.disabled=true;draw();
      if(picked.length===d.items.length){const ok=picked.every((v,j)=>v===j);finish(ok,picked.slice());paint(picked)}};pool.appendChild(b);return b});
  function draw(){out.innerHTML=picked.map((k,j)=>`<div class="opt"><span class="num">${j+1}</span><span>${esc(d.items[k])}</span></div>`).join("")}
  clr.onclick=()=>{picked.length=0;bs.forEach(b=>{b.disabled=false;b.classList.remove("used")});draw()};
  function paint(p){pool.hidden=true;clr.hidden=true;
    out.innerHTML=p.map((k,j)=>`<div class="opt ${k===j?"right":"wrong"}"><span class="num">${j+1}</span><span>${esc(d.items[k])}</span></div>`).join("");
    if(!p.every((v,j)=>v===j))out.insertAdjacentHTML("afterend",`<div class="ansbox"><b>Correct order</b><ol>${d.items.map(t=>"<li>"+esc(t)+"</li>").join("")}</ol></div>`)}
  if(done)paint(done.resp);
};
R.match=(d,box,done)=>{
  box.appendChild(el(`<p class="hint">Tap one on the left, then its partner on the right.</p>`));
  const g=el(`<div class="match"><div class="col"></div><div class="col"></div></div>`);box.appendChild(g);
  const [L,Rc]=g.children,rord=shuffled(d.pairs,7);let cur=null,miss=0,matched=0;
  const lb=d.pairs.map((p,k)=>{const b=el(`<button class="opt" id="l${state.i}-${k}">${esc(p[0])}</button>`);
    b.onclick=()=>{lb.forEach(x=>x.classList.remove("sel"));cur=k;b.classList.add("sel")};return b});
  shuffled(d.pairs,4).forEach(k=>L.appendChild(lb[k]));
  const rb=[];rord.forEach(k=>{const b=el(`<button class="opt" id="r${state.i}-${k}">${esc(d.pairs[k][1])}</button>`);
    b.onclick=()=>{if(cur===null)return;
      if(cur===k){[lb[k],b].forEach(x=>{x.classList.remove("sel");x.classList.add("right");x.disabled=true});cur=null;matched++;
        if(matched===d.pairs.length)finish(miss===0,miss)}
      else{miss++;b.classList.add("flash");setTimeout(()=>b.classList.remove("flash"),500)}};
    rb[k]=b;Rc.appendChild(b)});
  if(done){g.remove();box.appendChild(el(`<div class="ansbox"><ul>${d.pairs.map(p=>`<li><b>${esc(p[0])}</b>: ${esc(p[1])}</li>`).join("")}</ul>${done.resp?`<p class="hint" style="margin:8px 0 0">${done.resp} wrong ${done.resp===1?"try":"tries"} on the first run.</p>`:""}</div>`))}
};
R.sort=(d,box,done)=>{
  const pick={},rows=[],wrap=el(`<div class="opts"></div>`);box.appendChild(wrap);
  d.items.forEach((it,k)=>{const row=el(`<div class="srow"><span>${esc(it[0])}</span><div class="bk">${d.buckets.map((bn,j)=>`<button aria-pressed="false" data-j="${j}" id="s${state.i}-${k}-${j}">${esc(bn)}</button>`).join("")}</div></div>`);
    row.querySelectorAll("button").forEach(b=>b.onclick=()=>{pick[k]=+b.dataset.j;row.querySelectorAll("button").forEach(x=>x.setAttribute("aria-pressed",x===b));chk.disabled=Object.keys(pick).length<d.items.length});
    rows.push(row)});
  shuffled(d.items,5).forEach(k=>wrap.appendChild(rows[k]));
  const chk=el(`<button class="btn" disabled>Check</button>`);box.appendChild(chk);
  function paint(p){chk.hidden=true;rows.forEach((row,k)=>{row.querySelectorAll("button").forEach(b=>{b.disabled=true;b.setAttribute("aria-pressed",+b.dataset.j===p[k])});
    row.classList.add(p[k]===d.items[k][1]?"right":"wrong");
    if(p[k]!==d.items[k][1])row.insertAdjacentHTML("beforeend",`<span class="hint" style="margin:0">Belongs in: ${esc(d.buckets[d.items[k][1]])}</span>`)})}
  chk.onclick=()=>{const p=d.items.map((_,k)=>pick[k]);paint(p);finish(p.every((v,k)=>v===d.items[k][1]),p)};
  if(done)paint(done.resp);
};
R.recall=(d,box,done)=>{
  box.appendChild(el(`<p class="hint">Answer out loud between reps, then check yourself.</p>`));
  const show=el(`<button class="btn">Show answer</button>`);box.appendChild(show);
  const ans=el(`<div class="ansbox" hidden>${Array.isArray(d.answer)?`<ul>${d.answer.map(t=>"<li>"+esc(t)+"</li>").join("")}</ul>`:esc(d.answer)}</div>`);box.appendChild(ans);
  const gr=el(`<div class="row" hidden><button class="btn">Got it</button><button class="btn ghost">Not yet</button></div>`);box.appendChild(gr);
  show.onclick=()=>{show.hidden=true;ans.hidden=false;gr.hidden=false};
  const [y,n]=gr.children;y.onclick=()=>{gr.hidden=true;finish(true,1)};n.onclick=()=>{gr.hidden=true;finish(false,0)};
  if(done){show.hidden=true;ans.hidden=false}
};

function render(){
  renderGrid();
  if(state.i>=DRILLS.length){
    const ok=Object.values(state.res).filter(r=>r.ok).length;
    card.innerHTML=`<div class="reveal finish"><div class="tag">Session complete · ${ok} of ${DRILLS.length} solid</div>
      <p class="prompt">__ENDTITLE__</p><ul>${DRILLS.map((d,k)=>{const r=state.res[k];return `<li><span class="mk ${r&&r.ok?"ok":"rv"}">${r&&r.ok?"Solid":"Review"}</span><span>${esc(d.lock)}</span></li>`}).join("")}</ul>
      <div class="row"><button class="btn ghost" id="again">Start over</button></div></div>`;
    card.querySelector("#again").onclick=()=>{state={i:0,res:{},seed:newSeed()};save();render();window.scrollTo(0,0)};
    return;
  }
  const d=DRILLS[state.i],ex=Math.floor(state.i/SETS)+1,r=state.i%SETS+1,done=state.res[state.i];
  card.innerHTML=`<div class="meta"><span class="kind">${KIND[d.type]}</span><span class="tag">Ex ${ex} · Set ${r} — ${esc(d.tag)}</span></div>
    <p class="prompt">${esc(d.prompt)}</p><div class="card" id="body"></div><div id="rev"></div>
    <div class="row"><button class="btn ghost" id="prev">Back</button><button class="btn" id="next">${state.i===DRILLS.length-1?"Finish":"Next"}</button></div>`;
  R[d.type](d,card.querySelector("#body"),done);
  if(done)showReveal();
  card.querySelector("#prev").onclick=()=>{if(state.i>0){state.i--;save();render();window.scrollTo(0,0)}};
  card.querySelector("#next").onclick=()=>{state.i++;save();render();window.scrollTo(0,0)};
}
render();
</script>

```
