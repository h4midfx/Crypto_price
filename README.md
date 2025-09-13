<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>قیمت لحظه‌ای ارزهای دیجیتال</title>
  <style>
    body {
      font-family: sans-serif;
      background: #f0f2f5;
      text-align: center;
      padding: 30px;
    }
    header img {
      width: 120px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.2);
    }
    h1 { margin-top: 15px; color: #222; }
    table {
      margin: 20px auto;
      border-collapse: collapse;
      background: white;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      border-radius: 8px;
      overflow: hidden;
    }
    th, td { padding: 12px 18px; border-bottom: 1px solid #eee; text-align: center; }
    th { background: #fafafa; font-weight: bold; }
    .up { color: green; }
    .down { color: red; }
    .feedback {
      margin-top: 25px;
      font-size: 16px;
      background: #fff;
      display: inline-block;
      padding: 12px 20px;
      border-radius: 8px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }
    .feedback a { text-decoration: none; color: #0066cc; font-weight: bold; margin: 0 5px; }
  </style>
</head>
<body>
  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="پرچم افغانستان">
    <h1>💰 قیمت لحظه‌ای ارزهای دیجیتال (USDT)</h1>
  </header>

  <table>
    <thead>
      <tr>
        <th>نماد</th>
        <th>قیمت (USDT)</th>
        <th>تغییر ۲۴ ساعته</th>
      </tr>
    </thead>
    <tbody id="prices">
      <tr><td colspan="3">در حال بارگذاری...</td></tr>
    </tbody>
  </table>

  <div class="feedback">
    💡 پیشنهادی داری؟  
    <a href="mailto:Bhack050@gmail.com">ایمیل بده</a> | 
    <a href="https://t.me/username" target="_blank">تلگرام</a>
  </div>

  <script>
    // داده نمونه برای نمایش بدون fetch
    const sampleData = [
      {symbol:"BTC", price: 32000, change: 1.2},
      {symbol:"ETH", price: 2100, change: -0.5},
      {symbol:"BNB", price: 310, change: 0.8},
      {symbol:"XRP", price: 0.55, change: -1.3},
      {symbol:"ADA", price: 1.25, change: 0.4},
      {symbol:"DOGE", price: 0.065, change: -0.2},
      {symbol:"SOL", price: 38, change: 2.5},
      {symbol:"TRX", price: 0.06, change: -0.1},
      {symbol:"DOT", price: 15, change: 0.7}
    ];

    function loadPrices() {
      let html = "";
      sampleData.forEach(coin => {
        const cls = coin.change >= 0 ? "up" : "down";
        html += `
          <tr>
            <td><b>${coin.symbol}</b></td>
            <td>${coin.price.toLocaleString()} USDT</td>
            <td class="${cls}">${coin.change}%</td>
          </tr>
        `;
      });
      document.getElementById("prices").innerHTML = html;
    }

    loadPrices();
    // شبیه‌سازی آپدیت قیمت هر 10 ثانیه
    setInterval(() => {
      sampleData.forEach(c => {
        const delta = (Math.random()*2-1).toFixed(2); // تغییر کوچک تصادفی
        c.price = (c.price * (1 + delta/100)).toFixed(2);
        c.change = delta;
      });
      loadPrices();
    }, 10000);
  </script>
</body>
</html>
