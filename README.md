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

<script>
const inhalt = document.getElementById("inhalt");

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

function speichern() {
    const name = document.getElementById("name").value;
    const bild = document.getElementById("bild").files[0];

    if (!name || !bild) {
        alert("Bitte Name und Bild eingeben!");
        return;
    }

    const reader = new FileReader();
    reader.onload = function() {
        let daten = JSON.parse(localStorage.getItem("castelDaten")) || [];
        daten.push({
            name: name,
            bild: reader.result
        });

        localStorage.setItem("castelDaten", JSON.stringify(daten));
        alert("Daten gespeichert!");
        datenAnzeigen();
    };
    reader.readAsDataURL(bild);
}

function datenAnzeigen() {
    let daten = JSON.parse(localStorage.getItem("castelDaten")) || [];
    inhalt.innerHTML = "<h2>Gespeicherte Daten</h2>";

    if (daten.length === 0) {
        inhalt.innerHTML += "<p>Noch keine Daten vorhanden.</p>";
        return;
    }

    daten.forEach(eintrag => {
        inhalt.innerHTML += `
            <div class="entry">
                <strong>Letzte Attacke auf:</strong> ${eintrag.name}
                <br>
                <img src="${eintrag.bild}">
            </div>
        `;
    });
}
</script>

</body>
</html>
