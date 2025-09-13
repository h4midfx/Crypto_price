<div id="crypto-news-widget" style="font-family:sans-serif; color:#fff; text-align:center; padding:20px; background:linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url('https://images.unsplash.com/photo-1627485044552-9942a2f0f153?auto=format&fit=crop&w=1950&q=80'); background-size:cover; border-radius:10px;">

  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="پرچم افغانستان" style="width:100px; border-radius:8px; box-shadow:0 2px 6px rgba(0,0,0,0.5);">
    <h2 style="margin-top:10px;">نرخ لحظه‌ای ارزهای دیجیتال و اخبار اقتصادی</h2>
  </header>

  <div class="crypto-grid" style="display:flex; flex-wrap:wrap; justify-content:center; gap:15px; margin:20px 0;" id="crypto-grid">
    <div>در حال بارگذاری...</div>
  </div>

  <div style="margin:10px auto; font-size:16px; background:rgba(0,0,0,0.6); display:inline-block; padding:10px 15px; border-radius:8px;">
    ⚡ گروه تلگرامی و کانال بزودی فعال می‌شوند!<br>
    📚 آموزش‌های رایگان از طرف اساتید به زودی اضافه می‌شوند!
  </div>

  <h3 style="margin-top:30px;">اخبار اقتصادی</h3>
  <div id="news" style="margin-top:10px;">
    <div>در حال بارگذاری...</div>
  </div>

  <div style="margin-top:15px; font-size:16px; background:rgba(0,0,0,0.6); display:inline-block; padding:10px 15px; border-radius:8px;">
    💡 پیشنهادی داری؟  
    <a href="mailto:Bhack050@gmail.com" style="color:#00ccff; font-weight:bold;">ایمیل بده</a> | 
    <a href="https://t.me/h4mid_fx" target="_blank" style="color:#00ccff; font-weight:bold;">تلگرام</a>
  </div>

</div>

<script>
(async function(){
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
        const cls = change >= 0 ? "color:#00ff00":"color:#ff3333";

        const card = document.createElement('div');
        card.style.background="rgba(0,0,0,0.7)";
        card.style.padding="10px";
        card.style.borderRadius="8px";
        card.style.width="120px";
        card.style.color="#fff";
        card.style.textAlign="center";
        card.style.boxShadow="0 2px 6px rgba(0,0,0,0.5)";
        card.style.margin="5px";
        card.innerHTML = `<img src="${sym.logo}" style="width:36px;height:36px;margin-bottom:5px;"><div>${sym.name}</div><div>${price} USDT</div><div style="${cls}">${change}%</div>`;
        grid.appendChild(card);
      }catch(e){ console.error(e); }
    }

    // نرخ ارزهای فیات
    try{
      const resFiat = await fetch('https://api.exchangerate.host/latest?base=USD&symbols=AFN,EUR,GBP');
      const dataFiat = await resFiat.json();
      const afnRateUSD = dataFiat.rates.AFN;
      const afnRateEUR = dataFiat.rates.AFN / dataFiat.rates.EUR;
      const afnRateGBP = dataFiat.rates.AFN / dataFiat.rates.GBP;

      for(let fiat of fiatSymbols){
        let priceAFN = fiat.code==="USD"?afnRateUSD.toFixed(2): fiat.code==="EUR"?afnRateEUR.toFixed(2): afnRateGBP.toFixed(2);
        const card = document.createElement('div');
        card.style.background="rgba(0,0,0,0.7)";
        card.style.padding="10px";
        card.style.borderRadius="8px";
        card.style.width="120px";
        card.style.color="#fff";
        card.style.textAlign="center";
        card.style.boxShadow="0 2px 6px rgba(0,0,0,0.5)";
        card.style.margin="5px";
        card.innerHTML = `<div>${fiat.name}</div><div>${priceAFN} AFN</div>`;
        grid.appendChild(card);
      }
    }catch(e){ console.error(e); }
  }
  loadCrypto();
  setInterval(loadCrypto,10000);

  // اخبار اقتصادی فارسی (rss2json)
  async function loadNews(){
    const rssUrl='https://www.farsnews.ir/rss/economy';
    const apiUrl=`https://api.rss2json.com/v1/api.json?rss_url=${encodeURIComponent(rssUrl)}`;
    try{
      const res = await fetch(apiUrl);
      const data = await res.json();
      const newsDiv=document.getElementById('news');
      newsDiv.innerHTML='';
      data.items.forEach(item=>{
        const div=document.createElement('div');
        div.style.background="rgba(255,255,255,0.85)";
        div.style.color="#000";
        div.style.padding="10px";
        div.style.margin="5px auto";
        div.style.borderRadius="6px";
        div.style.textAlign="right";
        div.style.maxWidth="800px";
        div.style.boxShadow="0 2px 6px rgba(0,0,0,0.2)";
        div.innerHTML=`<a href="${item.link}" target="_blank" style="text-decoration:none; color:#000; font-weight:bold;">${item.title}</a><div style="font-size:12px; color:#555;">${new Date(item.pubDate).toLocaleString('fa-IR')}</div>`;
        newsDiv.appendChild(div);
      });
    }catch(e){ console.error(e); document.getElementById('news').innerHTML='خطا در بارگذاری اخبار'; }
  }
  loadNews();
  setInterval(loadNews,5*60*1000);
})();
</script>
