OK luôn bro 😎 — đây là bản **hoàn chỉnh**, bạn chỉ cần **copy toàn bộ** rồi **paste vào file `README.md`** trên GitHub là xong, không cần chỉnh gì thêm nhé.

---

````markdown
# 🛒 SHOPPING CART PROJECT

📌 **Giới thiệu**  
Dự án này được phát triển trong môn *Nhập môn Công nghệ Phần mềm*.  
Mục tiêu là áp dụng quy trình phát triển phần mềm, từ phân tích yêu cầu, thiết kế, lập trình, kiểm thử và triển khai.  

👤 **Sinh viên thực hiện:** Nguyễn Trần Quốc Tuấn  
📘 **Lớp:** N23DCPT112-AI  
🏫 **Trường:** Học viện Công nghệ Bưu chính Viễn thông (PTIT) – Cơ sở Hồ Chí Minh  

---

## 💻 Công nghệ sử dụng
- **Ngôn ngữ:** HTML, CSS, JavaScript  
- **IDE:** Visual Studio Code  
- **CSDL:** LocalStorage (mô phỏng chức năng giỏ hàng, có thể mở rộng sang MySQL)  
- **Quản lý phiên bản:** Git + GitHub  
- **Mô hình phát triển:** Agile – Scrum  
- **Quản lý dự án:** Jira  

---

## 📁 Cấu trúc thư mục dự án

```text
📦 MyWebsite  
┣ 📜 index.html  
┣ 📜 style.css  
┣ 📜 script.js  
┗ 📜 README.md  
````

---

## 📐 Lap_01: Đề tài Shopping Cart

### 🎯 Mục tiêu

Xây dựng hệ thống bán hàng online đơn giản, mô phỏng chức năng giỏ hàng với 2 vai trò chính: **Admin** và **Người dùng (Customer)**.

---

### 👤 Admin:

* Thêm sản phẩm
* Sửa thông tin sản phẩm
* Xóa sản phẩm

---

### 🛒 Người dùng (Customer):

* Tìm kiếm sản phẩm theo tên
* Thêm sản phẩm vào giỏ hàng
* Xóa sản phẩm khỏi giỏ hàng
* Thay đổi số lượng sản phẩm trong giỏ hàng
* Tính tổng tiền và hiển thị giỏ hàng theo thời gian thực

---

## 🧩 Mô tả hoạt động chính

1. **Trang chủ (index.html):** Hiển thị danh sách sản phẩm.
2. **Người dùng** có thể chọn sản phẩm → nhấn “Thêm vào giỏ hàng”.
3. **Giỏ hàng** hiển thị tất cả sản phẩm được thêm, kèm số lượng và tổng giá.
4. Có thể cập nhật số lượng hoặc xóa sản phẩm ra khỏi giỏ hàng.
5. Dữ liệu giỏ hàng được lưu tạm trong **LocalStorage**, giúp giữ lại thông tin khi tải lại trang.

---

## 📊 Biểu đồ & Tài liệu liên quan

* **Use Case Diagram:** *Use Case*
* **Sequence User Diagram:** *User Sequence*
* **Sequence Admin Diagram:** *Admin Sequence*
* **ERD (Entity Relationship Diagram):** *ERD*
* **Quản lý dự án Jira:** [Xem tại đây](https://student-team-tjrvokrv.atlassian.net/jira/software/projects/SC/boards/34/backlog)

---

## 🧠 Demo Code (HTML + JS tích hợp)

```html
<!DOCTYPE html>
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Shopping Cart</title>
  <style>
    body { font-family: Arial, sans-serif; margin: 40px; background: #f6f6f6; }
    h1 { text-align: center; color: #333; }
    .product, .cart { background: white; border-radius: 10px; padding: 20px; margin: 10px 0; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
    button { padding: 6px 10px; border: none; background: #007bff; color: white; border-radius: 5px; cursor: pointer; }
    button:hover { background: #0056b3; }
  </style>
</head>
<body>
  <h1>🛍️ SHOPPING CART</h1>

  <div class="product-list">
    <div class="product">
      <h3>Áo thun</h3>
      <p>Giá: 150.000₫</p>
      <button onclick="addToCart('Áo thun',150000)">Thêm vào giỏ hàng</button>
    </div>
    <div class="product">
      <h3>Quần jeans</h3>
      <p>Giá: 300.000₫</p>
      <button onclick="addToCart('Quần jeans',300000)">Thêm vào giỏ hàng</button>
    </div>
  </div>

  <div class="cart">
    <h2>🛒 Giỏ hàng</h2>
    <ul id="cartItems"></ul>
    <h3 id="totalPrice">Tổng: 0₫</h3>
  </div>

  <script>
    let cart = JSON.parse(localStorage.getItem("cart")) || [];

    function saveCart() {
      localStorage.setItem("cart", JSON.stringify(cart));
      renderCart();
    }

    function addToCart(name, price) {
      const item = cart.find(p => p.name === name);
      if (item) item.quantity++;
      else cart.push({ name, price, quantity: 1 });
      saveCart();
    }

    function removeFromCart(index) {
      cart.splice(index, 1);
      saveCart();
    }

    function renderCart() {
      const cartItems = document.getElementById("cartItems");
      const totalPrice = document.getElementById("totalPrice");
      cartItems.innerHTML = "";
      let total = 0;
      cart.forEach((item, index) => {
        total += item.price * item.quantity;
        cartItems.innerHTML += `
          <li>
            ${item.name} - ${item.price.toLocaleString()}₫ x 
            <input type="number" min="1" value="${item.quantity}" onchange="updateQuantity(${index}, this.value)">
            <button onclick="removeFromCart(${index})">Xóa</button>
          </li>`;
      });
      totalPrice.innerText = "Tổng: " + total.toLocaleString() + "₫";
    }

    function updateQuantity(index, value) {
      cart[index].quantity = parseInt(value);
      saveCart();
    }

    renderCart();
  </script>
</body>
</html>
```

---

## 📚 Tổng kết

Dự án giúp sinh viên hiểu quy trình phát triển phần mềm từ **phân tích – thiết kế – lập trình – kiểm thử – triển khai**, đồng thời áp dụng **Agile (Scrum)** trong quản lý tiến độ với **Jira** và **GitHub**.

---

✉️ **Liên hệ:** Nguyễn Trần Quốc Tuấn – PTIT HCM
🔗 **Jira:** [student-team-tjrvokrv.atlassian.net](https://student-team-tjrvokrv.atlassian.net/jira/software/projects/SC/boards/34/backlog)
💾 **GitHub:** [n23dcpt112-ai.github.io](https://github.com/n23dcpt112-ai/n23dcpt112-ai.github.io)

```
