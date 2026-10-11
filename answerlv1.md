# lv1 模範解答
Last login: Sun Oct 11 15:38:16 on ttys001
```sh
curl https://rits1019c1.github.io/crackme-web/lv1%                                                                      rituto@Ritsutos-MacBook-Air ~ % curl https://rits1019c1.github.io/crackme-web/lv1
<!doctype html>
<html lang="ja"><head><meta charset="utf-8"><title>crackme lv1</title>
<style>
body{margin:0;min-height:100vh;display:grid;place-items:center;background:#0b0f14;color:#9fe;font:16px/1.7 ui-monospace,Menlo,monospace}
main{max-width:520px;padding:24px}h1{font-size:20px;margin:0 0 8px}small{color:#678}
</style></head><body>
<main id="app"><h1>crackme lv1</h1>
<p>このページのどこかに <b>FLAG</b> がある。<br>DevToolsは開かせない。ページが白紙になる前に取り出せるか?</p>
<small>ルール: ページを壊さず(白紙にされず)FLAGを入手したら勝ち</small></main>
<script>
(()=>{
"use strict";
const _k=0x2a,_d="bGZrbVFGXBt1Tk9cXkVFRll1Q1l1REVedUt1XUtGRlc=";
const _flag=()=>atob(_d).split("").map(c=>String.fromCharCode(c.charCodeAt(0)^_k)).join("");
void _flag; // 使われないが残ってる = 取り出す対象

let tripped=false;
const trip=(why)=>{
  if(tripped)return;tripped=true;
  document.body.innerHTML='<p style="color:#f66;font:16px monospace;padding:24px">detected: '+why+'</p>';
  setTimeout(()=>location.replace("about:blank"),1500);
};

// 1) 入力妨害
addEventListener("contextmenu",e=>e.preventDefault());
addEventListener("keydown",e=>{
  const k=e.key.toLowerCase(),c=e.ctrlKey||e.metaKey;
  if(e.key==="F12"||(c&&e.shiftKey&&"ijc".includes(k))||(c&&e.altKey&&"ijuc".includes(k))||(c&&k==="u")){
    e.preventDefault();e.stopPropagation();
  }
},true);

// 2) ウィンドウサイズ差(ドッキング時のみ)
const bySize=()=>(outerWidth-innerWidth>160)||(outerHeight-innerHeight>160);

// 3) console.log の getter トラップ(開いてるとフォーマット時にidが読まれる)
let open3=false;
const el=new Image();
Object.defineProperty(el,"id",{get(){open3=true;return "x"}});

// 4) debugger の時間差(開いてると止まる)
const byDebugger=()=>{const t=performance.now();new Function("debugger")();return performance.now()-t>100};

setInterval(()=>{
  open3=false;console.log(el);console.clear();
  if(bySize())trip("window size");
  else if(open3)trip("console getter");
  else if(byDebugger())trip("debugger timing");
},500);
})();
</script></body></html>
rituto@Ritsutos-MacBook-Air ~ % node -e 'const d="bGZrbVFGXBt1Tk9cXkVFRll1Q1l1REVedUt1XUtGRlc="; console.log([...Buffer.from(d,"base64")].map(b=>String.fromCharCode(b^0x2a)).join(""))'
FLAG{lv1_devtools_is_not_a_wall}
rituto@Ritsutos-MacBook-Air ~ %
```
