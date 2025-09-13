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
    <h1>💰 قیمت لحظه‌ای ارزهای دیجیتال (بر حسب USDT)</h1>
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
    async function loadPrices() {
      const coins = "bitcoin,ethereum,binancecoin,ripple,cardano,dogecoin,solana,tron,polkadot";
      const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins}&vs_currencies=usdt&include_24hr_change=true`;

      try {
        const res = await fetch(url);
        const data = await res.json();

        const mapping = {
          bitcoin: "BTC",
          ethereum: "ETH",
          binancecoin: "BNB",
          ripple: "XRP",
          cardano: "ADA",
          dogecoin: "DOGE",
          solana: "SOL",
          tron: "TRX",
          polkadot: "DOT"
        };

        let html = "";
        for (const id in mapping) {
          if (!data[id]) continue;
          const symbol = mapping[id];
          const price = data[id].usdt.toLocaleString();
          const change = data[id].usdt_24h_change.toFixed(2);
          const cls = change >= 0 ? "up" : "down";
          html += `
            <tr>
              <td><b>${symbol}</b></td>
              <td>${price} USDT</td>
              <td class="${cls}">${change}%</td>
            </tr>
          `;
        }
        document.getElementById("prices").innerHTML = html;
      } catch (error) {
        document.getElementById("prices").innerHTML = `<tr><td colspan="3">⚠ خطا در بارگذاری داده‌ها</td></tr>`;
        console.error("Error loading data:", error);
      }
    }

    loadPrices();
    setInterval(loadPrices, 10000); // هر ۱۰ ثانیه آپدیت
  </script>
</body>
</html>
