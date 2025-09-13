<div id="crypto-widget" style="font-family:sans-serif; color:#fff; text-align:center; padding:20px; background:linear-gradient(rgba(0,0,0,0.7), rgba(0,0,0,0.7)), url('https://images.unsplash.com/photo-1627485044552-9942a2f0f153?auto=format&fit=crop&w=1950&q=80'); background-size:cover; border-radius:10px;">

  <header>
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan_%282004%E2%80%932021%29.svg" alt="پرچم افغانستان" style="width:100px; border-radius:8px; box-shadow:0 2px 6px rgba(0,0,0,0.5);">
    <h2 style="margin-top:10px;">نرخ لحظه‌ای ارزهای دیجیتال و ارزهای فیات</h2>
  </header>

  <div class="crypto-grid" style="display:flex; flex-wrap:wrap; justify-content:center; gap:15px; margin:20px 0;" id="crypto-grid">
    <div>در حال بارگذاری...</div>
  </div>

  <div style="margin:10px auto; font-size:16px; background:rgba(0,0,0,0.6); display:inline-block; padding:10px 15px; border-radius:8px;">
    ⚡ گروه تلگرامی و کانال بزودی فعال می‌شوند!<br>
    📚 آموزش‌های رایگان از طرف اساتید به زودی اضافه می‌شوند!
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
    {name:"XRP", binance:"XRPUSDT", logo:"https://cryptologos.cc/logos/xrp-xrp-logo.png"},
    {name:"Litecoin", binance:"LTCUSDT", logo:"https://cryptologos.cc/logos/litecoin-ltc-logo.png"},
    {name:"Cardano", binance:"ADAUSDT", logo:"https://cryptologos.cc/logos/cardano-ada-logo.png"},
    {name:"Solana", binance:"SOLUSDT", logo:"https://cryptologos.cc/logos/solana-sol-logo.png"},
    {name:"Polkadot", binance:"DOTUSDT", logo:"https://cryptologos.cc/logos/polkadot-dot-logo.png"},
    {name:"Shiba Inu", binance:"SHIBUSDT", logo:"https://cryptologos.cc/logos/shiba-inu-shib-logo.png"}
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
    }catch(e){ console.error(e);}
  }
  loadCrypto();
  setInterval(loadCrypto,10000);
})();
</script>
