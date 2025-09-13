<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>قیمت کریپتو</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to bottom right, #dfe9f3, #ffffff);
            text-align: center;
            padding: 20px;
        }
        img.flag {
            width: 100px;
            margin-bottom: 10px;
        }
        h1 {
            color: #333;
            margin-bottom: 20px;
        }

        /* جدول کریپتو */
        table {
            margin: 0 auto 40px auto;
            border-collapse: collapse;
            width: 90%;
            max-width: 900px;
            background: #fff;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        th, td {
            padding: 15px;
            border-bottom: 1px solid #eee;
            text-align: center;
        }
        th {
            background-color: #007bff;
            color: white;
            font-size: 16px;
        }
        td {
            font-size: 15px;
        }
        tr:hover {
            background-color: #f1f1f1;
        }
        .positive { color: green; font-weight: bold; }
        .negative { color: red; font-weight: bold; }

        /* موبایل */
        @media (max-width: 600px) {
            th, td {
                padding: 10px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <!-- پرچم افغانستان -->
    <img src="https://upload.wikimedia.org/wikipedia/commons/5/5c/Flag_of_Afghanistan.svg" alt="پرچم افغانستان" class="flag">
    
    <h1>قیمت ارزهای دیجیتال</h1>
    
    <table>
        <thead>
            <tr>
                <th>ارز</th>
                <th>قیمت (USD)</th>
                <th>قیمت (AFN)</th>
                <th>تغییر ۲۴ ساعته</th>
            </tr>
        </thead>
        <tbody id="crypto-table"></tbody>
    </table>

    <script>
        const cryptoTable = document.getElementById('crypto-table');
        const coins = ['bitcoin','ethereum','dogecoin','litecoin','ripple','cardano','solana','polkadot','binancecoin'];
        const USD_TO_AFN = 90; // نرخ تقریبی تبدیل دلار به افغانی

        async function fetchCryptoData() {
            try {
                const response = await fetch(`https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(',')}&vs_currencies=usd&include_24hr_change=true`);
                const data = await response.json();

                cryptoTable.innerHTML = '';
                coins.forEach(coin => {
                    const priceUSD = data[coin].usd.toFixed(2);
                    const priceAFN = (data[coin].usd * USD_TO_AFN).toFixed(2);
                    const change = data[coin].usd_24h_change.toFixed(2);
                    const changeClass = change >= 0 ? 'positive' : 'negative';
                    cryptoTable.innerHTML += `
                        <tr>
                            <td>${coin.charAt(0).toUpperCase() + coin.slice(1)}</td>
                            <td>$${priceUSD}</td>
                            <td>${priceAFN} AFN</td>
                            <td class="${changeClass}">${change}%</td>
                        </tr>
                    `;
                });
            } catch (error) {
                cryptoTable.innerHTML = `<tr><td colspan="4">خطا در دریافت داده‌ها</td></tr>`;
                console.error(error);
            }
        }

        fetchCryptoData();
        setInterval(fetchCryptoData, 15000); // به‌روزرسانی هر ۱۵ ثانیه
    </script>
</body>
</html>
