<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>سوبر ماركت النخبة | Elite Market</title>
    
    <style>
        /* --- التصميم (CSS) --- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: #0b0b0b; /* أسود فخم */
            color: #ffffff;
            overflow-x: hidden;
        }

        /* شريط التنقل */
        nav {
            display: flex;
            justify-content: space-around;
            align-items: center;
            padding: 15px;
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            position: sticky;
            top: 0;
            z-index: 1000;
            border-bottom: 1px solid #222;
        }

        .logo { font-size: 24px; font-weight: bold; }
        .logo span { color: #ffcc00; }

        .cart-btn-nav {
            background: #ffcc00;
            color: black;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            font-weight: bold;
            transition: 0.3s;
        }

        /* واجهة العرض */
        header {
            height: 40vh;
            display: flex;
            align-items: center;
            justify-content: center;
            text-align: center;
            background: linear-gradient(rgba(0,0,0,0.7), #0b0b0b), url('https://images.unsplash.com/photo-1542838132-92c53300491e?auto=format&fit=crop&w=1000&q=80');
            background-size: cover;
            background-position: center;
        }

        /* شبكة المنتجات */
        .products-section { padding: 40px 10%; }
        .grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 25px;
        }

        .card {
            background: #161616;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
            border: 1px solid #222;
            transition: 0.3s;
        }

        .card:hover {
            transform: translateY(-10px);
            border-color: #ffcc00;
        }

        .card img {
            width: 100%;
            height: 150px;
            object-fit: cover;
            border-radius: 10px;
            margin-bottom: 15px;
        }

        .price { color: #ffcc00; font-weight: bold; margin: 10px 0; }

        .add-btn {
            width: 100%;
            padding: 10px;
            background: transparent;
            border: 1px solid #ffcc00;
            color: #ffcc00;
            border-radius: 8px;
            cursor: pointer;
            transition: 0.3s;
        }

        .add-btn:hover { background: #ffcc00; color: black; }

        /* السلة الجانبية */
        .side-cart {
            position: fixed;
            top: 0;
            left: -400px;
            width: 350px;
            height: 100%;
            background: #111;
            z-index: 2000;
            transition: 0.4s;
            padding: 20px;
            display: flex;
            flex-direction: column;
            border-right: 1px solid #ffcc00;
        }

        .side-cart.active { left: 0; }

        .cart-item {
            display: flex;
            justify-content: space-between;
            padding: 10px;
            background: #1a1a1a;
            margin-bottom: 10px;
            border-radius: 5px;
        }

        .checkout-btn {
            margin-top: auto;
            padding: 15px;
            background: #ffcc00;
            border: none;
            width: 100%;
            font-weight: bold;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <nav>
        <div class="logo">سوبر<span>ماركت</span></div>
        <div class="cart-btn-nav" onclick="toggleCart()">🛒 السلة (<span id="count">0</span>)</div>
    </nav>

    <header>
        <h1>تسوق أرقى المنتجات العالمية</h1>
    </header>

    <section class="products-section">
        <div class="grid">
            <div class="card">
                <img src="https://images.unsplash.com/photo-1559056199-641a0ac8b55e?w=400" alt="قهوة">
                <h3>قهوة برازيلية</h3>
                <p class="price">50 ريال</p>
                <button class="add-btn" onclick="addToCart('قهوة برازيلية', 50)">أضف للسلة</button>
            </div>
            <div class="card">
                <img src="https://images.unsplash.com/photo-1523205771623-e0faa4d2813d?w=400" alt="شوكولاتة">
                <h3>شوكولاتة داكنة</h3>
                <p class="price">15 ريال</p>
                <button class="add-btn" onclick="addToCart('شوكولاتة داكنة', 15)">أضف للسلة</button>
            </div>
            <div class="card">
                <img src="https://images.unsplash.com/photo-1619566636858-adf3ef46400b?w=400" alt="فواكه">
                <h3>صندوق فواكه</h3>
                <p class="price">30 ريال</p>
                <button class="add-btn" onclick="addToCart('صندوق فواكه', 30)">أضف للسلة</button>
            </div>
        </div>
    </section>

    <div id="sideCart" class="side-cart">
        <h2 style="margin-bottom:20px;">سلة المشتريات <span onclick="toggleCart()" style="float:left; cursor:pointer;">&times;</span></h2>
        <div id="cartItems"></div>
        <hr style="margin: 20px 0; border: 0; border-top: 1px solid #333;">
        <h3>الإجمالي: <span id="totalPrice">0</span> ريال</h3>
        <button class="checkout-btn">إتمام الدفع الآن</button>
    </div>

    <script>
        /* --- البرمجة (JavaScript) --- */
        let cart = [];
        let total = 0;

        function toggleCart() {
            document.getElementById('sideCart').classList.toggle('active');
        }

        function addToCart(name, price) {
            cart.push({name, price});
            total += price;
            updateUI();
        }

        function updateUI() {
            document.getElementById('count').innerText = cart.length;
            document.getElementById('totalPrice').innerText = total;
            
            const container = document.getElementById('cartItems');
            container.innerHTML = '';
            
            cart.forEach((item, index) => {
                const div = document.createElement('div');
                div.className = 'cart-item';
                div.innerHTML = `<span>${item.name}</span> <span>${item.price} ريال</span>`;
                container.appendChild(div);
            });
        }
    </script>
</body>
</html>
