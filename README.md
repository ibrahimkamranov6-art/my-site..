<!DOCTYPE html>
<html>
<head>
<title>My Site</title>
</head>
<body>
<h1>Salam</h1>
<p>Bu mənim ilk saytımdır.</p>
</body>
</html><!DOCTYPE html>
<html>
<head>
<title>İbrahimin Mega Restoran Oyunu</title>
</head>
<body style="background-color:#fff8e7; font-family:Arial; text-align:center;">

<h1>🍽️ İbrahimin Mega Restoran Oyunu 🍽️</h1>
<p>Yeməkləri sat, müştəriləri məmnun et və restoranını böyüt! 😎</p>

<!-- Status -->
<p id="money">Büdcən: 0 AZN 💰</p>
<p id="score">Xal: 0 ⭐</p>
<p id="stock">Stok: Burger 5, Pizza 5, Salad 5, Sushi 3, Donut 3</p>
<p id="customer" style="margin-top:20px; font-weight:bold;"></p>
<p id="result" style="margin-top:10px;"></p>

<!-- Yemək düymələri -->
<button onclick="serve('Burger')">Burger 🍔 (Sat 15 AZN)</button>
<button onclick="serve('Pizza')">Pizza 🍕 (Sat 20 AZN)</button>
<button onclick="serve('Salad')">Salad 🥗 (Sat 10 AZN)</button>
<button onclick="serve('Sushi')">Sushi 🍣 (Sat 25 AZN)</button>
<button onclick="serve('Donut')">Donut 🍩 (Sat 8 AZN)</button>

<br><br>

<!-- Al düymələri -->
<button onclick="buy('Burger')">Al Burger 🛒 (10 AZN)</button>
<button onclick="buy('Pizza')">Al Pizza 🛒 (10 AZN)</button>
<button onclick="buy('Salad')">Al Salad 🛒 (5 AZN)</button>
<button onclick="buy('Sushi')">Al Sushi 🛒 (15 AZN)</button>
<button onclick="buy('Donut')">Al Donut 🛒 (4 AZN)</button>

<!-- Şəkil -->
<img id="food-img" src="" width="150" style="display:none; margin-top:20px; border-radius:10px;">

<script>
let money = 100;
let score = 0;
let stock = { Burger:5, Pizza:5, Salad:5, Sushi:3, Donut:3 };

// Müştərilər
let customers = [
    {type:"səbirli", patience:3},
    {type:"tələskən", patience:1},
    {type:"bonus", patience:2} // bonus müştəri
];

function updateUI() {
    document.getElementById('money').innerText = `Büdcən: ${money} AZN 💰`;
    document.getElementById('score').innerText = `Xal: ${score} ⭐`;
    document.getElementById('stock').innerText = `Stok: Burger ${stock.Burger}, Pizza ${stock.Pizza}, Salad ${stock.Salad}, Sushi ${stock.Sushi}, Donut ${stock.Donut}`;
}

// Random müştəri yarat
function newCustomer() {
    const foodChoices = ["Burger","Pizza","Salad","Sushi","Donut"];
    let customerType = customers[Math.floor(Math.random()*customers.length)];
    let choice = foodChoices[Math.floor(Math.random()*foodChoices.length)];
    document.getElementById('customer').innerText = `Müştəri (${customerType.type}) istəyir: ${choice}`;
    return {food:choice, type:customerType.type};
}

let currentCustomer = newCustomer();

function serve(food) {
    let img = document.getElementById('food-img');
    // Şəkillər
    if(food === "Burger") img.src = "https://upload.wikimedia.org/wikipedia/commons/0/0b/RedDot_Burger.jpg";
    if(food === "Pizza") img.src = "https://upload.wikimedia.org/wikipedia/commons/d/d3/Pizza_on_stone.jpg";
    if(food === "Salad") img.src = "https://upload.wikimedia.org/wikipedia/commons/3/39/Salad_platter.jpg";
    if(food === "Sushi") img.src = "https://upload.wikimedia.org/wikipedia/commons/6/60/Sushi_platter.jpg";
    if(food === "Donut") img.src = "https://upload.wikimedia.org/wikipedia/commons/d/d5/Glazed-Donut.jpg";
    
    img.style.display = "block";

    // Satış və bonus
    let foodPrice = food==="Burger"?15:food==="Pizza"?20:food==="Salad"?10:food==="Sushi"?25:8;
    let bonus = Math.random()<0.1; // 10% bonus xalı
    if(stock[food] > 0) {
        if(food === currentCustomer.food) {
            stock[food]--;
            money += foodPrice;
            let earnedScore = bonus ? 20 : 10;
            score += earnedScore;
            document.getElementById('result').innerText = bonus ? `🎉 Bonus müştəri! ${food} ikiqat xal qazandırdı!` : `✅ ${food} satdın və müştəri məmnun oldu!`;
        } else {
            stock[food]--;
            money += foodPrice/2; // səhv yemək satılsa az gəlir
            document.getElementById('result').innerText = `⚠ Müştəri istəmədi! Yarım qiymət qazandın.`;
        }
    } else {
        document.getElementById('result').innerText = `${food} stokda yoxdur!`;
    }
    currentCustomer = newCustomer();
    updateUI();
}

function buy(food) {
    let cost = food==="Burger"?5:food==="Pizza"?8:food==="Salad"?3:food==="Sushi"?12:food==="Donut"?4:0;
    if(money >= cost) {
        stock[food]++;
        money -= cost;
        document.getElementById('result').innerText = `🛒 ${food} aldın və stokunu artırdın!`;
    } else {
        document.getElementById('result').innerText = `💸 Büdcən çatmadı!`;
    }
    updateUI();
}

updateUI();
</script>
</body>
</html>
