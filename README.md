
<html lang="en">
<head>
<meta charset="UTF-8">
<title>🏰 Kushan Empire</title>
<style>
body {
    font-family: Arial, sans-serif;
    background: #1a1a1a;
    color: #ffffff;
    padding: 20px;
    margin: 0;
}
h1, h2 {
    text-align: center;
}
button {
    padding: 8px 15px;
    margin: 5px;
    border: none;
    cursor: pointer;
    border-radius: 5px;
    font-weight: bold;
}
button:hover {
    opacity: 0.8;
}
input {
    padding: 8px;
    margin: 5px 0;
    width: 100%;
    border-radius: 5px;
    border: 1px solid #ccc;
}
img {
    max-width: 200px;
    margin-top: 10px;
    border-radius: 5px;
}
.entry {
    border: 1px solid #555;
    padding: 10px;
    margin-top: 10px;
    border-radius: 5px;
    background: #222;
}
#language-select button {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    background: #444;
    color: #fff;
}
#section-select button {
    background: #555;
    color: #fff;
}
.admin-btn {
    background: #ff4444;
    color: #fff;
}
.delete-btn {
    background: #cc0000;
    color: #fff;
    margin-top: 5px;
}
.entry {
  text-align: center;
}
</style>
</head>
<body>

<!-- Sprachwahl -->
<div id="language-select" style="text-align:center; margin-bottom:10px;">
    <button onclick="setLang('en')"><img src="https://flagcdn.com/us.svg" width="25"> English</button>
    <button onclick="setLang('fa')"><img src="https://flagcdn.com/ir.svg" width="25"> فارسی</button>
    <button onclick="setLang('dr')"><img src="https://flagcdn.com/af.svg" width="25"> Dari</button>
</div>

<h1>🏰 Kushan Empire</h1>
<p></p>
<!-- Abteilungsauswahl -->
<div id="section-select" style="text-align:center; margin-bottom:10px;">
    <button onclick="selectSection('kushan1')">🏰 Kushan 1 🔒</button>
    <button onclick="selectSection('kushan2')">🏰 Kushan 2 🔓</button>
</div>

<!-- Menü -->
<div style="text-align:center; margin-bottom:10px;">
    <button onclick="neueDaten()">➕ Add New Data</button>
    <button onclick="datenAnzeigen()">📂 View Saved Data</button>
</div>

<hr>
<div id="inhalt"></div>

<!-- Firebase -->
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>

