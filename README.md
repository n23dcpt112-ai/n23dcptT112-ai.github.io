<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <title>Shopping Cart Demo</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 20px; }
    h1 { text-align: center; }
    .container { display: flex; gap: 40px; }
    .products, .cart { width: 45%; }
    .product, .cart-item {
      border: 1px solid #ccc; padding: 10px; margin: 10px 0;
      display: flex; justify-content: space-between; align-items: center;
    }
    button { padding: 5px 10px; cursor: pointer; }
    .cart-item input { width: 50px; text-align: center; }
  </style>
</head>
<body>
  <h1>Shopping Cart Demo</h1>
  <div class="container">
    <!-- Danh sách sản phẩm -->
    <div class="products">
      <h2>Sản phẩm</h2>
      <div class="product">
        <span>Áo thun</span>
        <span>100,000đ</span>
        <button onclick="addToCart('Áo thun', 100000)">Thêm</button>
      </div>
      <div class="product">
        <span>Quần jeans</span>
        <span>250,000đ</span>
        <button onclick="addToCart('Quần jeans', 250000)">Thêm</button>
      </div>
      <div class="product">
        <span>Giày sneaker</span>
        <span>500,000đ</span>
        <button onclick="addToCart('Giày sneaker', 500000)">Thêm</button>
      </div>
    </div>

    <!-- Giỏ hàng -->
    <div class="cart">
      <h2>Giỏ hàng</h2>
      <div id="cart-items"></div>
      <h3>Tổng cộng: <span id="total">0</span> đ</h3>
    </div>
  </div>

  <script>
    let cart = [];

    function addToCart(name, price) {
      let item = cart.find(p => p.name === name);
      if (item) {
        item.quantity++;
      } else {
        cart.push({ name, price, quantity: 1 });
      }
      renderCart();
    }

    function removeFromCart(name) {
      cart = cart.filter(p => p.name !== name);
      renderCart();
    }

    function updateQuantity(name, qty) {
      let item = cart.find(p => p.name === name);
      if (item) {
        item.quantity = parseInt(qty) || 1;
      }
      renderCart();
    }

    function renderCart() {
      let cartContainer = document.getElementById("cart-items");
      cartContainer.innerHTML = "";
      let total = 0;

      cart.forEach(p => {
        total += p.price * p.quantity;
        cartContainer.innerHTML += `
          <div class="cart-item">
            <span>${p.name} (${p.price}đ)</span>
            <input type="number" value="${p.quantity}" min="1" onchange="updateQuantity('${p.name}', this.value)">
            <button onclick="removeFromCart('${p.name}')">Xóa</button>
          </div>
        `;
      });

      document.getElementById("total").innerText = total.toLocaleString();
    }
  </script>
</body>
</html>
