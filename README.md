<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>所得稅變更試算工具</title>
    <style>
        :root { --primary: #2c3e50; --accent: #e74c3c; --success: #27ae60; }
        body { font-family: "PingFang TC", "Microsoft JhengHei", sans-serif; background: #f0f2f5; padding: 20px; color: #333; }
        .container { max-width: 600px; margin: 0 auto; background: white; padding: 30px; border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        h2 { text-align: center; color: var(--primary); border-bottom: 2px solid #eee; padding-bottom: 10px; }
        .input-section { background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0; }
        label { display: block; margin-bottom: 8px; font-weight: bold; }
        input[type="number"] { width: 100%; padding: 12px; border: 2px solid #ddd; border-radius: 8px; font-size: 16px; transition: 0.3s; box-sizing: border-box; }
        input:focus { border-color: var(--primary); outline: none; }
        
        .tax-config { margin-top: 20px; }
        table { width: 100%; border-collapse: collapse; font-size: 14px; }
        th, td { padding: 10px; border-bottom: 1px solid #eee; text-align: center; }
        .future-input { width: 60px !important; padding: 5px !important; text-align: center; }
        
        button { width: 100%; background: var(--primary); color: white; border: none; padding: 15px; border-radius: 8px; font-size: 18px; font-weight: bold; cursor: pointer; margin-top: 20px; transition: 0.3s; }
        button:hover { background: #1a252f; transform: translateY(-2px); }

        .result-card { display: none; margin-top: 25px; padding: 20px; border-radius: 10px; border-left: 8px solid var(--primary); background: #fdfdfd; }
        .diff-box { font-size: 24px; font-weight: bold; margin-top: 10px; }
        .plus { color: var(--accent); }
        .minus { color: var(--success); }
    </style>
</head>
<body>

<div class="container">
    <h2>所得稅新舊制試算器</h2>
    
    <div class="input-section">
        <label>請輸入您的預估年收入 (NTD)：</label>
        <input type="number" id="income" value="1000000" oninput="calculate()">
        <p style="font-size: 12px; color: #666; margin-top: 10px;">* 已自動扣除單身基礎額度 46.4 萬 (免稅+標扣+薪扣)</p>
    </div>

    <div class="tax-config">
        <label>自定義未來稅率 (%)</label>
        <table>
            <thead>
                <tr>
                    <th>淨額級距</th>
                    <th>目前</th>
                    <th>未來</th>
                </tr>
            </thead>
            <tbody id="taxBody">
                </tbody>
        </table>
    </div>

    <div id="result" class="result-card">
        <div>目前應繳稅額：<span id="curTax">0</span> 元</div>
        <div>未來預估稅額：<span id="futTax">0</span> 元</div>
        <div class="diff-box" id="diffBox">差異：0 元</div>
    </div>
</div>

<script>
// 114年度 (2026) 官方級距資料
const config = [
    { limit: 610000, rate: 0.05 },
    { limit: 1380000, rate: 0.12 },
    { limit: 2770000, rate: 0.20 },
    { limit: 5190000, rate: 0.30 },
    { limit: Infinity, rate: 0.40 }
];

// 初始化表格
const tbody = document.getElementById('taxBody');
config.forEach((item, index) => {
    const range = index === 0 ? `0 - ${item.limit/10000}萬` : 
                  item.limit === Infinity ? `${config[index-1].limit/10000}萬以上` :
                  `${config[index-1].limit/10000} - ${item.limit/10000}萬`;
    
    tbody.innerHTML += `
        <tr>
            <td>${range}</td>
            <td>${item.rate * 100}%</td>
            <td><input type="number" class="f-rate" value="${item.rate * 100}" step="1" oninput="calculate()">%</td>
        </tr>
    `;
});

function calculateTax(netIncome, rates) {
    let tax = 0;
    let prevLimit = 0;
    
    for (let i = 0; i < config.length; i++) {
        const currentLimit = config[i].limit;
        const currentRate = rates[i];
        
        if (netIncome > prevLimit) {
            const taxableInThisBracket = Math.min(netIncome, currentLimit) - prevLimit;
            tax += taxableInThisBracket * currentRate;
        }
        prevLimit = currentLimit;
    }
    return Math.round(tax);
}

function calculate() {
    const income = parseFloat(document.getElementById('income').value) || 0;
    const netIncome = Math.max(0, income - 464000); // 扣除基本額度
    
    const currentRates = config.map(c => c.rate);
    const futureRates = Array.from(document.querySelectorAll('.f-rate')).map(input => parseFloat(input.value) / 100 || 0);
    
    const curTax = calculateTax(netIncome, currentRates);
    const futTax = calculateTax(netIncome, futureRates);
    const diff = futTax - curTax;

    document.getElementById('result').style.display = 'block';
    document.getElementById('curTax').innerText = curTax.toLocaleString();
    document.getElementById('futTax').innerText = futTax.toLocaleString();
    
    const diffBox = document.getElementById('diffBox');
    if (diff > 0) {
        diffBox.innerHTML = `需多繳：<span class="plus">+${diff.toLocaleString()}</span> 元`;
    } else if (diff < 0) {
        diffBox.innerHTML = `可省下：<span class="minus">${diff.toLocaleString()}</span> 元`;
    } else {
        diffBox.innerHTML = `稅額無變動`;
        diffBox.className = "diff-box";
    }
}

// 初始執行一次
calculate();
</script>

</body>
</html>
