
<html lang="fa">
<head>
<meta charset="UTF-8">
<title>نرخ ارز و اخبار اقتصادی</title>
<style>
  body {
    font-family: sans-serif;
    background-image: url('https://images.unsplash.com/photo-1627485044552-9942a2f0f153?auto=format&fit=crop&w=1950&q=80');
    background-size: cover;
    background-position: center;
    color: #fff;
    text-align: center;
    padding: 30px;
  }
  header img { width: 120px; border-radius: 8px; box-shadow: 0 2px 6px rgba(0,0,0,0.5); }
  h1 { margin-top: 15px; color: #fff; }

  /* کارت های ارز دیجیتال */
  .crypto-grid { display: flex; flex-wrap: wrap; justify-content: center; gap: 15px; margin-bottom: 20px; }
  .crypto-card {
    background: rgba(0,0,0,0.7);
    padding: 10px 15px;
    border-radius: 10px;
    text-align: center;
    width: 120px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.5);
    color: #fff;
  }
  .crypto-card img { width: 36px; height: 36px; margin-bottom: 5px; }
  .crypto-card .name { font-weight: bold; margin-bottom: 3px; }
  .crypto-card .price { font-size: 14px; }
  .crypto-card .change { font-size: 12px; }
  .up { color: #00ff00; }
  .down { color: #ff3333; }

  /* اخبار اقتصادی */
  .news-item {
    background: rgba(255,255,255,0.85);
    color: #000;
    padding: 15px;
    margin: 10px auto;
    border-radius: 8px;
    max-width: 800px;
    text-align:right;
    box-shadow:0 2px 6px rgba(0,0,0,0.2);
  }
  .news-title { font-weight: bold; font-size: 18px; }
  .news-time { color: #555; font-size: 14px; margin-top: 5px; }

  .announcement, .feedback {
    margin: 15px auto;
    font-size: 16px;
    background: rgba(0,0,0,0.6);
    display: inline-block;
    padding: 12px 20px;
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.5);
  }
  .feedback a { text-decoration: none; color: #00ccff; font-weight: bold; margin: 0 5px; }
</style>
</head>
<body>

<header>
  <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="پرچم افغانستان">
  <h1>نرخ لحظه‌ای ارزهای دیجیتال و اخبار اقتصادی</h1>
</header>

<!-- کارت های ارز دیجیتال -->
<div class="crypto-grid" id="crypto-grid">
  <div>در حال بارگذاری...</div>
</div>

<!-- پیام های اطلاع رسانی -->
<div class="announcement" style="color:#ffcc00;">
  ⚡ گروه تلگرامی و کانال بزودی فعال می‌شوند!
</div>
<div class="announcement" style="color:#00ccff;">
  📚 آموزش‌های رایگان از طرف اساتید به زودی به سایت اضافه می‌شوند!
</div>

<!-- اخبار اقتصادی -->
<h2 style="margin-top:40px;">اخبار اقتصادی</h2>
<div id="news">
  <div>در حال بارگذاری...</div>
</div>

<!-- لینک ها -->
<div class="feedback">
  💡 پیشنهادی داری؟  
  <a href="mailto:Bhack050@gmail.com">ایمیل بده</a> | 
  <a href="https://t.me/h4mid_fx" target="_blank">تلگرام</a>
</div>

<script>
// ارزهای دیجیتال
const cryptoSymbols = [
  {name:"بیتکوین", binance:"BTCUSDT", logo:"https://cryptologos.cc/logos/bitcoin-btc-logo.png"},
  {name:"اتریوم", binance:"ETHUSDT", logo:"https://cryptologos.cc/logos/ethereum-eth-logo.png"},
  {name:"ترون", binance:"TRXUSDT", logo:"https://cryptologos.cc/logos/tron-trx-logo.png"},
  {name:"دوج", binance:"DOGEUSDT", logo:"https://cryptologos.cc/logos/dogecoin-doge-logo.png"},
  {name:"XRP", binance:"XRPUSDT", logo:"https://cryptologos.cc/logos/xrp-xrp-logo.png"}
];
const fiatSymbols = [
  {name:"دلار آمریکا", code:"USD"},
  {name:"یورو", code:"EUR"},
  {name:"پوند", code:"GBP"}
];

// بارگذاری نرخ ارزها
async function loadCrypto(){
  const grid = document.getElementById('crypto-grid');
  grid.innerHTML = '';
  for(let sym of cryptoSymbols){
    try{
      const resPrice = await fetch(`https://api.binance.com/api/v3/ticker/price?symbol=${sym.binance}`);
      const priceData = await resPrice.json();
      const price = parseFloat(priceData.price).toFixed(4);

      const res24h = await fetch(`https://api.binance.com/api/v3/ticker/24hr?symbol=${sym.binance}`);
      const changeData = await res24h.json();
      const change = parseFloat(changeData.priceChangePercent).toFixed(2);
      const cls = change >= 0 ? "up":"down";

      const card = document.createElement('div');
      card.className = 'crypto-card';
      card.innerHTML = `
        <img src="${sym.logo}" alt="${sym.name}">
        <div class="name">${sym.name}</div>
        <div class="price">${price} USDT</div>
        <div class="change ${cls}">${change}%</div>
      `;
      grid.appendChild(card);
    }catch(err){
      const errDiv = document.createElement('div');
      errDiv.textContent = `خطا در دریافت ${sym.name}`;
      grid.appendChild(errDiv);
      console.error(err);
    }
  }

  // نرخ ارزهای فیات
  try{
    const resFiat = await fetch('https://api.exchangerate.host/latest?base=USD&symbols=AFN,EUR,GBP');
    const dataFiat = await resFiat.json();
    const afnRateUSD = dataFiat.rates.AFN;
    const afnRateEUR = dataFiat.rates.AFN / dataFiat.rates.EUR;
    const afnRateGBP = dataFiat.rates.AFN / dataFiat.rates.GBP;

    for(let fiat of fiatSymbols){
      let priceAFN = 0;
      if(fiat.code==="USD") priceAFN = afnRateUSD.toFixed(2);
      if(fiat.code==="EUR") priceAFN = afnRateEUR.toFixed(2);
      if(fiat.code==="GBP") priceAFN = afnRateGBP.toFixed(2);

      const card = document.createElement('div');
      card.className = 'crypto-card';
      card.innerHTML = `
        <div class="name">${fiat.name}</div>
        <div class="price">${priceAFN} AFN</div>
        <div class="change">-</div>
      `;
      grid.appendChild(card);
    }
  }catch(err){console.error(err);}
}
loadCrypto();
setInterval(loadCrypto,10000);

// بارگذاری اخبار اقتصادی فارسی
async function loadNews(){
  const rssUrl = 'https://www.farsnews.ir/rss/economy';
  const apiUrl = `https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(rssUrl)}`;
  
  try{
    const res = await fetch(apiUrl);
    const data = await res.json();
    const newsDiv = document.getElementById('news');
    newsDiv.innerHTML = '';
    data.items.forEach(item=>{
      const div = document.createElement('div');
      div.className = 'news-item';
      div.innerHTML = `
        <div class="news-title"><a href="${item.link}" target="_blank">${item.title}</a></div>
        <div class="news-time">${new Date(item.pubDate).toLocaleString('fa-IR')}</div>
      `;
      newsDiv.appendChild(div);
    });
  }catch(err){
    document.getElementById('news').innerHTML = 'خطا در بارگذاری اخبار';
    console.error(err);
  }
}
loadNews();
setInterval(loadNews, 5*60*1000);
</script>
</body>
</html>
