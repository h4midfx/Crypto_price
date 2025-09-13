<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>نرخ لحظه‌ای ارزهای دیجیتال</title>
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
    h1 {
      margin-top: 15px;
      color: #222;
    }
    table {
      margin: 20px auto;
      border-collapse: collapse;
      background: white;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      border-radius: 8px;
      overflow: hidden;
    }
    th, td {
      padding: 12px 18px;
      border-bottom: 1px solid #eee;
      text-align: center;
    }
    th {
      background: #fafafa;
      font-weight: bold;
    }
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
    .feedback a {
      text-decoration: none;
      color: #0066cc;
      font-weight: bold;
      margin: 0 5px;
    }
  </style>
</head>
<body>
  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="پرچم افغانستان">
    <h1>نرخ لحظه‌ای ارزهای دیجیتال</h1>
  </header>

  <table>
    <thead>
      <tr>
        <th>نام</th>
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
    const symbols = [
      {name: "بیتکوین", binance: "BTCUSDT"},
      {name: "اتریوم", binance: "ETHUSDT"},
      {name: "ترون", binance: "TRXUSDT"},
      {name: "دوج", binance: "DOGEUSDT"},
      {name: "XRP", binance: "XRPUSDT"},
      {name: "طلا", binance: "XAUUSDT"},
      {name: "نقره", binance: "XAGUSDT"},
      {name: "دلار آمریکا", binance: "BUSDUSDT"}, // نمونه برای دلار
      {name: "پوند", binance: "GBPUSDT"},
      {name: "یورو", binance: "EURUSDT"}
    ];

    async function loadPrices() {
      let html = "";
      for (let sym of symbols) {
        try {
          // قیمت فعلی
          const resPrice = await fetch(`https://api.binance.com/api/v3/ticker/price?symbol=${sym.binance}`);
          const priceData = await resPrice.json();
          const price = parseFloat(priceData.price).toFixed(4);

          // تغییر 24 ساعته
          const res24h = await fetch(`https://api.binance.com/api/v3/ticker/24hr?symbol=${sym.binance}`);
          const changeData = await res24h.json();
          const change = parseFloat(changeData.priceChangePercent).toFixed(2);
          const cls = change >= 0 ? "up" : "down";

          html += `
            <tr>
              <td><b>${sym.name}</b></td>
              <td>${price} USDT</td>
              <td class="${cls}">${change}%</td>
            </tr>
          `;
        } catch (err) {
          html += `<tr><td colspan="3">خطا در دریافت ${sym.name}</td></tr>`;
          console.error(err);
        }
      }
      document.getElementById("prices").innerHTML = html;
    }

    loadPrices();
    setInterval(loadPrices, 10000); // هر 10 ثانیه آپدیت
  </script>
</body>
</html>
