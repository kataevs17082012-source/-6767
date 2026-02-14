<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>😺 Котик Кликер 🥭</title>

<style>
body {
    font-family: Arial, sans-serif;
    text-align: center;
    background: linear-gradient(135deg, #ffeaa7, #fab1a0);
    margin: 0;
    padding: 20px;
}

h1 { font-size: 30px; }

#mangoCount {
    font-size: 22px;
    margin: 10px;
}

#cat {
    font-size: 100px;
    cursor: pointer;
    transition: transform 0.1s;
    user-select: none;
}

.bounce {
    animation: bounce 0.2s;
}

@keyframes bounce {
    0% { transform: scale(1); }
    50% { transform: scale(0.85); }
    100% { transform: scale(1); }
}

button {
    padding: 8px 15px;
    margin: 5px;
    border-radius: 10px;
    border: none;
    background: #fdcb6e;
}

#shop, #achievements {
    display: none;
    margin-top: 15px;
    background: white;
    padding: 10px;
    border-radius: 15px;
}

#notification {
    position: fixed;
    top: 20px;
    right: 20px;
    background: gold;
    padding: 10px;
    border-radius: 10px;
    display: none;
}
</style>
</head>
<body>

<h1>😺 Котик Кликер 🥭</h1>

<div id="mangoCount">Манго: 0 🥭</div>
<div id="cat">😺</div>

<br>
<button onclick="toggleShop()">МАГАЗИН</button>
<button onclick="toggleAchievements()">🏆 ДОСТИЖЕНИЯ</button>

<div id="shop">
    <button onclick="upgradeClick()">Улучшить клик — <span id="upgradeCost">10</span> 🥭</button><br>
    <button onclick="buyAuto()">Авто-кот — <span id="autoCost">50</span> 🥭</button>
</div>

<div id="achievements">
    <h3>🏆 Достижения</h3>
    <ul id="achList"></ul>
</div>

<div id="notification"></div>

<audio id="meow">
<source src="https://www.myinstants.com/media/sounds/cat-meow.mp3">
</audio>

<script>
let mango = 0;
let clickPower = 1;
let autoPower = 0;
let upgradeCost = 10;
let autoCost = 50;
let totalClicks = 0;

let achievements = {
    click10: false,
    mango100: false,
    auto1: false
};

const cat = document.getElementById("cat");
const display = document.getElementById("mangoCount");
const meow = document.getElementById("meow");
const notification = document.getElementById("notification");

function save(){
    localStorage.setItem("catGame2", JSON.stringify({
        mango, clickPower, autoPower, upgradeCost, autoCost, totalClicks, achievements
    }));
}

function load(){
    let data = JSON.parse(localStorage.getItem("catGame2"));
    if(data){
        mango=data.mango;
        clickPower=data.clickPower;
        autoPower=data.autoPower;
        upgradeCost=data.upgradeCost;
        autoCost=data.autoCost;
        totalClicks=data.totalClicks;
        achievements=data.achievements;
    }
}
load();

function update(){
    display.textContent = "Манго: " + mango + " 🥭";
    document.getElementById("upgradeCost").textContent = upgradeCost;
    document.getElementById("autoCost").textContent = autoCost;
    renderAchievements();
}

cat.addEventListener("click", ()=>{
    totalClicks++;

    let gain = clickPower;

    // 🌈 5% шанс радужного манго
    if(Math.random() < 0.05){
        gain *= 10;
        showNotification("🌈 Радужное манго! x10!");
    }

    mango += gain;

    cat.classList.add("bounce");
    setTimeout(()=>cat.classList.remove("bounce"),200);

    meow.currentTime = 0;
    meow.play();

    checkAchievements();
    update();
    save();
});

function toggleShop(){
    let shop=document.getElementById("shop");
    shop.style.display=shop.style.display==="none"?"block":"none";
}

function toggleAchievements(){
    let ach=document.getElementById("achievements");
    ach.style.display=ach.style.display==="none"?"block":"none";
}

function upgradeClick(){
    if(mango>=upgradeCost){
        mango-=upgradeCost;
        clickPower++;
        upgradeCost=Math.floor(upgradeCost*1.5);
        update();
        save();
    }
}

function buyAuto(){
    if(mango>=autoCost){
        mango-=autoCost;
        autoPower++;
        autoCost=Math.floor(autoCost*1.6);
        checkAchievements();
        update();
        save();
    }
}

function checkAchievements(){
    if(totalClicks>=10 && !achievements.click10){
        achievements.click10=true;
        showNotification("🏆 10 кликов!");
    }
    if(mango>=100 && !achievements.mango100){
        achievements.mango100=true;
        showNotification("🏆 100 манго!");
    }
    if(autoPower>=1 && !achievements.auto1){
        achievements.auto1=true;
        showNotification("🏆 Куплен авто-кот!");
    }
}

function renderAchievements(){
    let list=document.getElementById("achList");
    list.innerHTML="";
    for(let key in achievements){
        if(achievements[key]){
            let li=document.createElement("li");
            li.textContent="✅ "+key;
            list.appendChild(li);
        }
    }
}

function showNotification(text){
    notification.textContent=text;
    notification.style.display="block";
    setTimeout(()=>notification.style.display="none",2000);
}

setInterval(()=>{
    mango+=autoPower;
    update();
    save();
},1000);

update();
</script>

</body>
</html>
