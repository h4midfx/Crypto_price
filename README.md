
<html lang="fa"> <!-- fa für Dari (Persisch) -->
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>قیمت بیت‌کوین و ارزهای دیجیتال | Crypto Prices</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      margin: 20px;
    }
    #flag {
      width: 200px;
      height: auto;
      margin-bottom: 20px;
    }
    .prices {
      font-size: 1.5em;
      margin-top: 30px;
    }
    .crypto {
      margin: 10px;
    }
  </style>
</head>
<body>
  <!-- Afghanische Flagge -->
  <img id="flag" src="https://wallpapercave.com/wp/wp4056551.jpg" alt="Afghan Flag" />
 
  <!-- Titel in Dari und Englisch -->
  <h1>قیمت بیت‌کوین و ارزهای دیجیتال</h1>
  <h2>Bitcoin & Crypto Prices</h2>

  <!-- Preise werden hier angezeigt -->
  <div class="prices" id="prices">
    <div class="crypto" id="btc">BTC: …</div>
    <div class="crypto" id="eth">ETH: …</div>
    <div class="crypto" id="xrp">XRP: …</div>
  </div>

  <script>
    async function fetchPrices() {
      try {
        const response = await fetch('https://api.coingecko.com/api/v3/simple/price?ids=bitcoin,ethereum,ripple&vs_currencies=usd');
        const data = await response.json();
        document.getElementById('btc').textContent = 'BTC: $' + data.bitcoin.usd;
        document.getElementById('eth').textContent = 'ETH: $' + data.ethereum.usd;
        document.getElementById('xrp').textContent = 'XRP: $' + data.ripple.usd;
      } catch (error) {
        console.error('Fehler beim Laden der Preise:', error);
      }
    }

    // Preise sofort laden und danach jede Minute aktualisieren
    fetchPrices();
    setInterval(fetchPrices, 60000);
  </script>
</body>
</html>

