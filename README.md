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
<button onclick="neueDaten()">➕ Neue Daten hinzufügen</button>
<button onclick="datenAnzeigen()">📂 Gespeicherte Daten ansehen</button>

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
        <h2>Neue Attacke</h2>

        <p>Wen hast du zuletzt angegriffen?</p>
        <input type="text" id="name" placeholder="Name eingeben">

        <p>Bitte lade ein Bild hoch:</p>
        <input type="file" id="bild" accept="image/*">

        <br><br>
        <button onclick="speichern()">Speichern</button>
    `;
}

/* ==============================
   Daten SPEICHERN (ONLINE)
   ============================== */
function speichern() {
    const name = document.getElementById("name").value;
    const bild = document.getElementById("bild").files[0];

    if (!name || !bild) {
        alert("Bitte Name und Bild eingeben!");
        return;
    }

    const reader = new FileReader();
    reader.onload = function() {
        db.ref("castelDaten").push({
            name: name,
            bild: reader.result,
            zeit: new Date().toLocaleString()
        });

        alert("Daten online gespeichert!");
        datenAnzeigen();
    };
    reader.readAsDataURL(bild);
}

/* ==============================
   Daten ANZEIGEN (ONLINE)
   ============================== */
function datenAnzeigen() {
    inhalt.innerHTML = "<h2>Gespeicherte Daten</h2>";

    db.ref("castelDaten").once("value", snapshot => {
        const daten = snapshot.val();

        if (!daten) {
            inhalt.innerHTML += "<p>Noch keine Daten vorhanden.</p>";
            return;
        }

        for (let key in daten) {
            const e = daten[key];
            inhalt.innerHTML += `
                <div class="entry">
                    <strong>Letzte Attacke auf:</strong> ${e.name}<br>
                    <small>${e.zeit}</small><br>
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
