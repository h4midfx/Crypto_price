<html lang="fa">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>قیمت بیت‌کوین و ارزهای دیجیتال</title>
  <style>
    body {
      font-family: "Segoe UI", sans-serif;
      margin: 0;
      padding: 0;
      background: #f9fafc;
      color: #222;
      text-align: center;
    }
    header {
      padding: 20px;
      background: linear-gradient(135deg, #007bff, #00c6ff);
      color: white;
    }
    h1 {
      margin: 10px 0 5px;
    }
    h2 {
      font-weight: normal;
      margin: 0;
      font-size: 1.1em;
    }
    .prices {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      padding: 30px;
    }
    .crypto {
      background: white;
      border-radius: 12px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      padding: 20px;
      width: 200px;
      transition: transform 0.2s;
    }
    .crypto:hover {
      transform: translateY(-6px);
    }
    .symbol {
      font-size: 1.2em;
      font-weight: bold;
      margin-bottom: 10px;
    }
    .price {
      font-size: 0.95em;
      line-height: 1.6em;
    }
    .updated {
      margin: 20px;
      font-size: 0.9em;
      color: gray;
    }
  </style>
</head>
<body>
  <header>
    <h1>قیمت بیت‌کوین و ارزهای دیجیتال</h1>
    <h2>Bitcoin & Crypto Prices</h2>
  </header>

  <div class="prices" id="prices"></div>
  <div class="updated" id="lastUpdate">Aktualisiert: …</div>

  <script>
    const coins = [
      { id: "bitcoin", symbol: "BTC" },
      { id: "ethereum", symbol: "ETH" },
      { id: "ripple", symbol: "XRP" },
      { id: "litecoin", symbol: "LTC" },
      { id: "dogecoin", symbol: "DOGE" }
    ];

    async function fetchPrices() {
      try {
        const url = `https://api.coingecko.com/api/v3/simple/price?ids=${coins.map(c=>c.id).join(",")}&vs_currencies=usd,afn,irr`;
        const res = await fetch(url);
        const data = await res.json();

        document.getElementById("prices").innerHTML = coins.map(c => {
          const usd = data[c.id].usd.toLocaleString("en-US");
          const afn = data[c.id].afn.toLocaleString("en-US");
          const toman = (data[c.id].irr / 10).toLocaleString("fa-IR");
          return `
            <div class="crypto">
              <div class="symbol">${c.symbol}</div>
              <div class="price">
                💵 USD: $${usd}<br>
                🇦🇫 AFN: ${afn}<br>
                🇮🇷 تومان: ${toman}
              </div>
            </div>
          `;
        }).join("");

        const now = new Date();
        document.getElementById("lastUpdate").textContent =
          "Aktualisiert: " + now.toLocaleString();
      } catch (e) {
        console.error("Fehler beim Laden:", e);
      }
    }

    fetchPrices();
    setInterval(fetchPrices, 60000);
  </script>
</body>
</html>
