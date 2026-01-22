# Panpansuan
盘盘算（Panpansuan） · Sushiro Plate Calculator 是一个用于计算寿司郎（Sushiro）用餐价格的前端小工具。 通过点击不同颜色的盘子按钮，用户可以快速统计盘数并实时计算总金额，同时支持添加自定义价格的单点商品。  本项目为纯前端实现，无需后端，可直接在浏览器中打开使用，适合个人使用、学习展示或课程作业。
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>盘盘算 · Sushiro Plate Calculator</title>
    <style>
        :root {
            --s-white: #ffffff;
            --s-red: #e63946;
            --s-silver: #adb5bd;
            --s-yellow: #ffca3a;
            --s-black: #212529;
            --bg-color: #f2f2f7;
            --text-color: #1c1c1e;
            --card-bg: #ffffff;
            --accent-red: #d0021b;
        }

        * {
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
            background-color: var(--bg-color);
            color: var(--text-color);
            margin: 0;
            padding: 12px;
            display: flex;
            justify-content: center;
            min-height: 100vh;
        }

        .container {
            width: 100%;
            max-width: 450px;
            background: var(--card-bg);
            padding: 20px 16px;
            border-radius: 24px;
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
        }

        .header-section {
            text-align: center;
            margin-bottom: 24px;
            padding-bottom: 12px;
            border-bottom: 1px solid #eee;
        }

        .main-title {
            font-size: 20px;
            margin: 0;
            font-weight: 700;
        }

        .sub-title {
            font-size: 14px;
            margin-top: 4px;
            display: block;
        }

        .brand-name {
            color: var(--accent-red);
            font-weight: 800;
        }

        .vision-text {
            color: #8e8e93;
        }

        /* 盘子选择网格 - 第一行3个，第二行2个居中 */
        .plate-grid {
            display: grid;
            grid-template-columns: repeat(6, 1fr);
            gap: 12px;
            margin-bottom: 24px;
        }

        .plate-item {
            grid-column: span 2;
            display: flex;
            flex-direction: column;
            align-items: center;
            background: #fafafa;
            padding: 12px 4px;
            border-radius: 16px;
            border: 1px solid #f0f0f0;
        }

        .plate-item:nth-child(4) {
            grid-column: 2 / span 2;
        }
        .plate-item:nth-child(5) {
            grid-column: 4 / span 2;
        }

        .plate-btn {
            width: 58px;
            height: 58px;
            border-radius: 50%;
            border: 3px solid #eee;
            cursor: pointer;
            transition: transform 0.1s;
            box-shadow: 0 3px 8px rgba(0,0,0,0.12);
            margin-bottom: 8px;
        }

        .plate-btn:active {
            transform: scale(0.9);
        }

        .white { background-color: var(--s-white); border-color: #ddd; }
        .red { background-color: var(--s-red); border-color: #c4303b; }
        .silver { background-color: var(--s-silver); border-color: #9098a0; }
        .yellow { background-color: var(--s-yellow); border-color: #e5b634; }
        .black { background-color: var(--s-black); border-color: #000; }

        .plate-count { 
            font-size: 24px; 
            font-weight: 800; 
            color: var(--text-color);
            margin: 4px 0;
        }

        .counter-controls {
            display: flex;
            justify-content: center;
            width: 100%;
            margin-top: 4px;
        }

        .ctrl-btn-minus {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: 1px solid #ddd;
            background: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
            font-weight: bold;
            color: #8e8e93;
            cursor: pointer;
        }

        .ctrl-btn-minus:active { background: #f2f2f7; }

        .plate-price { font-size: 12px; color: #8e8e93; margin-top: 2px; font-weight: 500; }

        .ai-section {
            margin-bottom: 20px;
            padding: 14px;
            background: #f0f7ff;
            border-radius: 18px;
            border: 1px solid #d0e3ff;
        }

        .ai-buttons {
            display: flex;
            gap: 8px;
            margin-bottom: 10px;
        }

        .btn-ai {
            flex: 1;
            padding: 10px 4px;
            border: none;
            border-radius: 10px;
            font-weight: 600;
            font-size: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 4px;
        }

        .btn-ai-analyze { background: #007aff; color: white; }
        .btn-ai-suggest { background: #ff9500; color: white; }

        .ai-response {
            font-size: 12px;
            color: #3a3a3c;
            background: rgba(255,255,255,0.7);
            padding: 10px;
            border-radius: 10px;
            min-height: 34px;
        }

        /* 重新设计的紧凑单点区域 */
        .others-section { 
            background: #f9f9f9;
            padding: 12px;
            border-radius: 16px;
            margin-bottom: 20px;
        }

        .input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .input-row {
            display: flex;
            gap: 6px;
        }

        input {
            padding: 8px 10px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 13px;
            -webkit-appearance: none;
            outline: none;
            background: white;
            width: 100%;
        }

        /* 设置比例使两个框加起来和按钮宽度一致 */
        input[type="text"] { flex: 2; }
        input[type="number"] { flex: 1; }

        .btn-add { 
            width: 100%;
            padding: 10px; 
            background-color: #34c759; 
            color: white; 
            border: none; 
            border-radius: 8px; 
            font-weight: 600;
            font-size: 13px;
            cursor: pointer;
        }

        .btn-add:active { opacity: 0.8; }

        .others-list { max-height: 120px; overflow-y: auto; margin-top: 8px; }
        .other-item { 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            padding: 6px 0; 
            border-bottom: 1px solid #eee;
            font-size: 13px; 
        }
        .remove-btn { color: #ff3b30; padding: 4px 8px; font-weight: bold; }

        .summary { 
            background: #fff5f5; 
            padding: 16px; 
            border-radius: 20px; 
            text-align: center; 
            border: 1px solid #ffebeb;
        }

        .total-amount { 
            font-size: 36px; 
            font-weight: 900; 
            color: var(--accent-red); 
            margin: 4px 0; 
        }

        .total-label {
            font-size: 14px; 
            color: #1c1c1e; 
            font-weight: 800;
        }

        .actions { margin-top: 16px; display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
        .btn-reset, .btn-export { 
            padding: 16px; 
            border: none; 
            border-radius: 14px; 
            font-weight: 700; 
            font-size: 15px; 
        }
        .btn-reset { background-color: #8e8e93; color: white; }
        .btn-export { background-color: var(--accent-red); color: white; }

        .footer-note {
            margin-top: 20px;
            text-align: center;
            font-size: 11px;
            color: #c7c7cc;
        }

        .modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.7); align-items: flex-end; }
        .modal-content { 
            background-color: white; 
            padding: 24px 16px 40px; 
            border-radius: 24px 24px 0 0; 
            width: 100%; 
            max-height: 90vh; 
            overflow-y: auto; 
            animation: slideUp 0.3s ease-out;
        }
        @keyframes slideUp { from { transform: translateY(100%); } to { transform: translateY(0); } }

        .receipt-table { width: 100%; border-collapse: collapse; margin-top: 15px; }
        .receipt-table th { text-align: left; border-bottom: 1px solid #eee; padding: 10px 5px; font-size: 12px; color: #8e8e93; }
        .receipt-table td { padding: 12px 5px; border-bottom: 1px solid #f9f9f9; font-size: 14px; }

        #toast { visibility: hidden; background-color: rgba(0,0,0,0.8); color: #fff; text-align: center; border-radius: 50px; padding: 10px 20px; position: fixed; z-index: 2000; bottom: 100px; left: 50%; transform: translateX(-50%); font-size: 14px; }
        #toast.show { visibility: visible; animation: fade 2s; }

        .loading-spinner {
            display: inline-block;
            width: 14px;
            height: 14px;
            border: 2px solid rgba(0,0,0,.1);
            border-radius: 50%;
            border-top-color: #007aff;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { to { transform: rotate(360deg); } }
    </style>
</head>
<body>

<div class="container">
    <div class="header-section">
        <h1 class="main-title">盘盘算 · Sushiro</h1>
        <div class="sub-title">
            <span class="brand-name">スシロ</span><span class="vision-text">-vision</span>
        </div>
    </div>

    <!-- 盘子网格 -->
    <div class="plate-grid">
        <div class="plate-item">
            <div class="plate-btn white" onclick="adjustPlate('white', 1)"></div>
            <div class="plate-price">¥8</div>
            <div class="plate-count" id="count-white">0</div>
            <div class="counter-controls">
                <div class="ctrl-btn-minus" onclick="adjustPlate('white', -1)">−</div>
            </div>
        </div>
        <div class="plate-item">
            <div class="plate-btn red" onclick="adjustPlate('red', 1)"></div>
            <div class="plate-price">¥10</div>
            <div class="plate-count" id="count-red">0</div>
            <div class="counter-controls">
                <div class="ctrl-btn-minus" onclick="adjustPlate('red', -1)">−</div>
            </div>
        </div>
        <div class="plate-item">
            <div class="plate-btn silver" onclick="adjustPlate('silver', 1)"></div>
            <div class="plate-price">¥15</div>
            <div class="plate-count" id="count-silver">0</div>
            <div class="counter-controls">
                <div class="ctrl-btn-minus" onclick="adjustPlate('silver', -1)">−</div>
            </div>
        </div>
        <div class="plate-item">
            <div class="plate-btn yellow" onclick="adjustPlate('yellow', 1)"></div>
            <div class="plate-price">¥20</div>
            <div class="plate-count" id="count-yellow">0</div>
            <div class="counter-controls">
                <div class="ctrl-btn-minus" onclick="adjustPlate('yellow', -1)">−</div>
            </div>
        </div>
        <div class="plate-item">
            <div class="plate-btn black" onclick="adjustPlate('black', 1)"></div>
            <div class="plate-price">¥28</div>
            <div class="plate-count" id="count-black">0</div>
            <div class="counter-controls">
                <div class="ctrl-btn-minus" onclick="adjustPlate('black', -1)">−</div>
            </div>
        </div>
    </div>

    <!-- AI 助手 -->
    <div class="ai-section">
        <div class="ai-buttons">
            <button class="btn-ai btn-ai-analyze" id="ai-analyze-btn" onclick="callGemini('analyze')">
                🥗 AI 分析
            </button>
            <button class="btn-ai btn-ai-suggest" id="ai-suggest-btn" onclick="callGemini('suggest')">
                🍣 AI 推荐
            </button>
        </div>
        <div class="ai-response" id="ai-response">
            让 AI 评价一下这顿寿司...
        </div>
    </div>

    <!-- 单点项目 - 调整为对齐按钮宽度的比例 -->
    <div class="others-section">
        <div class="input-group">
            <div class="input-row">
                <input type="text" id="extra-name" placeholder="菜名">
                <input type="number" id="extra-price" placeholder="¥" inputmode="decimal">
            </div>
            <button class="btn-add" onclick="addExtra()">添加单项</button>
        </div>
        <div class="others-list" id="others-list"></div>
    </div>

    <!-- 汇总 -->
    <div class="summary">
        <div class="total-label">总计</div>
        <div class="total-amount">¥<span id="total-price">0.00</span></div>
        <div style="font-size: 13px; color: #666;">共 <span id="total-plates">0</span> 盘</div>
    </div>

    <div class="actions">
        <button class="btn-reset" onclick="resetAll()">重置</button>
        <button class="btn-export" onclick="showReceiptModal()">账单详情</button>
    </div>

    <div class="footer-note">@jory 个人计数工具 · AI技术由Gemini提供支持</div>
</div>

<div id="modal" class="modal">
    <div class="modal-content">
        <div id="receipt-content"></div>
        <button class="btn-reset" style="margin-top: 24px; width: 100%; background: #000;" onclick="closeModal()">返回修改</button>
    </div>
</div>

<div id="toast">已重置</div>

<script>
    const PRICES = { white: 8, red: 10, silver: 15, yellow: 20, black: 28 };
    const NAMES = { white: "白盘", red: "红盘", silver: "银盘", yellow: "黄盘", black: "黑盘" };
    
    let counts = { white: 0, red: 0, silver: 0, yellow: 0, black: 0 };
    let extras = [];

    const apiKey = ""; 

    async function callGemini(mode) {
        const analyzeBtn = document.getElementById('ai-analyze-btn');
        const suggestBtn = document.getElementById('ai-suggest-btn');
        const responseDiv = document.getElementById('ai-response');

        let currentStatus = "";
        for(let color in counts) if(counts[color] > 0) currentStatus += `${NAMES[color]}${counts[color]}盘, `;
        if(extras.length > 0) currentStatus += `单点：${extras.map(e => e.name).join('、')}。`;
        const total = document.getElementById('total-price').innerText;
        currentStatus += `总计¥${total}。`;

        let prompt = mode === 'analyze' 
            ? `分析点餐偏好：${currentStatus}` 
            : `基于目前的餐点推荐一个寿司郎必点特色：${currentStatus}`;

        analyzeBtn.disabled = true;
        suggestBtn.disabled = true;
        responseDiv.innerHTML = '<span class="loading-spinner"></span> 思考中...';

        try {
            const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;
            const res = await fetch(url, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify({
                    contents: [{ parts: [{ text: prompt }] }],
                    systemInstruction: { parts: [{ text: "你是一个幽默的寿司郎美食家。用中文回复，限制在40字以内。要毒舌或者非常赞赏。" }] }
                })
            });
            const data = await res.json();
            responseDiv.innerText = data.candidates?.[0]?.content?.parts?.[0]?.text || "AI 累了...";
        } catch (e) {
            responseDiv.innerText = "网络似乎有点问题。";
        } finally {
            analyzeBtn.disabled = false;
            suggestBtn.disabled = false;
        }
    }

    function adjustPlate(color, delta) {
        const newVal = counts[color] + delta;
        if (newVal >= 0) {
            counts[color] = newVal;
            updateUI();
            if(delta > 0 && window.navigator.vibrate) window.navigator.vibrate(10);
        }
    }

    function addExtra() {
        const priceInput = document.getElementById('extra-price');
        const nameInput = document.getElementById('extra-name');
        const price = parseFloat(priceInput.value);
        let name = nameInput.value.trim() || `单点${extras.length + 1}`;
        
        if (isNaN(price) || price <= 0) {
            showToast('写个价格呗');
            return;
        }
        
        extras.push({ name, price });
        priceInput.value = ''; 
        nameInput.value = '';
        updateUI();
    }

    function removeExtra(index) {
        extras.splice(index, 1);
        updateUI();
    }

    function resetAll() {
        counts = { white: 0, red: 0, silver: 0, yellow: 0, black: 0 };
        extras = [];
        updateUI();
        document.getElementById('ai-response').innerText = '让 AI 评价一下这顿寿司...';
        showToast('已重置');
    }

    function showToast(msg) {
        const t = document.getElementById("toast");
        t.innerText = msg;
        t.classList.add("show");
        setTimeout(() => { t.classList.remove("show"); }, 2000);
    }

    function updateUI() {
        let totalPrice = 0, totalPlates = 0;
        for (const color in counts) {
            document.getElementById(`count-${color}`).innerText = counts[color];
            totalPrice += counts[color] * PRICES[color];
            totalPlates += counts[color];
        }
        const listContainer = document.getElementById('others-list');
        listContainer.innerHTML = '';
        extras.forEach((item, index) => {
            totalPrice += item.price;
            const div = document.createElement('div');
            div.className = 'other-item';
            div.innerHTML = `<span>${item.name}</span><span>¥${item.price.toFixed(1)} <span class="remove-btn" onclick="removeExtra(${index})">✕</span></span>`;
            listContainer.appendChild(div);
        });
        document.getElementById('total-price').innerText = totalPrice.toFixed(2);
        document.getElementById('total-plates').innerText = totalPlates;
    }

    function showReceiptModal() {
        let totalPrice = 0, rows = '';
        for (const color in counts) {
            if (counts[color] > 0) {
                const sub = counts[color] * PRICES[color];
                totalPrice += sub;
                rows += `<tr><td>${NAMES[color]}</td><td>¥${PRICES[color]}</td><td>${counts[color]}</td><td style="text-align:right">¥${sub}</td></tr>`;
            }
        }
        extras.forEach(item => {
            totalPrice += item.price;
            rows += `<tr><td>${item.name}</td><td>-</td><td>1</td><td style="text-align:right">¥${item.price}</td></tr>`;
        });
        if (totalPrice === 0) return showToast('还没吃呢');
        
        document.getElementById('receipt-content').innerHTML = `
            <div style="text-align:center;">
                <h2 style="margin:0; font-size:24px;">盘盘算 · スシロ</h2>
                <p style="color:#8e8e93; font-size:12px; margin:5px 0 20px;">账单详情</p>
            </div>
            <table class="receipt-table">
                <thead><tr><th>项目</th><th>单价</th><th>数量</th><th style="text-align:right">合计</th></tr></thead>
                <tbody>${rows}</tbody>
            </table>
            <div style="margin-top:30px; text-align:right; border-top:2px solid #000; padding-top:15px;">
                <span style="font-size:16px; font-weight:600;">总计</span>
                <div style="font-size:32px; font-weight:900; color:#d0021b;">¥${totalPrice.toFixed(2)}</div>
            </div>`;
        document.getElementById('modal').style.display = 'flex';
    }

    function closeModal() { document.getElementById('modal').style.display = 'none'; }
    updateUI();
</script>
</body>
</html>
