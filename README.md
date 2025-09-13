<!DOCTYPE html>
<html lang="fa">
<head>
  <meta charset="UTF-8">
  <title>قیمت لحظه‌ای ارزها و دارایی‌ها</title>
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
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="">
    <h1>💰 قیمت لحظه‌ای ارزها و دارایی‌ها</h1>
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
    // داده نمونه قابل ویرایش
    const assets = [
      {name: "بیتکوین", price: 32000, change: 1.2},
      {name: "اتریوم", price: 2100, change: -0.5},
      {name: "ترون", price: 0.06, change: 0.3},
      {name: "دوج", price: 0.065, change: -0.2},
      {name: "XRP", price: 0.55, change: 1.1},
      {name: "طلا", price: 1800, change: 0.5},
      {name: "نقره", price: 25, change: -0.1},
      {name: "دلار آمریکا", price: 1, change: 0},
      {name: "پوند", price: 1.22, change: 0.2},
      {name: "یورو", price: 1.08, change: -0.1}
    ];

    function loadPrices() {
      let html = "";
      assets.forEach(a => {
        const cls = a.change >= 0 ? "up" : "down";
        html += `
          <tr>
            <td><b>${a.name}</b></td>
            <td>${a.price.toLocaleString()} USDT</td>
            <td class="${cls}">${a.change}%</td>
          </tr>
        `;
      });
      document.getElementById("prices").innerHTML = html;
    }

    loadPrices();

    // شبیه‌سازی آپدیت قیمت هر 10 ثانیه
    setInterval(() => {
      assets.forEach(a => {
        const delta = (Math.random()*2-1).toFixed(2);
        a.price = (a.price * (1 + delta/100)).toFixed(2);
        a.change = delta;
      });
      loadPrices();
    }, 10000);
  </script>
</body>
</html>
