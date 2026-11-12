<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="theme-color" content="#101827">
<title>TruckProfit — Owner-Operator Load Calculator</title>
<style>
*{box-sizing:border-box}body{margin:0;background:#f3f6fa;color:#172033;font-family:Arial,sans-serif}header{background:#101827;color:#fff;padding:24px 18px}header,main{width:100%}header h1{margin:0;font-size:25px}header p{margin:7px 0 0;color:#c4cede;font-size:14px}main{max-width:900px;margin:22px auto;padding:0 14px}.card{background:#fff;padding:19px;border-radius:14px;margin-bottom:16px;box-shadow:0 3px 14px #1522380d}h2{font-size:18px;margin:0 0 15px}.sub{font-size:13px;color:#64748b;line-height:1.5;margin:-7px 0 16px}.grid{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:13px}.field label{display:block;font-size:13px;font-weight:bold;color:#475569;margin-bottom:6px}.field input{width:100%;padding:12px;border:1px solid #d6deea;border-radius:8px;font-size:16px;color:#172033;background:white}.field input:focus{outline:2px solid #2563eb;border-color:transparent}.wide{grid-column:1/-1}button{border:0;border-radius:9px;padding:13px 15px;font-size:14px;font-weight:bold;cursor:pointer}.primary{width:100%;background:#1765d1;color:white;margin-top:17px}.secondary{background:#e9eef6;color:#263449}.danger{background:#fee2e2;color:#991b1b}.actions{display:flex;gap:9px;flex-wrap:wrap;margin-top:12px}.results{display:grid;grid-template-columns:repeat(2,minmax(0,1fr));gap:10px}.metric{background:#f7f9fc;padding:13px;border-radius:10px}.metric span{display:block;color:#64748b;font-size:12px;margin-bottom:5px}.metric strong{font-size:20px;overflow-wrap:anywhere}.decision{margin-top:13px;padding:14px;border-radius:10px;font-weight:bold}.take{background:#dcfce7;color:#166534}.negotiate{background:#fef3c7;color:#92400e}.reject{background:#fee2e2;color:#991b1b}.small{font-size:12px;color:#64748b;line-height:1.5;margin-top:12px}table{width:100%;border-collapse:collapse;font-size:13px}th,td{text-align:left;padding:10px 7px;border-bottom:1px solid #e5eaf1;white-space:nowrap}th{color:#475569}.tablewrap{overflow-x:auto}.empty{color:#64748b;font-size:14px}.footer{text-align:center;color:#718096;font-size:12px;padding:5px 10px 25px}@media(max-width:520px){.grid,.results{grid-template-columns:1fr}.wide{grid-column:auto}.metric strong{font-size:19px}}
</style>
</head>
<body>
<header><h1>TruckProfit</h1><p>Know your real numbers before you accept a load.</p></header>
<main>
<section class="card">
<h2>Load details</h2>
<p class="sub">Enter the load offer and your operating-cost estimates. You can change the default assumptions to match your truck.</p>
<div class="grid">
<div class="field"><label for="name">Load / broker (optional)</label><input id="name" type="text" placeholder="e.g. Dallas to Atlanta"></div>
<div class="field"><label for="payment">Load payment ($)</label><input id="payment" type="number" min="0" step="0.01" value="2500"></div>
<div class="field"><label for="loaded">Loaded miles</label><input id="loaded" type="number" min="0" step="1" value="800"></div>
<div class="field"><label for="deadhead">Deadhead miles</label><input id="deadhead" type="number" min="0" step="1" value="100"></div>
<div class="field"><label for="fuel">Diesel price ($/gallon)</label><input id="fuel" type="number" min="0" step="0.01" value="3.80"></div>
<div class="field"><label for="mpg">Fuel economy (MPG)</label><input id="mpg" type="number" min="0.1" step="0.1" value="6.5"></div>
<div class="field"><label for="fixed">Fixed operating cost ($/mile)</label><input id="fixed" type="number" min="0" step="0.01" value="0.65"></div>
<div class="field"><label for="other">Other trip costs ($)</label><input id="other" type="number" min="0" step="0.01" value="150"></div>
<div class="field"><label for="dispatch">Dispatch fee (%)</label><input id="dispatch" type="number" min="0" max="100" step="0.1" value="5"></div>
<div class="field"><label for="factoring">Factoring fee (%)</label><input id="factoring" type="number" min="0" max="100" step="0.1" value="3"></div>
<div class="field"><label for="target">Target profit per total mile ($)</label><input id="target" type="number" min="0" step="0.01" value="0.50"></div>
<div class="field"><label for="marginTarget">Target profit margin (%)</label><input id="marginTarget" type="number" min="0" max="100" step="1" value="15"></div>
</div>
<button class="primary" id="calculate">Calculate load</button>
<p class="small">Fixed cost per mile is your estimate for costs such as truck payment, insurance, maintenance reserves and other overhead. Avoid entering the same expense again under other trip costs. This calculator is an estimate, not financial or tax advice.</p>
</section>
<section class="card" id="resultCard">
<h2>Load analysis</h2>
<div class="results">
<div class="metric"><span>Estimated net profit</span><strong id="profit">$—</strong></div>
<div class="metric"><span>Profit per total mile</span><strong id="ppm">$—</strong></div>
<div class="metric"><span>Break-even rate per loaded mile</span><strong id="breakEven">$—</strong></div>
<div class="metric"><span>Profit margin</span><strong id="margin">—</strong></div>
<div class="metric"><span>Total miles</span><strong id="totalMiles">—</strong></div>
<div class="metric"><span>Estimated fuel cost</span><strong id="fuelCost">$—</strong></div>
</div>
<div id="decision" class="decision negotiate">Enter your load details and calculate.</div>
<p class="small" id="explain"></p>
<div class="actions"><button class="secondary" id="save">Save this load</button><button class="secondary" id="print">Print / Save PDF</button></div>
</section>
<section class="card">
<h2>Saved load comparison</h2>
<p class="sub">Saved loads stay in this browser on this device. Export a CSV to keep a separate copy.</p>
<div class="actions" style="margin:0 0 12px"><button class="secondary" id="csv">Export CSV</button><button class="danger" id="clear">Clear saved loads</button></div>
<div class="tablewrap" id="savedWrap"><p class="empty">No saved loads yet.</p></div>
</section>
</main>
<div class="footer">TruckProfit • Estimates only. Verify every rate, fee and operating cost before booking.</div>
<script>
const $=id=>document.getElementById(id);
let current=null;
const num=id=>Math.max(0,Number($(id).value)||0);
const money=n=>new Intl.NumberFormat('en-US',{style:'currency',currency:'USD'}).format(n);
function calculate(){
 const payment=num('payment'),loaded=num('loaded'),deadhead=num('deadhead'),fuel=num('fuel'),mpg=num('mpg'),fixed=num('fixed'),other=num('other'),dispatch=num('dispatch')/100,factoring=num('factoring')/100,target=num('target'),marginTarget=num('marginTarget')/100;
 const miles=loaded+deadhead;
 if(loaded<=0||miles<=0||mpg<=0){$('decision').className='decision reject';$('decision').textContent='Enter loaded miles and a valid MPG above zero.';return}
 if(dispatch+factoring>=1){$('decision').className='decision reject';$('decision').textContent='Dispatch and factoring fees combined must be below 100%.';return}
 const fuelCost=miles/mpg*fuel, fixedCost=miles*fixed, fees=payment*(dispatch+factoring), costs=fuelCost+fixedCost+other+fees, profit=payment-costs, ppm=profit/miles, margin=payment>0?profit/payment:0, breakEven=(fuelCost+fixedCost+other)/(loaded*(1-dispatch-factoring)), targetRate=(fuelCost+fixedCost+other+target*miles)/(loaded*(1-dispatch-factoring));
 let verdict,detail,cls;
 if(profit<0){verdict='REJECT — this load is estimated to lose money.';detail='The offered payment does not cover the estimated costs and fees.';cls='reject'}
 else if(ppm<target||margin<marginTarget){verdict='NEGOTIATE — the load is profitable but below your targets.';detail='Aim for at least '+money(targetRate)+' per loaded mile to reach your profit-per-mile target, based on these inputs.';cls='negotiate'}
 else{verdict='TAKE — this load meets the targets you entered.';detail='Check appointment times, route, broker credit and all accessorial details before accepting.';cls='take'}
 $('profit').textContent=money(profit);$('ppm').textContent=money(ppm);$('breakEven').textContent=money(breakEven);$('margin').textContent=(margin*100).toFixed(1)+'%';$('totalMiles').textContent=miles.toLocaleString('en-US');$('fuelCost').textContent=money(fuelCost);$('decision').className='decision '+cls;$('decision').textContent=verdict;$('explain').textContent=detail+' Estimated costs include fuel, fixed cost per mile, other trip costs, dispatch and factoring fees.';
 current={name:$('name').value||'Unnamed load',payment,loaded,deadhead,fuel,mpg,fixed,other,dispatch:dispatch*100,factoring:factoring*100,target,marginTarget:marginTarget*100,totalMiles:miles,fuelCost,profit,ppm,breakEven,margin:margin*100,verdict:verdict.split(' —')[0],targetRate};
}
function getSaved(){try{return JSON.parse(localStorage.getItem('truckprofitLoads')||'[]')}catch(e){return[]}}
function renderSaved(){
 const data=getSaved();if(!data.length){$('savedWrap').innerHTML='<p class="empty">No saved loads yet.</p>';return}
 $('savedWrap').innerHTML='<table><thead><tr><th>Load</th><th>Payment</th><th>Profit</th><th>$/mile</th><th>Decision</th><th></th></tr></thead><tbody>'+data.map((x,i)=>'<tr><td>'+escapeHtml(x.name)+'</td><td>'+money(x.payment)+'</td><td>'+money(x.profit)+'</td><td>'+money(x.ppm)+'</td><td>'+x.verdict+'</td><td><button class="danger" data-del="'+i+'" style="padding:7px">Delete</button></td></tr>').join('')+'</tbody></table>';
 document.querySelectorAll('[data-del]').forEach(b=>b.addEventListener('click',()=>{const a=getSaved();a.splice(Number(b.dataset.del),1);localStorage.setItem('truckprofitLoads',JSON.stringify(a));renderSaved()}));
}
function escapeHtml(s){return String(s).replace(/[&<>"']/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]))}
$('calculate').addEventListener('click',calculate);
$('save').addEventListener('click',()=>{if(!current){calculate();if(!current)return}const a=getSaved();a.unshift(current);localStorage.setItem('truckprofitLoads',JSON.stringify(a.slice(0,50)));renderSaved();$('save').textContent='Saved!';setTimeout(()=>{$('save').textContent='Save this load'},1400)});
$('print').addEventListener('click',()=>window.print());
$('clear').addEventListener('click',()=>{if(confirm('Delete all saved loads from this browser?')){localStorage.removeItem('truckprofitLoads');renderSaved()}});
$('csv').addEventListener('click',()=>{const a=getSaved();if(!a.length){alert('Save a load first.');return}const cols=['name','payment','loaded','deadhead','totalMiles','fuelCost','profit','ppm','breakEven','margin','verdict'];const rows=[cols.join(','),...a.map(x=>cols.map(k=>'"'+String(x[k]??'').replace(/"/g,'""')+'"').join(','))];const blob=new Blob([rows.join('\n')],{type:'text/csv'}),url=URL.createObjectURL(blob),link=document.createElement('a');link.href=url;link.download='truckprofit-loads.csv';link.click();URL.revokeObjectURL(url)});
calculate();renderSaved();
</script>
</body>
</html>
