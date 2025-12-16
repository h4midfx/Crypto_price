<!DOCTYPE html>
<html lang="de">
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

<h1>🏰 Castel Alliance</h1>

<!-- Menü -->
<button onclick="neueDaten()">➕ Neue Daten hinzufügen / Add New Data / افزودن داده جدید</button>
<button onclick="datenAnzeigen()">📂 Gespeicherte Daten ansehen / View Saved Data / مشاهده داده‌های ذخیره‌شده</button>

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
  apiKey: "DEIN_API_KEY",
  authDomain: "DEIN_PROJEKT.firebaseapp.com",
  databaseURL: "https://DEIN_PROJEKT.firebaseio.com",
  projectId: "DEIN_PROJEKT",
  storageBucket: "DEIN_PROJEKT.appspot.com",
  messagingSenderId: "DEINE_ID",
  appId: "DEINE_APP_ID"
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
        <h2>Neue Attacke / New Attack / حمله جدید</h2>

        <p>Wen hast du zuletzt angegriffen? / Who did you attack last? / آخرین حمله به کی بود؟</p>
        <input type="text" id="name" placeholder="Name eingeben / Enter Name / وارد کردن نام">

        <p>Bitte lade ein Bild hoch / Please upload an image / لطفاً یک تصویر آپلود کنید:</p>
        <input type="file" id="bild" accept="image/*">

        <br><br>
        <button onclick="speichern()">Speichern / Save / ذخیره</button>
    `;
}

/* ==============================
   Daten SPEICHERN (ONLINE)
   ============================== */
function speichern() {
    const name = document.getElementById("name").value;
    const bild = document.getElementById("bild").files[0];

    if (!name || !bild) {
        alert("Bitte Name und Bild eingeben! / Please enter name and image! / لطفاً نام و تصویر را وارد کنید");
        return;
    }

    const reader = new FileReader();
    reader.onload = function() {
        db.ref("castelDaten").push({
            name: name,
            bild: reader.result,
            zeit: new Date().toLocaleString()
        });

        alert("Daten online gespeichert! / Data saved online! / داده‌ها آنلاین ذخیره شدند!");
        datenAnzeigen();
    };
    reader.readAsDataURL(bild);
}

/* ==============================
   Daten ANZEIGEN (ONLINE)
   ============================== */
function datenAnzeigen() {
    inhalt.innerHTML = "<h2>Gespeicherte Daten / Saved Data / داده‌های ذخیره‌شده</h2>";

    db.ref("castelDaten").once("value", snapshot => {
        const daten = snapshot.val();

        if (!daten) {
            inhalt.innerHTML += "<p>Noch keine Daten vorhanden. / No data available. / داده‌ای موجود نیست</p>";
            return;
        }

        for (let key in daten) {
            const e = daten[key];
            inhalt.innerHTML += `
                <div class="entry">
                    <strong>Letzte Attacke auf:</strong> ${e.name} / Last attack on: ${e.name} / آخرین حمله به: ${e.name}<br>
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
