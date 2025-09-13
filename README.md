<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>قیمت کریپتو و شطرنج</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f0f2f5;
            text-align: center;
            padding: 20px;
        }
        img.flag {
            width: 100px;
            margin-bottom: 10px;
        }
        h1 {
            color: #333;
        }
        table {
            margin: 20px auto;
            border-collapse: collapse;
            width: 80%;
            max-width: 600px;
        }
        th, td {
            padding: 12px;
            border: 1px solid #ccc;
        }
        th {
            background-color: #007bff;
            color: white;
        }
        td {
            background-color: #fff;
        }
        .positive { color: green; }
        .negative { color: red; }

        /* سبک شطرنج */
        #chessboard {
            margin: 40px auto;
            display: grid;
            grid-template-columns: repeat(8, 60px);
            grid-template-rows: repeat(8, 60px);
            border: 2px solid #333;
        }
        .cell {
            width: 60px;
            height: 60px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 36px;
            cursor: pointer;
        }
        .white { background-color: #f0d9b5; }
        .black { background-color: #b58863; }
        #status { margin-top: 20px; font-weight: bold; }
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
                <th>تغییر ۲۴ ساعته</th>
            </tr>
        </thead>
        <tbody id="crypto-table"></tbody>
    </table>

    <h1>بازی شطرنج با کامپیوتر</h1>
    <div id="chessboard"></div>
    <div id="status"></div>

    <script>
        // =================== قیمت کریپتو ===================
        const cryptoTable = document.getElementById('crypto-table');
        const coins = ['bitcoin', 'ethereum', 'dogecoin', 'litecoin', 'ripple'];

        async function fetchCryptoData() {
            const response = await fetch(`https://api.coingecko.com/api/v3/simple/price?ids=${coins.join(',')}&vs_currencies=usd&include_24hr_change=true`);
            const data = await response.json();

            cryptoTable.innerHTML = '';
            coins.forEach(coin => {
                const price = data[coin].usd.toFixed(2);
                const change = data[coin].usd_24h_change.toFixed(2);
                const changeClass = change >= 0 ? 'positive' : 'negative';
                cryptoTable.innerHTML += `
                    <tr>
                        <td>${coin.charAt(0).toUpperCase() + coin.slice(1)}</td>
                        <td>$${price}</td>
                        <td class="${changeClass}">${change}%</td>
                    </tr>
                `;
            });
        }

        fetchCryptoData();
        setInterval(fetchCryptoData, 15000); // هر ۱۵ ثانیه

        // =================== شطرنج ===================
        const board = document.getElementById('chessboard');
        let cells = [];
        let boardState = [];
        let selected = null;

        const initialBoard = [
            ['♜','♞','♝','♛','♚','♝','♞','♜'],
            ['♟','♟','♟','♟','♟','♟','♟','♟'],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['','','','','','','',''],
            ['♙','♙','♙','♙','♙','♙','♙','♙'],
            ['♖','♘','♗','♕','♔','♗','♘','♖']
        ];

        function createBoard() {
            board.innerHTML = '';
            cells = [];
            for (let r = 0; r < 8; r++) {
                let row = [];
                for (let c = 0; c < 8; c++) {
                    const cell = document.createElement('div');
                    cell.classList.add('cell');
                    cell.classList.add((r+c)%2 === 0 ? 'white' : 'black');
                    cell.dataset.row = r;
                    cell.dataset.col = c;
                    cell.innerText = initialBoard[r][c];
                    cell.addEventListener('click', onCellClick);
                    board.appendChild(cell);
                    row.push(cell);
                }
                cells.push(row);
            }
            boardState = initialBoard.map(r => r.slice());
        }

        function onCellClick(e) {
            const r = parseInt(e.currentTarget.dataset.row);
            const c = parseInt(e.currentTarget.dataset.col);
            const piece = boardState[r][c];

            if (selected) {
                // حرکت دادن مهره
                boardState[r][c] = boardState[selected.row][selected.col];
                boardState[selected.row][selected.col] = '';
                selected = null;
                renderBoard();
                setTimeout(aiMove, 500); // حرکت کامپیوتر بعد از نیم ثانیه
            } else if (piece && piece === piece.toUpperCase()) {
                selected = {row: r, col: c};
            }
        }

        function renderBoard() {
            for (let r = 0; r < 8; r++) {
                for (let c = 0; c < 8; c++) {
                    cells[r][c].innerText = boardState[r][c];
                }
            }
        }

        function aiMove() {
            // حرکت تصادفی ساده برای مهره‌های سیاه
            let moves = [];
            for (let r = 0; r < 8; r++) {
                for (let c = 0; c < 8; c++) {
                    if (boardState[r][c] && boardState[r][c] === boardState[r][c].toLowerCase()) {
                        // مهره سیاه، حرکات به جلو فقط ساده
                        let dr = 1;
                        let nr = r + dr;
                        if (nr < 8 && boardState[nr][c] === '') {
                            moves.push({from:{r,c}, to:{r:nr,c}});
                        }
                    }
                }
            }
            if (moves.length > 0) {
                const move = moves[Math.floor(Math.random() * moves.length)];
                boardState[move.to.r][move.to.c] = boardState[move.from.r][move.from.c];
                boardState[move.from.r][move.from.c] = '';
                renderBoard();
            }
        }

        createBoard();
    </script>
</body>
</html>
