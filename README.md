<html lang="fa">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>قیمت بیت‌کوین و ارزهای دیجیتال | Crypto Prices</title>
  <style>
    /* Allgemeines Layout */
    body {
      font-family: "Segoe UI", Arial, sans-serif;
      text-align: center;
      margin: 0;
      padding: 0;
      background: #f2f4f7;
      color: #222;
      transition: background 0.3s, color 0.3s;
    }
    body.dark {
      background: #121212;
      color: #f5f5f5;
    }

    header {
      padding: 20px;
    }

    #flag {
      width: 120px;
      height: auto;
      margin-bottom: 10px;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.2);
    }

    h1 {
      font-size: 2em;
      margin: 10px 0 5px;
    }

    h2 {
      font-size: 1.2em;
      font-weight: normal;
      color: #555;
    }
    body.dark h2 {
      color: #ccc;
    }

    /* Dark/Light Toggle */
    .toggle-btn {
      margin: 15px;
      padding: 10px 20px;
      cursor: pointer;
      background: #007bff;
      border: none;
      color: white;
      font-size: 1em;
      border-radius: 30px;
      box-shadow: 0 3px 6px rgba(0,0,0,0.2);
      transition: background 0.3s;
    }
    .toggle-btn:hover {
      background: #0056cc;
    }

    /* Krypto-Karten */
    .prices {
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      gap: 20px;
      margin: 30px auto;
      max-width: 1000px;
    }

    .crypto-card {
      background: white;
      border-radius: 12px;
      padding: 20px;
      width: 200px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
      transition: transform 0.2s, background 0.3s, color 0.3s;
    }
    .crypto-card:hover {
      transform: translateY(-5px);
    }
    body.dark .crypto-card {
      background: #1f1f1f;
      color: #eee;
    }

    .crypto-title {
      font-weight: bold;
      font-size: 1.2em;
      margin-bottom: 10px;
    }

    .price {
      font-size: 1.1em;
      margin: 5px 0;
    }

    .updated {
      margin: 20px;
      font-size: 0.9em;
      color: gray;
    }
    body.dark .updated {
      color: #aaa;
    }
  </style>
</head>
<body>
  <header>
    <!-- Afghanische Flagge -->
    <img id="flag" src="https://wallpapercave.com/wp/wp4056551.jpg" alt="Afghan Flag" />
    <!-- Titel -->
    <h1>قیمت بیت‌کوین و ارزهای دیجیتال</h1>
    <h2>Bitcoin & Crypto Prices</h2>
    <!-- Dark/Light Button -->
    <button class="toggle-btn" onclick="toggleTheme()">🌙 / ☀️</button>
  </header>

  <!-- Preise -->
  <div class="prices">
    <div class="crypto-card" id="btc">
      <div class="crypto-title">BTC</div>
      <div class="price">Lade...</div>
    </div>
    <div class="crypto-card" id="eth">
      <div class="crypto-title">ETH</div>
      <div class="price">Lade...</div>
    </div>
    <div class="crypto-card" id="xrp">
      <div class="crypto-title">XRP</div>
      <div class="price">Lade...</div>
    </div>
    <div class="crypto-card" id="ltc">
      <div class="crypto-title">LTC</div>
      <div class="price">Lade...</div>
    </div>
    <div class="crypto-card" id="doge">
      <div class="crypto-title">DOGE</div>
      <div class="price">Lade...</div>
    </div>
  </div>

  <!-- Letzte Aktualisierung -->
  <div class="updated" id="lastUpdate">Aktualisiert: …</div>

  <script>
    async function fetchPrices() {
      try {
        const response = await fetch(
          'https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,ripple,litecoin,dogecoin&vs_currencies=usd,afn'
        );
        const data = await response.json();

        updateCard("btc", "BTC", data.bitcoin);
        updateCard("eth", "ETH", data.ethereum);
        updateCard("xrp", "XRP", data.ripple);
        updateCard("ltc", "LTC", data.litecoin);
        updateCard("doge", "DOGE", data.dogecoin);

        const now = new Date();
        document.getElementById("lastUpdate").textContent =
          "Aktualisiert: " + now.toLocaleString();
      } catch (error) {
        console.error("Fehler beim Laden der Preise:", error);
      }
    }

    function updateCard(id, symbol, coin) {
      const card = document.getElementById(id);
      card.querySelector(".price").textContent =
        `$${coin.usd} | AFN ${coin.afn}`;
    }

    function toggleTheme() {
      document.body.classList.toggle("dark");
    }

    fetchPrices();
    setInterval(fetchPrices, 60000);
  </script>
</body>
</html>
