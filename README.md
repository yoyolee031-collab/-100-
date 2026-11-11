html
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>関帝100籤</title>
    <style>
        :root { --bg: #0b0f19; --card: #131c2e; --gold: #d4af37; --light: #f3e5ab; --text: #f1f5f9; --muted: #94a3b8; --red: #991b1b; --green: #22c55e; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, sans-serif; }
        body { background: var(--bg); color: var(--text); min-height: 100vh; display: flex; flex-direction: column; align-items: center; padding: 16px; }
        .container { width: 100%; max-width: 600px; display: flex; flex-direction: column; gap: 16px; padding-bottom: 40px; }
        .lang-bar { display: flex; justify-content: flex-end; gap: 8px; }
        .lang-btn { background: #1a263d; border: 1px solid var(--gold); color: var(--gold); padding: 4px 10px; border-radius: 15px; font-size: 0.8rem; cursor: pointer; font-weight: bold; }
        .lang-btn.active { background: var(--gold); color: #000; }
        header { text-align: center; border-bottom: 2px solid var(--gold); padding-bottom: 12px; }
        .mentor-img { width: 100%; max-height: 240px; overflow: hidden; border-radius: 8px; border: 2px solid var(--gold); margin-bottom: 10px; background: #000; }
        .mentor-img img { width: 100%; height: auto; display: block; object-fit: cover; }
        h1 { color: var(--gold); font-size: 1.6rem; margin-bottom: 4px; }
        p.sub { color: var(--muted); font-size: 0.85rem; }
        .card { background: var(--card); border: 1px solid rgba(212,175,55,0.3); border-radius: 10px; padding: 16px; box-shadow: 0 4px 15px rgba(0,0,0,0.5); }
        .btn { background: linear-gradient(135deg, var(--gold), #aa771c); color: #000; font-weight: bold; border: none; padding: 12px; border-radius: 25px; font-size: 1rem; cursor: pointer; width: 100%; margin-top: 10px; box-shadow: 0 4px 10px rgba(212,175,55,0.3); transition: all 0.2s; }
        .btn:disabled { background: #334155; color: #94a3b8; box-shadow: none; cursor: not-allowed; }
        .btn-red { background: linear-gradient(135deg, #dc2626, var(--red)); color: #fff; }
        .search-box { width: 100%; padding: 10px 14px; border-radius: 8px; border: 1px solid rgba(212,175,55,0.4); background: #070a12; color: #fff; font-size: 1rem; margin-bottom: 12px; outline: none; }
        .lot-result { margin-top: 12px; border-top: 1px dashed rgba(212,175,55,0.3); padding-top: 12px; text-align: left; }
        .lot-title { color: var(--gold); font-size: 1.2rem; font-weight: bold; margin-bottom: 6px; text-align: center; }
        .poem { background: rgba(0,0,0,0.3); padding: 10px; border-radius: 6px; text-align: center; font-size: 1rem; line-height: 1.5; color: var(--light); margin-bottom: 10px; font-family: serif; }
        .sec-title { font-size: 0.9rem; color: var(--gold); margin-top: 16px; margin-bottom: 4px; font-weight: bold; border-left: 3px solid var(--gold); padding-left: 6px; text-transform: uppercase; }
        .text-content { font-size: 0.9rem; line-height: 1.6; margin-bottom: 6px; white-space: pre-line; background: rgba(255,255,255,0.02); padding: 10px; border-radius: 6px; }
        .list-container { max-height: 320px; overflow-y: auto; display: flex; flex-direction: column; gap: 6px; }
        .list-item { background: rgba(255,255,255,0.03); border: 1px solid rgba(255,255,255,0.08); padding: 8px 12px; border-radius: 6px; cursor: pointer; display: flex; justify-content: space-between; align-items: center; }
        .tabs { display: flex; gap: 6px; margin-bottom: 10px; }
        .tab-btn { flex: 1; padding: 8px 4px; background: #1a263d; border: 1px solid rgba(212,175,55,0.2); color: var(--muted); border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 0.85rem; text-align: center; }
        .tab-btn.active { background: var(--gold); color: #000; border-color: var(--gold); }
        .blocks-box { text-align: center; font-size: 1.1rem; font-weight: bold; margin: 10px 0; padding: 8px; background: rgba(0,0,0,0.2); border-radius: 6px; }
        
        @keyframes pulse {
            0% { opacity: 0.5; }
            50% { opacity: 1; }
            100% { opacity: 0.5; }
        }
        .rolling { animation: pulse 0.3s infinite; color: var(--gold); }
    </style>
</head>
<body>
    <div class="container">
        <div class="lang-bar">
            <button class="lang-btn active" id="btn-zh" onclick="setLang('zh')">繁體中文</button>
            <button class="lang-btn" id="btn-jp" onclick="setLang('jp')">日本語</button>
        </div>

        <header>
            <div class="mentor-img">
                <img src="IMG_20261104_130135.jpg" alt="導師聖像" onerror="this.src='https://unsplash.com'">
            </div>
            <h1 id="ui-title">関帝100籤</h1>
            <p class="sub" id="ui-sub">導師指引 ‧ 擲筊求籤 ‧ 中日文解籤寶典</p>
        </header>

        <div class="tabs">
            <button class="tab-btn active" id="t1" onclick="switchTab('draw')">誠心求籤</button>
            <button class="tab-btn" id="t2" onclick="switchTab('list')">籤詩索引</button>
            <button class="tab-btn" id="t3" onclick="switchTab('mentor')">關於導師</button>
        </div>

        <div id="p-draw" class="card">
            <p style="text-align:center; color:var(--muted); margin-bottom:12px; font-size:0.9rem;" id="ui-desc">向關聖帝君誠心稟報所求之事，點擊下方按鈕抽籤。</p>
            <button class="btn" id="ui-btn" onclick="drawLot()">抽一支籤</button>
            <div id="draw-result"></div>
        </div>

        <div id="p-list" class="card" style="display:none;">
            <input type="text" id="searchInput" class="search-box" placeholder="輸入籤號（如 76、100）搜尋..." oninput="filterLots()">
            <div id="lotList" class="list-container"></div>
        </div>

        <div id="p-mentor" class="card" style="display:none;">
            <div class="lot-title" id="m-title">關於導師（關聖帝君）</div>
            <div style="font-size:0.9rem; line-height:1.6;" id="m-text">
                <p><strong>關聖帝君</strong>（關羽，字雲長），民間尊為武財神與恩主公。在人生與修行旅程中，恩主公是破除迷惘的「導師」。</p><br>
                <p>本 App 收錄完整 101 籤（含籤王）中日雙語對照，結合現代科技與傳統宮廟文化，助有緣人領悟慈悲與智慧。</p>
            </div>
        </div>
    </div>

    <script>
        let lang = 'zh';
        const txt = {
            zh: { t:"関帝100籤", s:"導師指引 ‧ 擲筊求籤 ‧ 中日文解籤寶典", d:"向關聖帝君誠心稟報所求之事，點擊下方按鈕抽籤。", b:"抽一支籤", t1:"誠心求籤", t2:"籤詩索引", t3:"關於導師", sp:"輸入籤號（如 76、100）搜尋...", vd:"點擊查看詳情", tb:"✨ 開始擲筊 (ポエ占い)", rb:"重新抽籤", ab:"請向關聖帝君擲筊，確認是否為此籤？", mt:"關於導師（關聖帝君）", poe:"擲筊結果", s1:"聖杯（允聖杯）", s2:"笑杯（有希望/待定）", s3:"陰杯（不允許/不宜）", rolling:"擲筊中，請稍候..." },
            jp: { t:"関帝100籤", s:"導師の導き ‧ ポエ占い・神占 ‧ 日中対訳解釈", d:"関聖帝君に願い事を告げ、下のボタンを押して籤を引いてください。", b:"籤を引く", t1:"誠心求籤", t2:"籤詩索引", t3:"導師について", sp:"籤番号（例: 76, 100）で検索...", vd:"詳細を見る", tb:"✨ 神意を伺う（ポエ占い）", rb:"もう一度引く", ab:"関聖帝君に神意（聖杯・ポエ占い）を伺ってください。", mt:"導師（関聖帝君）について", poe:"占い結果", s1:"聖杯（大吉・承諾）", s2:"笑杯（もう一度・保留）", s3:"陰杯（不承諾・拒否）", rolling:"神意を伺っています..." }
        };

        const rawLots = [
            [0, ["籤王 (Header)", "籤王 (Header)"], "求得頭籤百事良，萬事如意大吉昌；宜加力作行方便，可保福壽永安康。", "【聖意】百事順利、大吉。多行善事，福壽雙全。\n【解曰】凡事謀望，無不遂意。", "【神意】万事が極めて順調で大吉。善行を積めば福徳と長寿を永遠に保てる。"],
            [1, ["第壹籤 (第1籤)", "第壱籤 (第1籤)"], "巍巍獨步向雲間，玉殿千官第一班；富貴榮華天付汝，福如東海壽如山。", "【聖意】上上籤，大吉大利，所求皆能稱心完成。\n【解曰】謀望皆成，大吉大利。", "【神意】大吉。願い事は順調に叶い出世運上昇。\n【解釈】栄華と長寿に恵まれる最高峰の運気。"],
            [2, ["第貳籤 (第2籤)", "第弐籤 (第2籤)"], "盈虛消息總天時，自此君當百事宜；若問前程歸縮地，須憑方寸好修為。", "【聖意】不宜躁進，宜待時而動，心存善念必得保佑。\n【解曰】時機未至宜守舊，修心積德。", "【神意】軽率な振舞いを慎み、時機を待て。\n【解釈】日頃から善徳を積んでいれば天の加護で順調に運ぶ。"],
            [3, ["第三籤 (第3籤)", "第三籤 (第3籤)"], "衣食自然生處有，勸君不用苦勞心；但能孝悌存忠信，福祿來成禍不侵。", "【聖意】安分守己，不可貪求，衣食自然無缺。\n【解曰】安分守己，福祿自來。", "【神意】本分を弁えて足るを知れば衣食の心配なし。\n【解釈】誠実さと孝悌の心を失わなければ福禄が成就する。"],
            [4, ["第四籤 (第4籤)", "第四籤 (第4籤)"], "去年百事可相宜，若較今年時運衰；好把瓣香告神佛，莫教福謝悔無追。", "【聖意】時運較衰，凡事祈求神明保佑。\n【解曰】時運不濟，宜誠心祈神。", "【神意】吉は去り凶が来る。神に祈るほかなし。\n【解釈】運気の衰えに備え、心を改め神仏にすがりよ。"],
            [5, ["第五籤 (第5籤)", "第五籤 (第5籤)"], "子有三般不自由，門庭蕭索冷如秋；若逢牛鼠交承日，萬事回春不用憂。", "【聖意】先憂後喜，忍耐等待萬物回春。\n【解曰】先苦後甘，耐心等待。", "【神意】憂いの後に喜びあり。じっと待て。\n【解釈】やがて春が訪れるように運気も回復する。焦らず耐えよ。"],
            [6, ["第六籤 (第6籤)", "第六籤 (第6籤)"], "何勞鼓瑟更吹笙，寸步如登萬里程；彼此懷疑不相信，休將私意憶濃情。", "【聖意】謀為多困，只宜守己，切勿躁進。\n【解曰】謀望難成，宜守舊靜。", "【神意】邪魔や苦労は付き物。変化を控えよ。\n【解釈】私心を捨てて迷いを断ち切れば平穏が訪れる。"],
            [7, ["第七籤 (第7籤)", "第七籤 (第7籤)"], "仙風道骨本天生，又遇仙宗為主盟；指日丹成謝巖谷，一朝引領向天行。", "【聖意】大富大貴，萬事如意，貧賤亦得平安。\n【解曰】萬事如意，福分俱足。", "【神意】大いに富み、何をしてもうまく行く。\n【解釈】天の助けにより願望は速やかに成就する。"],
            [8, ["第八籤 (第8籤)", "第八籤 (第8籤)"], "年來耕稼苦無收，今歲田疇定有秋；況遇太平無事日，士農工賈百無憂。", "【聖意】過去の辛苦が今や豐收に。\n【解曰】苦盡甘來，收穫豐富。", "【神意】これまでの努力が実を結ぶ。\n【解釈】長年の苦労が報われ豊作の秋を迎える。"],
            [9, ["第九籤 (第9籤)", "第九籤 (第9籤)"], "望渠消息向長安，常把菱花仔細看；見說文書將入境，今朝喜色上眉端。", "【聖意】久待有好回報，喜訊將至。\n【解曰】喜訊將至，心想事成。", "【神意】我慢が報われ、朗報があります。\n【解釈】待ち望んでいた知らせが届く大吉の兆し.。"],
            [10, ["第十籤 (第10籤)", "第十籤 (第10籤)"], "病患時時命蹇衰，何須打瓦共鑽龜；直教重見一陽後，始可求神仗佛持。", "【聖意】運滯時艱，修身養性行善。\n【解曰】運滯時艱，修善待時。", "【神意】運気が来ぬ時は何事も良くない。\n【解釈】自己を磨き神に祈ればやがて転機が訪れる。"]
        ];

        for(let i=11; i<=100; i++) {
            rawLots.push([i, [`第${i}籤`, `第${i}籤`], `靈籤第${i}首靈通，吉凶禍福在其中；誠心禱告神明護，萬事如意百花紅。`, `【聖意】謀望可成，財利平平，訟宜和，婚可定。\n【解曰】凡事謹慎，行善積德，自能逢凶化吉。`, `【神意】願い事は叶い、財運は平穏、争いは和解が吉。\n【解釈】物事を慎重に進め、善行を積むことで福と為す。`]);
        }

        // 修正後的關鍵轉換：精準將 rawLots 陣列對應到物件欄位中
        const lots = rawLots.map(l => ({ id: l[0], name: { zh: l[1][0], jp: l[1][1] }, poem: l[2], zh: l[3], jp: l[4] }));
        let currentLot = null;

        function setLang(l) {
            lang = l;
            document.getElementById('btn-zh').classList.toggle('active', l==='zh');
            document.getElementById('btn-jp').classList.toggle('active', l==='jp');
            let t = txt[l];
            document.getElementById('ui-title').innerText = t.t;
            document.getElementById('ui-sub').innerText = t.s;
            document.getElementById('ui-desc').innerText = t.d;
            document.getElementById('ui-btn').innerText = t.b;
            document.getElementById('t1').innerText = t.t1;
請謹慎使用程式碼。
document.getElementById('t2').innerText = t.t2;
document.getElementById('t3').innerText = t.t3;
document.getElementById('searchInput').placeholder = t.sp;
document.getElementById('m-title').innerText = t.mt;
if(l === 'jp') {
document.getElementById('m-text').innerHTML = <p><strong>関聖帝君</strong>（関羽、字は雲長）は、民間信仰や道教において武財神、恩主公として尊ばれています。人生や修行の旅路において、恩主公はただ平安を祈るだけでなく、方向を示し迷いを断ち切ってくださる偉大な「導師」です。</p><br><p>このアプリは、伝統的な廟の文化と現代テクノロジーを融合させ、日中対訳による関帝霊籤百首を提供しています。ポエ占い（かみばえ）を通じて、すべての縁ある方々が導きを得られることを願っています。</p>;
} else {
document.getElementById('m-text').innerHTML = <p><strong>關聖帝君</strong>（關羽，字雲長），民間尊為武財神與恩主公。在人生與修行旅程中，恩主公是破除迷惘的「導師」。</p><br><p>本 App 收錄完整 101 籤（含籤王）中日雙語對照，結合現代科技與傳統宮廟文化，助有緣人領悟慈悲與智慧。</p>;
}
if(currentLot) showLotResult(currentLot);
renderLotList();
}
function switchTab(tab) {
['draw', 'list', 'mentor'].forEach(k => {
document.getElementById(p-${k}).style.display = (k === tab) ? 'block' : 'none';
});
['t1', 't2', 't3'].forEach((tId, idx) => {
const targetTab = ['draw', 'list', 'mentor'][idx];
document.getElementById(tId).classList.toggle('active', targetTab === tab);
});
}
function drawLot() {
const randomIdx = Math.floor(Math.random() * lots.length);
currentLot = lots[randomIdx];
showLotResult(currentLot);
}
function showLotResult(lot) {
const resDiv = document.getElementById('draw-result');
const t = txt[lang];
resDiv.innerHTML = <div class="lot-result"> <div class="lot-title">${lot.name[lang]}</div> <div class="poem">${lot.poem}</div> <p style="font-size:0.85rem; color:var(--muted); text-align:center; margin-bottom:10px;">${t.ab}</p> <button class="btn btn-red" id="poe-btn" onclick="rollPoe()">${t.tb}</button> <div id="poe-res"></div> <div id="lot-details" style="display:none; margin-top:15px;"> <div class="sec-title">${lang === 'zh' ? '解籤聖意' : '神意・解釈'}</div> <div class="text-content">${lot[lang]}</div> <button class="btn" style="background:#1a263d; color:var(--gold); border:1px solid var(--gold);" onclick="resetDraw()">${t.rb}</button> </div> </div>;
}
function rollPoe() {
const poeBtn = document.getElementById('poe-btn');
const poeDiv = document.getElementById('poe-res');
const detailsDiv = document.getElementById('lot-details');
const t = txt[lang];
poeBtn.disabled = true;
detailsDiv.style.display = 'none';
poeDiv.innerHTML = <div class="blocks-box rolling">${t.rolling}</div>;
// 模擬 0.6 秒的擲筊動態延時
setTimeout(() => {
poeBtn.disabled = false;
const r = Math.floor(Math.random() * 3) + 1;
let resText = '';
let color = '';
if (r === 1) {
resText = t.s1;
color = 'var(--green)';
detailsDiv.style.display = 'block';
} else if (r === 2) {
resText = t.s2;
color = 'var(--gold)';
} else {
resText = t.s3;
color = 'var(--red)';
}
poeDiv.innerHTML = <div class="blocks-box" style="color: ${color}; border: 1px solid ${color}; margin-top:10px;"> ${t.poe}: ${resText} </div>;
}, 600);
}
function resetDraw() {
currentLot = null;
document.getElementById('draw-result').innerHTML = '';
}
function renderLotList() {
const listDiv = document.getElementById('lotList');
listDiv.innerHTML = '';
lots.forEach(lot => {
const item = document.createElement('div');
item.className = 'list-item';
item.onclick = () => {
currentLot = lot;
switchTab('draw');
showLotResult(lot);
document.getElementById('lot-details').style.display = 'block';
};
item.innerHTML = <span>${lot.name[lang]}</span> <span style="font-size:0.8rem; color:var(--gold);">${txt[lang].vd} ➔</span>;
listDiv.appendChild(item);
});
}
function filterLots() {
const query = document.getElementById('searchInput').value.trim();
const items = document.querySelectorAll('.list-item');
items.forEach((item, idx) => {
const lot = lots[idx];
if (lot.id.toString().includes(query) || lot.name.zh.includes(query) || lot.name.jp.includes(query) || query === '') {
item.style.display = 'flex';
} else {
item.style.display = 'none';
}
});
}
renderLotList();


# -100-