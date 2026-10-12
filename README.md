<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Diário do Léo</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.min.js"></script>
<style>
:root{
  --bg:#050816;--card:rgba(10,18,40,.78);--ink:#E6F0FF;--mute:#7F93B5;--line:rgba(0,246,255,.16);--soft:rgba(0,246,255,.06);
  --acc:#00F6FF;--acc-soft:rgba(0,246,255,.10);--gain:#00FF88;--gain-soft:rgba(0,255,136,.09);--loss:#FF3864;--loss-soft:rgba(255,56,100,.10);color-scheme:dark;
  box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px);
}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
*{box-sizing:border-box}
body{margin:0;background:radial-gradient(circle at 8% 4%,rgba(0,246,255,.14),transparent 28%),radial-gradient(circle at 92% 8%,rgba(139,92,246,.14),transparent 28%),radial-gradient(circle at 50% 100%,rgba(22,131,255,.12),transparent 32%),var(--bg);background-attachment:fixed;color:var(--ink);font-family:'Inter',system-ui,sans-serif;font-size:14px;-webkit-font-smoothing:antialiased}
.hidden{display:none!important}
body:before{content:"";position:fixed;inset:0;pointer-events:none;z-index:-1;opacity:.5;background-image:linear-gradient(rgba(0,246,255,.06) 1px,transparent 1px),linear-gradient(90deg,rgba(0,246,255,.06) 1px,transparent 1px);background-size:44px 44px;-webkit-mask-image:linear-gradient(to bottom,#000,transparent 85%);mask-image:linear-gradient(to bottom,#000,transparent 85%)}
.card{backdrop-filter:blur(12px);box-shadow:0 0 28px rgba(0,246,255,.05),inset 0 0 22px rgba(0,246,255,.02)}
.gain{text-shadow:0 0 12px rgba(0,255,136,.35)}.loss{text-shadow:0 0 12px rgba(255,56,100,.35)}
.logo{color:#021018;box-shadow:0 0 18px rgba(0,246,255,.5)}
.btn:hover{border-color:var(--acc);box-shadow:0 0 14px rgba(0,246,255,.18)}
.btn.pri{background:linear-gradient(135deg,#00D9E6,#2563EB);border-color:transparent;color:#fff;box-shadow:0 0 18px rgba(0,246,255,.28)}
.tab.on{background:linear-gradient(135deg,#0891B2,#2563EB);color:#fff;box-shadow:0 0 16px rgba(0,246,255,.25)}
.kpi.main{background:linear-gradient(135deg,#0891B2,#2563EB);border-color:rgba(0,246,255,.5);box-shadow:0 0 26px rgba(0,246,255,.3)}
.kpi.main b{text-shadow:none}
.day:hover{box-shadow:0 0 18px rgba(0,246,255,.2)}
.day.g{border:1px solid rgba(0,255,136,.7);box-shadow:0 0 14px rgba(0,255,136,.16),inset 0 0 14px rgba(0,255,136,.04)}
.day.l{border:1px solid rgba(255,56,100,.7);box-shadow:0 0 14px rgba(255,56,100,.16),inset 0 0 14px rgba(255,56,100,.04)}
input:focus,select:focus,textarea:focus{box-shadow:0 0 0 3px rgba(0,246,255,.12),0 0 14px rgba(0,246,255,.2)}
select option{background:#0A1228;color:#E6F0FF}
#modal .m{background:#0A1228;border-color:rgba(0,246,255,.35);box-shadow:0 0 40px rgba(0,246,255,.15)}
#auth .box{border-color:rgba(0,246,255,.35);box-shadow:0 0 40px rgba(0,246,255,.12)}
.mini{border:1px solid var(--line)}
button,input,select,textarea{font-family:inherit;font-size:inherit;color:inherit}
.wrap{max-width:1180px;margin:0 auto;padding:20px 16px 48px}
.card{background:var(--card);border:1px solid var(--line);border-radius:16px}
.gain{color:var(--gain)}.loss{color:var(--loss)}.mute{color:var(--mute)}
.btn{border:1px solid var(--line);background:var(--card);padding:9px 14px;border-radius:10px;font-weight:600;cursor:pointer}
.btn:hover{background:var(--soft)}
.btn.pri{background:var(--acc);border-color:var(--acc);color:#fff}
.btn.pri:hover{filter:brightness(1.08)}
input,select,textarea{background:var(--soft);border:1px solid transparent;border-radius:10px;padding:10px 12px;width:100%}
input:focus,select:focus,textarea:focus{outline:2px solid var(--acc);outline-offset:0;background:var(--card)}
label.l{display:block;font-size:11px;font-weight:600;color:var(--mute);margin-bottom:6px;letter-spacing:.02em}

/* login */
#auth{min-height:100vh;display:flex;align-items:center;justify-content:center;padding:20px}
#auth .box{width:100%;max-width:380px;padding:32px;text-align:center}
.logo{width:44px;height:44px;border-radius:12px;background:var(--acc);color:#fff;display:inline-flex;align-items:center;justify-content:center;font-weight:800;font-size:18px}

/* topbar */
.top{display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;margin-bottom:20px}
.brand{display:flex;align-items:center;gap:12px}
.brand h1{font-size:20px;margin:0;font-weight:800;letter-spacing:-.02em}
.brand p{margin:2px 0 0;font-size:12px;color:var(--mute)}
.tabs{display:inline-flex;background:var(--soft);padding:4px;border-radius:12px;gap:2px;overflow-x:auto;max-width:100%}
.tab{border:0;background:transparent;padding:8px 14px;border-radius:9px;font-weight:600;color:var(--mute);cursor:pointer;white-space:nowrap}
.tab.on{background:var(--card);color:var(--ink);box-shadow:0 1px 2px rgba(15,23,42,.08)}

/* KPIs */
.kpis{display:grid;grid-template-columns:repeat(6,1fr);gap:12px;margin-bottom:20px}
.kpi{padding:16px 18px}
.kpi span{font-size:11.5px;color:var(--mute);font-weight:600;display:block}
.kpi b{font-size:20px;font-weight:800;letter-spacing:-.02em;display:block;margin-top:6px}
.kpi.main{background:var(--acc);border-color:var(--acc);color:#fff}
.kpi.main span{color:rgba(255,255,255,.75)}

/* calendar */
.sec{padding:20px}
.bar{display:flex;align-items:center;justify-content:space-between;gap:12px;flex-wrap:wrap;margin-bottom:16px}
.bar h2{margin:0;font-size:17px;font-weight:800;letter-spacing:-.01em}
.nav{display:flex;gap:8px;align-items:center}
.nav select{width:auto;font-weight:700}
.nav .btn{padding:8px 12px}
.split{display:grid;grid-template-columns:1fr 270px;gap:20px}
.dow,.cal{display:grid;grid-template-columns:repeat(7,1fr);gap:6px}
.dow div{font-size:11px;font-weight:700;color:var(--mute);text-align:center;padding-bottom:6px}
.blank{min-height:92px;border-radius:12px;background:var(--soft);opacity:.35}
.day{min-height:92px;border-radius:12px;border:1px solid var(--line);background:var(--card);padding:8px 9px;cursor:pointer;display:flex;flex-direction:column;justify-content:space-between;transition:.15s}
.day:hover{border-color:var(--acc);transform:translateY(-1px)}
.day.g{background:var(--gain-soft);border-color:transparent}
.day.l{background:var(--loss-soft);border-color:transparent}
.day .n{font-size:12px;font-weight:700;color:var(--mute)}
.day .v{font-size:13px;font-weight:800;letter-spacing:-.01em}
.day .m{font-size:10.5px;color:var(--mute)}
.gl{height:4px;border-radius:4px;overflow:hidden;display:flex;background:var(--line);margin:4px 0 2px}
.gl i{display:block;height:100%}
.side{display:flex;flex-direction:column;gap:10px}
.side h3{margin:0 0 4px;font-size:13px;font-weight:700}
.mini{padding:12px 14px;border-radius:12px;background:var(--soft)}
.mini span{font-size:11px;color:var(--mute);font-weight:600;display:block}
.mini b{font-size:16px;font-weight:800;display:block;margin-top:3px}
.mini small{font-size:10.5px;color:var(--mute)}

/* performance */
.chartbox{position:relative;height:300px}
.weeks{display:grid;grid-template-columns:repeat(auto-fill,minmax(250px,1fr));gap:14px;margin-top:20px}
.wk{padding:16px}
.wk header{display:flex;justify-content:space-between;align-items:baseline;margin-bottom:12px}
.wk header b{font-size:14px}.wk header small{color:var(--mute);font-size:11px}
.wk .two{display:grid;grid-template-columns:1fr 1fr;gap:8px;margin-bottom:12px}
.wrow{display:flex;justify-content:space-between;align-items:center;padding:8px 10px;background:var(--soft);border-radius:10px;margin-bottom:6px}
.wrow.off{opacity:.4}
.wrow small{color:var(--mute);font-size:10.5px;display:block}
.eq{display:grid;grid-template-columns:2fr 1fr;gap:16px}
.eq .card{padding:16px}

/* calc */
.calc{display:grid;grid-template-columns:repeat(auto-fill,minmax(230px,1fr));gap:14px}
.calc .card{padding:16px;display:flex;flex-direction:column;gap:12px}
.res{background:var(--soft);border-radius:12px;padding:12px;display:flex;flex-direction:column;gap:6px}
.res div{display:flex;justify-content:space-between;font-size:13px}

/* modal */
#modal{position:fixed;inset:0;background:rgba(10,14,22,.55);backdrop-filter:blur(4px);display:flex;align-items:center;justify-content:center;padding:16px;z-index:50}
#modal .m{width:100%;max-width:640px;max-height:92vh;overflow:auto;padding:22px}
.row3{display:grid;grid-template-columns:repeat(3,1fr);gap:12px}
.strats{display:grid;grid-template-columns:1fr 1fr;gap:8px}
.strats label{display:flex;gap:8px;align-items:center;background:var(--soft);padding:8px 10px;border-radius:10px;font-size:12.5px;cursor:pointer}
.strats input{width:auto;accent-color:var(--acc)}
#paste{border:1.5px dashed var(--line);border-radius:12px;padding:16px;text-align:center;color:var(--mute);font-size:12.5px;cursor:pointer}
#paste:focus{outline:none;border-color:var(--acc)}
kbd{background:var(--soft);padding:2px 6px;border-radius:5px;font-size:11px}
.img{display:flex;gap:10px;background:var(--soft);padding:10px;border-radius:12px;margin-top:8px}
.img img{width:96px;height:68px;object-fit:cover;border-radius:8px;cursor:zoom-in;flex:0 0 96px}
.img textarea{height:68px;resize:none;background:var(--card)}
#lb{position:fixed;inset:0;background:rgba(0,0,0,.85);z-index:60;display:flex;align-items:center;justify-content:center;padding:20px;cursor:zoom-out}
#lb img{max-width:100%;max-height:100%;border-radius:10px}

@media (max-width:900px){.kpis{grid-template-columns:repeat(2,1fr)}.split,.eq{grid-template-columns:1fr}.row3{grid-template-columns:1fr 1fr}}
@media (max-width:560px){.day,.blank{min-height:70px}.day .m{display:none}.day .v{font-size:10.5px}.cal,.dow{gap:4px}.strats{grid-template-columns:1fr}}
.kpi.main{background:rgba(0,246,255,.07);border:1px solid #00F6FF;box-shadow:0 0 22px rgba(0,246,255,.35),inset 0 0 22px rgba(0,246,255,.08)}
.kpi.main span{color:var(--mute)}
.kpi.main b{color:#00F6FF;text-shadow:0 0 14px rgba(0,246,255,.6)}
.logo{background:#00F6FF;color:#021018;box-shadow:0 0 18px rgba(0,246,255,.5)}
.btn.pri{background:linear-gradient(135deg,#00D9E6,#2563EB);border-color:transparent;color:#fff;box-shadow:0 0 18px rgba(0,246,255,.28)}
.tab.on{background:linear-gradient(135deg,#0891B2,#2563EB);color:#fff;box-shadow:0 0 16px rgba(0,246,255,.25)}
.day.g{background:var(--gain-soft);border:1px solid rgba(0,255,136,.7);box-shadow:0 0 14px rgba(0,255,136,.16),inset 0 0 14px rgba(0,255,136,.04)}
.day.l{background:var(--loss-soft);border:1px solid rgba(255,56,100,.7);box-shadow:0 0 14px rgba(255,56,100,.16),inset 0 0 14px rgba(255,56,100,.04)}
.mini{border:1px solid var(--line)}
.kpis.k3{grid-template-columns:repeat(3,1fr)}@media (max-width:700px){.kpis.k3{grid-template-columns:1fr}}
.tb{width:100%;border-collapse:collapse;min-width:460px}.tb th{font-size:11px;color:var(--mute);text-align:right;padding:8px 10px;font-weight:600}.tb th:first-child,.tb td:first-child{text-align:left}.tb td{text-align:right;padding:10px;border-top:1px solid var(--line);font-weight:700;font-size:13px}.tb tfoot td{font-weight:800;background:var(--acc-soft)}
</style>
</head>
<body>

<div id="auth">
  <div class="card box">
    <div class="logo">DL</div>
    <h1 style="font-size:20px;margin:16px 0 4px;font-weight:800">Diário do Léo</h1>
    <p class="mute" style="margin:0 0 20px;font-size:13px">Digite a senha de licença para entrar.</p>
    <input type="password" id="pw" placeholder="Senha" style="text-align:center;margin-bottom:10px">
    <button class="btn pri" style="width:100%" onclick="login()">Entrar</button>
    <p id="err" class="loss hidden" style="font-size:12px;font-weight:600;margin:10px 0 0">Senha incorreta.</p>
  </div>
</div>

<div id="app" class="wrap hidden">
  <div class="top">
    <div class="brand"><div class="logo">DL</div><div><h1 id="bTitle">Diário do Léo</h1><p>Mini Índice WIN · 2026</p></div></div>
    <div class="tabs" id="emp"></div>
    <div class="tabs" id="tabs"></div>
    <button class="btn" onclick="logout()">Sair</button>
  </div>

  <div class="kpis">
    <div class="card kpi"><span>Total positivo</span><b id="k-pos" class="gain">R$ 0,00</b></div>
    <div class="card kpi"><span>Total negativo</span><b id="k-neg" class="loss">R$ 0,00</b></div>
    <div class="card kpi main"><span>Saldo líquido</span><b id="k-net">R$ 0,00</b></div>
    <div class="card kpi"><span>Mesa · 1º ao 15º</span><b id="k-m1">R$ 0,00</b></div>
    <div class="card kpi"><span>Mesa · 16º ao fim</span><b id="k-m2">R$ 0,00</b></div>
    <div class="card kpi" style="background:var(--acc-soft)"><span>Mesa total</span><b id="k-mt">R$ 0,00</b></div>
  </div>

  <section id="v-calendar" class="card sec">
    <div class="bar">
      <div><h2>Calendário</h2><span class="mute" style="font-size:12px">Clique num dia para registrar resultado, operações, estratégias e prints.</span></div>
      <div class="nav">
        <button class="btn" onclick="stepMonth(-1)" aria-label="Mês anterior">‹</button>
        <select id="sel" onchange="changeMonth(this.value)"></select>
        <button class="btn" onclick="stepMonth(1)" aria-label="Próximo mês">›</button>
      </div>
    </div>
    <div class="split">
      <div>
        <div class="dow"><div>DOM</div><div>SEG</div><div>TER</div><div>QUA</div><div>QUI</div><div>SEX</div><div>SÁB</div></div>
        <div class="cal" id="cal"></div>
      </div>
      <div class="side">
        <h3>Resumo quinzenal</h3>
        <div class="mini"><span>Saldo · 1º ao 15º</span><b id="s-b1"></b></div>
        <div class="mini"><span>Mesa · 1º ao 15º</span><b id="s-m1"></b><small>Saldo − 30% (se positivo)</small></div>
        <div class="mini"><span>Saldo · 16º ao fim</span><b id="s-b2"></b></div>
        <div class="mini"><span>Mesa · 16º ao fim</span><b id="s-m2"></b><small>Saldo − 30% (se positivo)</small></div>
        <div class="mini" style="background:var(--acc-soft)"><span>Mesa total</span><b id="s-mt"></b><small>Soma das duas quinzenas</small></div>
      </div>
    </div>
  </section>

  <section id="v-performance" class="hidden">
    <div class="card sec"><div class="bar"><h2>Evolução semanal</h2></div><div class="chartbox"><canvas id="cWeek"></canvas></div></div>
    <div class="weeks" id="weeks"></div>
  </section>

  <section id="v-equity" class="hidden">
    <div class="bar" style="margin-bottom:12px"><div><h2 style="margin:0;font-size:17px;font-weight:800">Curva de evolução</h2><span class="mute" style="font-size:12px">Passe o cursor na curva para ver o Gain/Loss acumulado até o dia.</span></div></div>
    <div class="eq">
      <div class="card"><div class="chartbox" style="height:380px"><canvas id="cEq"></canvas></div></div>
      <div class="card"><div id="dTitle" class="mute" style="font-size:12px;font-weight:700;margin-bottom:8px">Gain vs Loss</div><div class="chartbox" style="height:320px"><canvas id="cDo"></canvas></div></div>
    </div>
  </section>

  <section id="v-calc" class="hidden">
    <div class="bar"><div><h2 style="margin:0;font-size:17px;font-weight:800">Calculadora WIN</h2><span class="mute" style="font-size:12px">Simule até 4 cenários. Base: R$ 0,20 por ponto por contrato.</span></div></div>
    <div class="calc" id="calc"></div>
  </section>

  <section id="v-anual" class="hidden">
    <div class="bar"><div><h2 style="margin:0;font-size:17px;font-weight:800">Resultado anual 2026</h2><span class="mute" style="font-size:12px">Mesa total de cada mês: soma das duas quinzenas (saldo − 30% quando positivo).</span></div></div>
    <div class="card kpi main" style="margin-bottom:16px"><span>Total do ano · Mesa</span><b id="an-total">R$ 0,00</b></div>
    <div class="weeks" id="an-months" style="margin-top:0"></div>
  </section>

  <section id="v-comp" class="hidden">
    <div class="bar"><div><h2 style="margin:0;font-size:17px;font-weight:800">Comparativo das empresas</h2><span class="mute" style="font-size:12px">Evolução semanal e soma anual (Mesa) da Empresa 1 e da Empresa 2.</span></div>
      <div class="nav"><select id="cmpSel" onchange="changeMonth(this.value)"></select></div></div>
    <div class="kpis k3" style="margin-bottom:16px">
      <div class="card kpi"><span>Empresa 1 · ano (Mesa)</span><b id="cp-1">R$ 0,00</b></div>
      <div class="card kpi"><span>Empresa 2 · ano (Mesa)</span><b id="cp-2">R$ 0,00</b></div>
      <div class="card kpi main"><span>Soma anual das duas empresas</span><b id="cp-s">R$ 0,00</b></div>
    </div>
    <div class="card sec"><div class="bar"><h2>Evolução semanal · <span id="cmpMonth"></span></h2></div><div class="chartbox"><canvas id="cCmp"></canvas></div></div>
    <div class="weeks" id="cmpWeeks"></div>
    <div class="card sec" style="margin-top:20px"><div class="bar"><h2>Mesa total por mês · 2026</h2></div>
      <div style="overflow-x:auto"><table class="tb"><thead><tr><th>Mês</th><th>Empresa 1</th><th>Empresa 2</th><th>Soma</th></tr></thead><tbody id="cmpRows"></tbody><tfoot id="cmpFoot"></tfoot></table></div></div>
  </section>
</div>

<div id="modal" class="hidden">
  <div class="card m">
    <div class="bar"><h2 id="mTitle">Detalhes do dia</h2><button class="btn" onclick="closeModal()">Fechar</button></div>
    <div style="display:flex;flex-direction:column;gap:16px">
      <div style="display:flex;justify-content:space-between;align-items:center;background:var(--soft);padding:10px 12px;border-radius:12px">
        <span class="mute" style="font-size:12.5px">Ditar resultado por voz</span>
        <button class="btn" onclick="voice()">🎙 Falar</button>
      </div>
      <div class="row3">
        <div><label class="l">RESULTADO (R$)</label><input type="number" step="0.01" id="iR" placeholder="250 ou -100"></div>
        <div><label class="l">OPERAÇÕES</label><input type="number" id="iT" placeholder="4"></div>
        <div><label class="l">CONTRATOS</label><input type="number" id="iC" placeholder="5"></div>
      </div>
            <div>
        <label class="l">RESULTADOS DAS OPERAÇÕES (R$)</label>
        <div style="display:flex;gap:8px"><input type="number" step="0.01" id="pIn" placeholder="Ex: 120 ou -50"><button class="btn pri" onclick="addPart()">Adicionar</button></div>
        <div id="pList" style="display:flex;flex-wrap:wrap;gap:6px;margin-top:8px"></div>
      </div>
      <div>
        <div style="display:flex;justify-content:space-between"><label class="l">PRINTS, GAIN/LOSS E NOTAS</label><span id="iCount" class="mute" style="font-size:11px;font-weight:700">0 / 5</span></div>
        <div id="paste" tabindex="0">Clique aqui e pressione <kbd>Ctrl+V</kbd> para colar um print</div>
        <div id="imgs"></div>
      </div>
      <div style="display:flex;justify-content:flex-end;gap:8px">
        <button class="btn" onclick="closeModal()">Cancelar</button>
        <button class="btn pri" onclick="saveDay()">Salvar</button>
      </div>
    </div>
  </div>
</div>
<div id="lb" class="hidden" onclick="this.classList.add('hidden')"><img alt="Print ampliado"></div>

<script>
const PASS="123456", Y=2026;
const MN=['Janeiro','Fevereiro','Março','Abril','Maio','Junho','Julho','Agosto','Setembro','Outubro','Novembro','Dezembro'];
const STRATS=['Linha com imbalance venda','Linha com imbalance de compra','Linha de alvo','Linha de rompimento importante','Linha de alvo 1','Linha de alvo total','Linha fundo do 60','Linha topo do 60'];
const TABS=[['calendar','Calendário'],['performance','Performance'],['equity','Equity'],['calc','Calculadora'],['anual','Resultado anual 2026'],['comp','Comparativo']];
let M=8,data={},day=null,imgs=[],ch={},mem={},parts=[];
const $=id=>document.getElementById(id);
const st={get(k){try{return localStorage.getItem(k)}catch(e){return mem[k]??null}},set(k,v){try{localStorage.setItem(k,v)}catch(e){mem[k]=v}},del(k){try{localStorage.removeItem(k)}catch(e){delete mem[k]}}};
const brl=v=>Number(v||0).toLocaleString('pt-BR',{style:'currency',currency:'BRL'});
const esc=s=>String(s??'').replace(/[&<>"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const dim=()=>new Date(Y,M+1,0).getDate();
let E=parseInt(st.get('win_empresa'))===2?2:1;
const kfor=m=>E===2?`win_trading_data_e2_${Y}_${m}`:`win_trading_data_${Y}_${m}`;
const key=()=>kfor(M);
const load=()=>{try{data=JSON.parse(st.get(key())||'{}')}catch(e){data={}}};
const save=()=>st.set(key(),JSON.stringify(data));
const sum=(a,b)=>{let s=0;for(let d=a;d<=Math.min(b,dim());d++){const r=parseFloat(data[d]?.result);if(!isNaN(r))s+=r}return s};
const net=v=>v>0?v*.7:v;
const css=n=>getComputedStyle(document.documentElement).getPropertyValue(n).trim();
const cls=(el,v)=>{el.classList.toggle('gain',v>=0);el.classList.toggle('loss',v<0)};

/* auth */
function login(){if($('pw').value===PASS){st.set('win_system_auth','true');init()}else{$('err').classList.remove('hidden');$('pw').value=''}}
function logout(){st.del('win_system_auth');$('app').classList.add('hidden');$('auth').classList.remove('hidden')}
$('pw').addEventListener('keydown',e=>{if(e.key==='Enter')login()});
function init(){$('auth').classList.add('hidden');$('app').classList.remove('hidden');paintEmp();renderCal()}

/* empresas */
$('emp').innerHTML=[1,2].map(n=>`<button class="tab" id="e-${n}" onclick="setEmpresa(${n})">Empresa ${n}</button>`).join('');
function paintEmp(){[1,2].forEach(i=>$('e-'+i).classList.toggle('on',i===E));$('bTitle').textContent='Diário do Léo · Empresa '+E;document.title='Diário do Léo · Empresa '+E}
function refresh(){renderCal();if(!$('v-performance').classList.contains('hidden'))renderPerf();if(!$('v-equity').classList.contains('hidden'))renderEq();if(!$('v-anual').classList.contains('hidden'))renderAnual();if(!$('v-comp').classList.contains('hidden'))renderComp()}
function setEmpresa(n){E=n;st.set('win_empresa',String(n));paintEmp();refresh()}

/* tabs */
$('tabs').innerHTML=TABS.map(([k,n],i)=>`<button class="tab${i?'':' on'}" id="t-${k}" onclick="tab('${k}')">${n}</button>`).join('');
function tab(k){TABS.forEach(([t])=>{$('t-'+t).classList.toggle('on',t===k);$('v-'+t).classList.toggle('hidden',t!==k)});
  if(k==='performance')renderPerf();if(k==='equity')renderEq();if(k==='anual')renderAnual();if(k==='comp')renderComp()}
$('sel').innerHTML=MN.map((n,i)=>`<option value="${i}"${i===M?' selected':''}>${n} ${Y}</option>`).join('');
function changeMonth(m){M=parseInt(m);$('sel').value=M;$('cmpSel').value=M;renderCal();
  if(!$('v-performance').classList.contains('hidden'))renderPerf();
  if(!$('v-equity').classList.contains('hidden'))renderEq();
  if(!$('v-comp').classList.contains('hidden'))renderComp()}
function stepMonth(d){const n=M+d;if(n>=0&&n<=11)changeMonth(n)}

/* calendar */
function renderCal(){
  load();const T=dim(),first=new Date(Y,M,1).getDay();let h='',pos=0,neg=0;
  for(let i=0;i<first;i++)h+='<div class="blank"></div>';
  for(let d=1;d<=T;d++){
    const x=data[d]||{},r=parseFloat(x.result),has=!isNaN(r);
    if(has){r>0?pos+=r:neg+=Math.abs(r)}
    const im=x.images||[],g=im.filter(i=>i.status==='gain').length,l=im.filter(i=>i.status==='loss').length,tg=g+l;
    h+=`<div class="day${has&&r>0?' g':''}${has&&r<0?' l':''}" onclick="openModal(${d})">
      <div class="n">${d}</div>
      <div>${has?`<div class="v ${r>=0?'gain':'loss'}">${brl(r)}</div>`:''}
      ${tg?`<div class="gl"><i style="width:${g/tg*100}%;background:var(--gain)"></i><i style="width:${l/tg*100}%;background:var(--loss)"></i></div>`:''}
      <div class="m">${x.trades||0} op · ${x.contracts||0} ct${im.length?' · 📷'+im.length:''}</div></div></div>`;
  }
  $('cal').innerHTML=h;
  const n=pos-neg,a=sum(1,15),b=sum(16,T),ma=net(a),mb=net(b);
  $('k-pos').textContent=brl(pos);$('k-neg').textContent=brl(neg);$('k-net').textContent=brl(n);
  [['k-m1',ma],['k-m2',mb],['k-mt',ma+mb],['s-b1',a],['s-m1',ma],['s-b2',b],['s-m2',mb],['s-mt',ma+mb]].forEach(([id,v])=>{$(id).textContent=brl(v);cls($(id),v)});
}

/* performance */
function renderPerf(){
  load();const T=dim();let wk=[],s=1,n=1;
  while(s<=T){const e=Math.min(s+6,T);wk.push({n,s,e});s=e+1;n++}
  const tot=w=>sum(w.s,w.e);
  if(ch.w)ch.w.destroy();
  ch.w=new Chart($('cWeek'),{type:'bar',
    data:{labels:wk.map(w=>'Semana '+w.n),datasets:[{data:wk.map(tot),backgroundColor:wk.map(w=>tot(w)>=0?css('--gain'):css('--loss')),borderRadius:8,maxBarThickness:70}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>' '+brl(c.raw)}}},
      scales:{x:{grid:{display:false},ticks:{color:css('--mute')}},y:{grid:{color:css('--line')},ticks:{color:css('--mute'),callback:v=>brl(v)}}}},
    plugins:[{id:'vals',afterDatasetsDraw(c){const x=c.ctx;x.save();x.font='700 12px Inter';x.textAlign='center';x.textBaseline='middle';x.fillStyle='#04121C';
      c.getDatasetMeta(0).data.forEach((b,i)=>{const v=c.data.datasets[0].data[i];x.fillText(brl(v),b.x,(b.y+b.base)/2)});x.restore()}}]});
  const WD=['Segunda','Terça','Quarta','Quinta','Sexta'];
  $('weeks').innerHTML=wk.map(w=>{
    const t=tot(w),nt=net(t);
    const rows=WD.map((nm,k)=>{
      let td=null;for(let d=w.s;d<=w.e;d++){if(new Date(Y,M,d).getDay()===k+1){td=d;break}}
      if(td===null)return `<div class="wrow off"><div><b style="font-size:12px">${nm}</b><small>Fora do período</small></div><span>—</span></div>`;
      const r=parseFloat(data[td]?.result),has=!isNaN(r);let G=0,L=0;
      for(let d=1;d<=T;d++){if(new Date(Y,M,d).getDay()===k+1){const q=parseFloat(data[d]?.result);if(!isNaN(q)){q>0?G++:q<0&&L++}}}
      const tt=G+L,gp=tt?Math.round(G/tt*100):0,lp=tt?Math.round(L/tt*100):0;
      return `<div class="wrow"><div><b style="font-size:12px">${nm} <span class="mute" style="font-weight:500">(${String(td).padStart(2,'0')})</span></b><small>Gain ${gp}% · Loss ${lp}%</small></div>
        <div style="text-align:right"><b class="${has?(r>=0?'gain':'loss'):'mute'}" style="font-size:12px">${has?brl(r):'—'}</b></div></div>`}).join('');
    return `<div class="card wk"><header><b>Semana ${w.n}</b><small>Dias ${String(w.s).padStart(2,'0')} a ${String(w.e).padStart(2,'0')}</small></header>
      <div class="two"><div class="mini"><span>Valor total</span><b class="${t>=0?'gain':'loss'}" style="font-size:14px">${brl(t)}</b></div>
      <div class="mini"><span>Valor líquido</span><b class="${nt>=0?'gain':'loss'}" style="font-size:14px">${brl(nt)}</b></div></div>${rows}</div>`}).join('');
}

/* equity */
function renderEq(){
  load();const T=dim(),lab=[],eq=[],lo=[];let c=0,cl=0;
  for(let d=1;d<=T;d++){lab.push(String(d).padStart(2,'0')+'/'+String(M+1).padStart(2,'0'));const r=parseFloat(data[d]?.result);if(!isNaN(r)){c+=r;if(r<0)cl+=Math.abs(r)}eq.push(c);lo.push(cl)}
  if(ch.e)ch.e.destroy();if(ch.d)ch.d.destroy();
  const ctx=$('cEq').getContext('2d'),gr=ctx.createLinearGradient(0,0,0,380);
  gr.addColorStop(0,'rgba(0,246,255,.28)');gr.addColorStop(1,'rgba(0,246,255,0)');
  ch.e=new Chart(ctx,{type:'line',
    data:{labels:lab,datasets:[{label:'Capital acumulado',data:eq,borderColor:css('--acc'),backgroundColor:gr,fill:true,tension:.3,borderWidth:2.5,pointRadius:2,pointHoverRadius:6},
      {label:'Loss acumulado',data:lo,borderColor:css('--loss'),borderDash:[5,5],borderWidth:1.5,pointRadius:0,tension:.3}]},
    options:{responsive:true,maintainAspectRatio:false,interaction:{mode:'index',intersect:false},
      onHover:(e,a,chart)=>{if(a.length)dough(a[0].index+1,T);else dough(T,T)},
      plugins:{legend:{labels:{color:css('--mute'),usePointStyle:true,boxWidth:8}},tooltip:{callbacks:{label:c=>` ${c.dataset.label}: ${brl(c.raw)}`}}},
      scales:{x:{grid:{display:false},ticks:{color:css('--mute'),maxTicksLimit:10}},y:{grid:{color:css('--line')},ticks:{color:css('--mute'),callback:v=>'R$ '+v}}}}});
  ch.d=new Chart($('cDo'),{type:'doughnut',
    data:{labels:['Gain','Loss'],datasets:[{data:[1,1],backgroundColor:[css('--gain'),css('--loss')],borderWidth:0,hoverOffset:6}]},
    options:{responsive:true,maintainAspectRatio:false,cutout:'72%',plugins:{legend:{position:'bottom',labels:{color:css('--mute'),usePointStyle:true,boxWidth:8}},tooltip:{callbacks:{label:c=>` ${c.label}: ${brl(c.raw)}`}}}},
    plugins:[{id:'ctr',afterDraw(c){const{ctx:x,chartArea:a}=c,cx=(a.left+a.right)/2,cy=(a.top+a.bottom)/2;x.save();x.textAlign='center';x.textBaseline='middle';x.font='800 17px Inter';
      x.fillStyle=css('--gain');x.fillText(brl(c.gv||0),cx,cy-12);x.fillStyle=css('--loss');x.fillText(brl(c.lv||0),cx,cy+12);x.restore()}}]});
  dough(T,T);
}
function dough(max,T){
  let g=0,l=0;for(let d=1;d<=max;d++){const r=parseFloat(data[d]?.result);if(!isNaN(r)){r>0?g+=r:l+=Math.abs(r)}}
  const c=ch.d;c.data.datasets[0].data=[g||.01,l||.01];c.gv=g;c.lv=l;c.update('none');
  $('dTitle').textContent=max===T?`Gain vs Loss · ${MN[M]} (total)`:`Gain vs Loss · até ${String(max).padStart(2,'0')}/${String(M+1).padStart(2,'0')}`;
}

/* resultado anual */
function monthData(m){try{return JSON.parse(st.get(kfor(m))||'{}')}catch(e){return {}}}
function renderAnual(){
  let tot=0,h='';
  MN.forEach((n,m)=>{
    const dd=monthData(m),T=new Date(Y,m+1,0).getDate();
    const s=(a,b)=>{let t=0;for(let d=a;d<=b;d++){const r=parseFloat(dd[d]?.result);if(!isNaN(r))t+=r}return t};
    const q1=net(s(1,15)),q2=net(s(16,T)),mt=q1+q2;tot+=mt;
    h+=`<div class="card wk"><header><b>${n}</b><small>${Y}</small></header>
      <div class="mini" style="background:var(--acc-soft);margin-bottom:10px"><span>Mesa total</span><b class="${mt>=0?'gain':'loss'}" style="font-size:18px">${brl(mt)}</b></div>
      <div class="wrow"><small>1ª quinzena</small><b class="${q1>=0?'gain':'loss'}" style="font-size:12px">${brl(q1)}</b></div>
      <div class="wrow"><small>2ª quinzena</small><b class="${q2>=0?'gain':'loss'}" style="font-size:12px">${brl(q2)}</b></div></div>`;
  });
  $('an-months').innerHTML=h;
  const el=$('an-total');el.textContent=brl(tot);el.style.color=tot<0?'var(--loss)':'';
}

/* comparativo */
$('cmpSel').innerHTML=MN.map((n,i)=>`<option value="${i}"${i===M?' selected':''}>${n} ${Y}</option>`).join('');
function mdE(e,m){try{return JSON.parse(st.get(e===2?`win_trading_data_e2_${Y}_${m}`:`win_trading_data_${Y}_${m}`)||'{}')}catch(x){return {}}}
const sumD=(dd,a,b)=>{let s=0;for(let d=a;d<=b;d++){const r=parseFloat(dd[d]?.result);if(!isNaN(r))s+=r}return s};
const mesaM=(dd,m)=>{const T=new Date(Y,m+1,0).getDate();return net(sumD(dd,1,15))+net(sumD(dd,16,T))};
const gc=v=>v>=0?'gain':'loss';
function renderComp(){
  const T=dim(),d1=mdE(1,M),d2=mdE(2,M);let wk=[],s=1,n=1;
  while(s<=T){const e=Math.min(s+6,T);wk.push({n,s,e});s=e+1;n++}
  const a=wk.map(w=>sumD(d1,w.s,w.e)),b=wk.map(w=>sumD(d2,w.s,w.e)),c=a.map((v,i)=>v+b[i]);
  $('cmpMonth').textContent=MN[M];
  if(ch.c)ch.c.destroy();
  ch.c=new Chart($('cCmp'),{type:'bar',
    data:{labels:wk.map(w=>'Semana '+w.n),datasets:[
      {label:'Empresa 1',data:a,backgroundColor:css('--acc'),borderRadius:6,maxBarThickness:46},
      {label:'Empresa 2',data:b,backgroundColor:'#8B5CF6',borderRadius:6,maxBarThickness:46},
      {label:'Soma das duas',data:c,backgroundColor:css('--gain'),borderRadius:6,maxBarThickness:46}]},
    options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{labels:{color:css('--mute'),usePointStyle:true,boxWidth:8}},tooltip:{callbacks:{label:x=>` ${x.dataset.label}: ${brl(x.raw)}`}}},
      scales:{x:{grid:{display:false},ticks:{color:css('--mute')}},y:{grid:{color:css('--line')},ticks:{color:css('--mute'),callback:v=>brl(v)}}}}});
  $('cmpWeeks').innerHTML=wk.map((w,i)=>`<div class="card wk"><header><b>Semana ${w.n}</b><small>Dias ${String(w.s).padStart(2,'0')} a ${String(w.e).padStart(2,'0')}</small></header>
    <div class="wrow"><small>Empresa 1</small><b class="${gc(a[i])}" style="font-size:12px">${brl(a[i])}</b></div>
    <div class="wrow"><small>Empresa 2</small><b class="${gc(b[i])}" style="font-size:12px">${brl(b[i])}</b></div>
    <div class="mini" style="background:var(--acc-soft);margin-top:8px"><span>Soma das duas</span><b class="${gc(c[i])}" style="font-size:16px">${brl(c[i])}</b></div></div>`).join('');
  let t1=0,t2=0,rows='';
  MN.forEach((nm,m)=>{const x=mesaM(mdE(1,m),m),y=mesaM(mdE(2,m),m);t1+=x;t2+=y;
    rows+=`<tr><td>${nm}</td><td class="${gc(x)}">${brl(x)}</td><td class="${gc(y)}">${brl(y)}</td><td class="${gc(x+y)}">${brl(x+y)}</td></tr>`});
  $('cmpRows').innerHTML=rows;
  $('cmpFoot').innerHTML=`<tr><td>Total do ano</td><td class="${gc(t1)}">${brl(t1)}</td><td class="${gc(t2)}">${brl(t2)}</td><td class="${gc(t1+t2)}">${brl(t1+t2)}</td></tr>`;
  [['cp-1',t1],['cp-2',t2],['cp-s',t1+t2]].forEach(([id,v])=>{const el=$(id);el.textContent=brl(v);if(id!=='cp-s')cls(el,v);else el.style.color=v<0?'var(--loss)':''});
}

/* calculator */
$('calc').innerHTML=[1,2,3,4].map(i=>`<div class="card"><b>Cenário ${i}</b>
  <div><label class="l">PONTOS</label><input type="number" id="cp${i}" placeholder="500" oninput="calcCol(${i})"></div>
  <div><label class="l">CONTRATOS</label><input type="number" id="cc${i}" value="1" oninput="calcCol(${i})"></div>
  <div class="res"><div><span class="mute">Bruto</span><b id="cb${i}">R$ 0,00</b></div><div><span class="mute">Mesa (−30%)</span><b id="cm${i}">R$ 0,00</b></div></div></div>`).join('');
function calcCol(i){
  const p=parseFloat($('cp'+i).value)||0,c=parseInt($('cc'+i).value)||1,b=p*c*.2,m=net(b);
  $('cb'+i).textContent=brl(b);$('cm'+i).textContent=brl(m);cls($('cb'+i),b);cls($('cm'+i),m);
}

/* modal */
function openModal(d){
  day=d;const x=data[d]||{};
  $('mTitle').textContent=`${String(d).padStart(2,'0')}/${String(M+1).padStart(2,'0')}/${Y}`;
  $('iR').value=x.result??'';$('iT').value=x.trades??'';$('iC').value=x.contracts??'';
  document.querySelectorAll('.sc').forEach(c=>c.checked=(x.strategies||[]).includes(c.value));
  imgs=[...(x.images||[])];renderImgs();parts=[...(x.parts||[])];renderParts();$('modal').classList.remove('hidden');
}
function closeModal(){$('modal').classList.add('hidden');day=null;renderCal()}
function saveDay(){
  if(!day)return;const r=$('iR').value,t=$('iT').value,c=$('iC').value;
  data[day]={result:r===''?'':parseFloat(r),trades:t===''?'':parseInt(t),contracts:c===''?'':parseInt(c),
    strategies:data[day]?.strategies||[],parts,images:imgs};
  save();closeModal();
}
$('paste').addEventListener('click',()=>$('paste').focus());
$('paste').addEventListener('paste',e=>{
  for(const it of (e.clipboardData?.items||[]))if(it.type.startsWith('image/')){
    if(imgs.length>=5){alert('Máximo de 5 imagens por dia.');return}
    const rd=new FileReader();rd.onload=ev=>{imgs.push({url:ev.target.result,status:'gain',note:''});renderImgs()};rd.readAsDataURL(it.getAsFile());e.preventDefault();break}
});
function renderImgs(){
  $('iCount').textContent=`${imgs.length} / 5`;
  $('imgs').innerHTML=imgs.map((im,i)=>`<div class="img"><img src="${im.url}" alt="Print ${i+1}" onclick="zoom(${i})">
    <div style="flex:1;display:flex;flex-direction:column;gap:6px;min-width:0">
      <div style="display:flex;gap:8px"><select style="padding:6px 10px" onchange="imgs[${i}].status=this.value"><option value="gain"${im.status==='gain'?' selected':''}>Gain</option><option value="loss"${im.status==='loss'?' selected':''}>Loss</option></select>
      <button class="btn" style="padding:6px 10px;color:var(--loss)" onclick="imgs.splice(${i},1);renderImgs()">Remover</button></div>
      <textarea placeholder="Notas sobre a operação..." oninput="imgs[${i}].note=this.value" style="height:52px">${esc(im.note)}</textarea></div></div>`).join('');
}
function zoom(i){const l=$('lb');l.querySelector('img').src=imgs[i].url;l.classList.remove('hidden')}
function addPart(){const v=parseFloat($('pIn').value);if(isNaN(v))return;parts.push(v);$('pIn').value='';renderParts();$('pIn').focus()}
function delPart(i){parts.splice(i,1);renderParts();if(!parts.length)$('iR').value=''}
function renderParts(){
  $('pList').innerHTML=parts.map((v,i)=>`<span style="display:inline-flex;gap:8px;align-items:center;padding:5px 6px 5px 11px;border-radius:999px;border:1px solid var(--line);background:var(--soft);font-weight:700;font-size:12px"><span class="${v>=0?'gain':'loss'}">${brl(v)}</span><button class="btn" style="padding:1px 8px;border-radius:999px" onclick="delPart(${i})" aria-label="Remover">×</button></span>`).join('');
  if(parts.length)$('iR').value=+parts.reduce((a,b)=>a+b,0).toFixed(2);
}
$('pIn').addEventListener('keydown',e=>{if(e.key==='Enter'){e.preventDefault();addPart()}});
function voice(){
  const SR=window.SpeechRecognition||window.webkitSpeechRecognition;
  if(!SR){alert('Seu navegador não suporta reconhecimento de voz.');return}
  const r=new SR();r.lang='pt-BR';r.start();
  r.onresult=e=>{const m=e.results[0][0].transcript.toLowerCase().match(/-?\d+[\.,]?\d*/);if(m)$('iR').value=parseFloat(m[0].replace(',','.'))};
}
document.addEventListener('keydown',e=>{if(e.key==='Escape'&&day)closeModal()});

if(st.get('win_system_auth')==='true')init();
</script>
</body>
</html>