<script>
// ===================== FIREBASE =====================
const firebaseConfig = {
  apiKey: "AIzaSyDQvWVcwDQivNGysnvfm4fBBykNbYtnDZc",
  authDomain: "castel-game.firebaseapp.com",
  databaseURL: "https://castel-game-default-rtdb.europe-west1.firebasedatabase.app",
  projectId: "castel-game",
  storageBucket: "castel-game.appspot.com",
  messagingSenderId: "504558378124",
  appId: "1:504558378124:web:f494c563b755454770efa9"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

// ===================== TEXTE =====================
const texts = {
    en: {
        title: "🏰 Kushan Empire",
        addData: "➕ Add New Data",
        viewData: "📂 View Saved Data",
        newAttack: "New Attack",
        attackName: "Who attacked?",
        enterName: "Enter attacker name",
        attackTarget: "Who was attacked?",
        enterTarget: "Enter target name",
        attackAlliance: "Which alliance was attacked?",
        enterAlliance: "Enter alliance",
        uploadImage: "Please upload an image",
        save: "Save",
        noData: "No data available.",
        lastAttack: "Last attack",
        time: "Time",
        alertEnter: "Please fill all fields and upload an image!",
        alertSaved: "Data saved online!",
        wrongPassword: "❌ Wrong password!",
        accessGranted: "✅ Access granted to ",
        delete: "Delete"
    },
    fa: {
        title: "🏰 امپراتوری کوشان",
        addData: "➕ افزودن داده جدید",
        viewData: "📂 مشاهده داده‌های ذخیره‌شده",
        newAttack: "حمله جدید",
        attackName: "چه کسی حمله کرد؟",
        enterName: "نام مهاجم را وارد کنید",
        attackTarget: "به چه کسی حمله شد؟",
        enterTarget: "نام هدف را وارد کنید",
        attackAlliance: "کدام اتحاد مورد حمله قرار گرفت؟",
        enterAlliance: "نام اتحاد را وارد کنید",
        uploadImage: "لطفاً یک تصویر آپلود کنید",
        save: "ذخیره",
        noData: "داده‌ای موجود نیست",
        lastAttack: "آخرین حمله",
        time: "زمان",
        alertEnter: "لطفاً همه فیلدها را پر کنید و تصویر را آپلود کنید!",
        alertSaved: "داده‌ها آنلاین ذخیره شدند!",
        wrongPassword: "❌ رمز عبور اشتباه است!",
        accessGranted: "✅ دسترسی به ",
        delete: "حذف"
    },
    dr: {
        title: "🏰 امپراتوری کوشان",
        addData: "➕ افزودن داده جدید",
        viewData: "📂 دیدن داده‌های ذخیره‌شده",
        newAttack: "حمله جدید",
        attackName: "چه کسی حمله کرد؟",
        enterName: "نام مهاجم را وارد کنید",
        attackTarget: "به چه کسی حمله شد؟",
        enterTarget: "نام هدف را وارد کنید",
        attackAlliance: "کدام اتحاد مورد حمله قرار گرفت؟",
        enterAlliance: "نام اتحاد را وارد کنید",
        uploadImage: "لطفاً یک تصویر بارگذاری کنید",
        save: "ذخیره",
        noData: "داده‌ای موجود نیست",
        lastAttack: "آخرین حمله",
        time: "زمان",
        alertEnter: "لطفاً همه فیلدها را پر کنید و تصویر را بارگذاری کنید!",
        alertSaved: "داده‌ها آنلاین ذخیره شدند!",
        wrongPassword: "❌ رمز عبور اشتباه است!",
        accessGranted: "✅ دسترسی به ",
        delete: "حذف"
    }
};

// ===================== VARIABLEN =====================
let currentLang = 'en';
let currentSection = 'kushan2';
const KUSHAN1_PASSWORD = "12345"; // 🔒 Kushan 1 Passwort
let isAdmin = false;
const ADMIN_PASSWORD = "admin123"; // 🔒 Admin Passwort
const inhalt = document.getElementById("inhalt");

// ===================== SPRACHWECHSEL =====================
function setLang(lang){
    currentLang = lang;
    document.title = texts[lang].title;
    document.querySelector('h1').innerText = texts[lang].title;
    datenAnzeigen();
}

// ===================== ABTEILUNG =====================
function selectSection(section){
    if(section === "kushan1"){
        const pw = prompt("Enter password for Kushan 1:");
        if(pw !== KUSHAN1_PASSWORD){
            alert(texts[currentLang].wrongPassword);
            return;
        }
    }
    currentSection = section;
    alert(texts[currentLang].accessGranted + section.toUpperCase());
    datenAnzeigen();
}

// ===================== NEUE DATEN =====================
function neueDaten(){
    inhalt.innerHTML = `
        <h2>${texts[currentLang].newAttack}</h2>
        <p>${texts[currentLang].attackName}</p>
        <input type="text" id="attacker" placeholder="${texts[currentLang].enterName}">
        <p>${texts[currentLang].attackTarget}</p>
        <input type="text" id="target" placeholder="${texts[currentLang].enterTarget}">
        <p>${texts[currentLang].attackAlliance}</p>
        <input type="text" id="alliance" placeholder="${texts[currentLang].enterAlliance}">
        <p>${texts[currentLang].uploadImage}</p>
        <input type="file" id="bild" accept="image/*">
        <br><br>
        <button onclick="speichern()">${texts[currentLang].save}</button>
    `;
}

// ===================== DATEN SPEICHERN =====================
function speichern(){
    const attacker = document.getElementById("attacker").value;
    const target = document.getElementById("target").value;
    const alliance = document.getElementById("alliance").value;
    const bild = document.getElementById("bild").files[0];

    if(!attacker || !target || !alliance || !bild){
        alert(texts[currentLang].alertEnter);
        return;
    }

    const reader = new FileReader();
    reader.onload = function(){
        db.ref("castelDaten/"+currentSection).push({
            attacker: attacker,
            target: target,
            alliance: alliance,
            bild: reader.result,
            zeit: new Date().toLocaleString()
        });
        alert(texts[currentLang].alertSaved);
        datenAnzeigen();
    }
    reader.readAsDataURL(bild);
}

// ===================== DATEN ANZEIGEN =====================
function datenAnzeigen(){
    inhalt.innerHTML = `<h2>Saved Data – ${currentSection.toUpperCase()}</h2>`;
    db.ref("castelDaten/"+currentSection).once("value", snapshot=>{
        const daten = snapshot.val();
        if(!daten){
            inhalt.innerHTML += `<p>${texts[currentLang].noData}</p>`;
            return;
        }
        
        for(let key in daten){
            const e = daten[key];
            let html = `
                <div class="entry">
                    <strong>${texts[currentLang].attackName}:</strong> ${e.attacker}<br>
                    <strong>${texts[currentLang].attackTarget}:</strong> ${e.target}<br>
                    <strong>${texts[currentLang].attackAlliance}:</strong> ${e.alliance}<br>
                    <small>${e.zeit} / ${texts[currentLang].time}</small><br>
                    <img src="${e.bild}"><br>
            `;
            if(isAdmin){
                html += `<button class="delete-btn" onclick="deleteEntry('${key}')">${texts[currentLang].delete}</button>`;
            }
            html += `</div>`;
            inhalt.innerHTML += html;
        }
    });
}

// ===================== ADMIN LOGIN =====================
function adminLogin(){
    const pw = prompt("Enter admin password:");
    if(pw === ADMIN_PASSWORD){
        isAdmin = true;
        alert("✅ Admin access granted!");
        datenAnzeigen();
    } else{
        alert("❌ Wrong password!");
    }
}

// ===================== DATEN LÖSCHEN =====================
function deleteEntry(key){
    if(!isAdmin){
        alert("❌ Admin only!");
        return;
    }
    if(confirm("Do you really want to delete this entry?")){
        db.ref("castelDaten/"+currentSection+"/"+key).remove();
        datenAnzeigen();
    }
}

// ===================== AUTOMATISCH LADEN =====================
window.onload = datenAnzeigen;
</script>
</body>
</html>
