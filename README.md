<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Castel Alliance</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #0f0f0f;
    color: #ffffff;
    padding: 20px;
}

button {
    padding: 10px 15px;
    margin: 5px;
    background: #444;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background: #666;
}

input {
    padding: 8px;
    margin: 5px 0;
    width: 100%;
}

img {
    max-width: 250px;
    margin-top: 10px;
}

.entry {
    border: 1px solid #333;
    padding: 10px;
    margin-top: 10px;
}
</style>
</head>

<body>

<h1>🏰 Kushan Empire</h1>

<!-- Menü -->
<button onclick="neueDaten()">➕ Add New Data / افزودن داده جدید</button>
<button onclick="datenAnzeigen()">📂 View Saved Data / مشاهده داده‌های ذخیره‌شده</button>

<hr>

<div id="inhalt"></div>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

<script>
/* ==============================
   🔴 HIER DEINE FIREBASE DATEN
   ============================== */
const firebaseConfig = {
  apiKey: "AIzaSyDQvWVcwDQivNGysnvfm4fBBykNbYtnDZc",
  authDomain: "castel-game.firebaseapp.com",
  databaseURL: "https://castel-game-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "castel-game",
  storageBucket: "castel-game.firebasestorage.app",
  messagingSenderId: "504558378124",
  appId: "1:504558378124:web:f494c563b755454770efa9"
};

/* Firebase starten */
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

/* Inhalt */
const inhalt = document.getElementById("inhalt");

/* ==============================
   Neue Daten Formular
   ============================== */
function neueDaten() {
    inhalt.innerHTML = `
        <h2>New Attack / حمله جدید</h2>

        <p>Who did you attack last? / آخرین حمله به کی بود؟</p>
        <input type="text" id="name" placeholder="Enter Name / وارد کردن نام">

        <p>Please upload an image / لطفاً یک تصویر آپلود کنید:</p>
        <input type="file" id="bild" accept="image/*">

        <br><br>
        <button onclick="speichern()">Save / ذخیره</button>
    `;
}

/* ==============================
   Daten SPEICHERN (ONLINE)
   ============================== */
function speichern() {
    const name = document.getElementById("name").value;
    const bild = document.getElementById("bild").files[0];

    if (!name || !bild) {
        alert("Please enter name and image! / لطفاً نام و تصویر را وارد کنید");
        return;
    }

    const reader = new FileReader();
    reader.onload = function() {
        db.ref("castelDaten").push({
            name: name,
            bild: reader.result,
            zeit: new Date().toLocaleString()
        });

        alert("Data saved online! / داده‌ها آنلاین ذخیره شدند!");
        datenAnzeigen();
    };
    reader.readAsDataURL(bild);
}

/* ==============================
   Daten ANZEIGEN (ONLINE)
   ============================== */
function datenAnzeigen() {
    inhalt.innerHTML = "<h2>Saved Data / داده‌های ذخیره‌شده</h2>";

    db.ref("castelDaten").once("value", snapshot => {
        const daten = snapshot.val();

        if (!daten) {
            inhalt.innerHTML += "<p>No data available. / داده‌ای موجود نیست</p>";
            return;
        }

        for (let key in daten) {
            const e = daten[key];
            inhalt.innerHTML += `
                <div class="entry">
                    <strong>Last attack on:</strong> ${e.name} / آخرین حمله به: ${e.name}<br>
                    <small>${e.zeit} / Time / زمان</small><br>
                    <img src="${e.bild}">
                </div>
            `;
        }
    });
}

/* ==============================
   Automatisch beim Start laden
   ============================== */
window.onload = datenAnzeigen;
</script>

</body>
</html>
