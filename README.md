
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Меню с 10 категориями и модалкой</title>
<style>
  body { font-family: Arial, sans-serif; margin: 0; padding: 0; background: #f5f5f5; }
  .sidebar { width: 200px; background: #fff; padding: 20px; float: left; height: 100vh; box-shadow: 2px 0 5px rgba(0,0,0,0.1); }
  .sidebar ul { list-style: none; padding: 0; }
  .sidebar li { padding: 8px; cursor: pointer; transition: background 0.3s; }
  .sidebar li:hover { background: #ddd; }
  .sidebar li.active { background: #bbb; font-weight: bold; }
  .content { margin-left: 220px; padding: 20px; display: flex; flex-wrap: wrap; gap: 15px; }
  .product { width: 200px; background: #fff; padding: 15px; border-radius: 8px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); cursor: pointer; }
  .product img { width: 100%; border-radius: 8px; }
  /* Модал */
  .modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); }
  .modal-content { background-color: #fff; margin: 5% auto; padding: 20px; border-radius: 10px; width: 500px; position: relative; text-align: center; }
  .modal-content img { width: 100%; border-radius: 10px; }
  .close { position: absolute; top: 15px; right: 20px; font-size: 24px; cursor: pointer; }
  .counter { margin-top: 15px; }
  .counter button { padding: 5px 10px; font-size: 16px; }
  .cart-button { background-color: #28a745; color: white; border: none; padding: 10px 20px; margin-top: 15px; border-radius: 5px; font-size: 16px; cursor: pointer; }
  .cart-button:hover { background-color: #218838; }
  .modal-content img {
  width: 100%;
  height: 300px; /* фиксированная высота для всех */
  object-fit: cover; /* обрезка по центру */
  border-radius: 10px;
}

</style>
</head>
<body>

<div class="sidebar">
  <h3>Категории</h3>
  <ul id="categoryList"></ul>
</div>

<div class="content" id="productContainer"></div>

<!-- Модал -->
<div id="modal" class="modal">
  <div class="modal-content">
    <span class="close" id="closeModal">&times;</span>
    <img id="modalImg" src="" alt="">
    <h2 id="modalName"></h2>
    <p id="modalPrice"></p>
    <div class="counter">
      <button onclick="changeQuantity(-1)">-</button>
      <span id="quantity">1</span>
      <button onclick="changeQuantity(1)">+</button>
    </div>
    <button class="cart-button">В корзину</button>
  </div>
</div>

<script>
// Данные категорий и продуктов
// Данные категорий и продуктов
const categories = [
  "Фильтр", "Супы", "Напитки", "Десерты", "Пицца", 
  "Бургеры", "Салаты", "Гарниры", "Закуски", "Соусы"
];

const productsData = {};

// Категория "Фильтр" с конкретными товарами
productsData["Фильтр"] = [
  { name: "83076 Фильтр салона BYD с углем  BYD Song Plus (83076)", price: "20 320 сум", img: "https://i.postimg.cc/44DYZ0wT/temp-Image-Mjy-Bn-J.avif$0" },
  { name: "84102 Фильтр салона с углем YUAN PLUS (84102)", price: "21 971 сум", img: "https://i.postimg.cc/ZnxCK8Th/temp-Imageh-BXRZO.avif$0" },
  { name: "89096 Фильтр салона BYD CHAZOR (С УГЛЕМ) (89096)", price: "21 844 сум", img: "https://i.postimg.cc/C1R3xm73/temp-Imagebx-K9-S1.avif$0" },
  { name: "87002 Фильтр салона BYD SEAGULL EV (87002)", price: "20 955 сум", img: "https://i.postimg.cc/3RhtP16j/temp-Image-Yc-Nsr-Y.avif$0" },
  { name: "83214 Фильтр салона BYD SONG PRO EV (83214)", price: "22 606 сум", img: "https://i.postimg.cc/HLrmrYsv/temp-Image6-Be6v-Z.avif$0" },
  { name: "95031 Фильтр салона с углем BYD E2 (95031)", price: "20 574 сум", img: "https://i.postimg.cc/PJ4QskZT/temp-Image-Q9txm-V.avif$0" },
  { name: "81014 Фильтр салона Уголь BYD HAN №1 (81014)", price: "20 574 сум", img: "https://i.postimg.cc/kg7BZbXr/temp-Image-YZ7-Wm-E.avif$0" },
  { name: "Фильтр салона с углем YUAN UP (84127)", price: "21 590 сум", img: "https://i.postimg.cc/yY9z919k/temp-Imagec2895-D.avif$0" }
];


// Остальные категории заполняем как раньше
categories.forEach(cat => {
  if (cat !== "Фильтр") {
    productsData[cat] = [];
    for (let i = 1; i <= 10; i++) {
      productsData[cat].push({
        name: `${cat} продукт ${i}`,
        price: `${(Math.floor(Math.random() * 90) + 10) * 1000} сум`,
        img: `https://via.placeholder.com/200x150.png?text=${encodeURIComponent(cat + " " + i)}`
      });
    }
  }
});


// Рендер категорий
const categoryList = document.getElementById("categoryList");
categories.forEach((cat, index) => {
  const li = document.createElement("li");
  li.textContent = cat;
  if (index === 0) li.classList.add("active");
  li.onclick = () => showCategory(cat, li);
  categoryList.appendChild(li);
});

// Рендер продуктов
function showCategory(categoryName, element) {
  document.querySelectorAll(".sidebar li").forEach(li => li.classList.remove("active"));
  element.classList.add("active");

  const productContainer = document.getElementById("productContainer");
  productContainer.innerHTML = "";
  productsData[categoryName].forEach(prod => {
    const div = document.createElement("div");
    div.classList.add("product");
    div.dataset.name = prod.name;
    div.dataset.price = prod.price;
    div.dataset.img = prod.img;
    div.innerHTML = `
      <img src="${prod.img}" alt="${prod.name}">
      <h3>${prod.name}</h3>
      <p>${prod.price}</p>
    `;
    div.onclick = () => openModal(prod);
    productContainer.appendChild(div);
  });
}

// Модалка
const modal = document.getElementById("modal");
const closeModal = document.getElementById("closeModal");
const quantityDisplay = document.getElementById("quantity");
let quantity = 1;

function openModal(product) {
  document.getElementById("modalImg").src = product.img;
  document.getElementById("modalName").textContent = product.name;
  document.getElementById("modalPrice").textContent = product.price;
  quantity = 1;
  quantityDisplay.textContent = quantity;
  modal.style.display = "block";
}

closeModal.onclick = () => modal.style.display = "none";
window.onclick = (event) => { if (event.target === modal) modal.style.display = "none"; };

function changeQuantity(delta) {
  quantity = Math.max(1, quantity + delta);
  quantityDisplay.textContent = quantity;
}

// Показ первой категории по умолчанию
showCategory(categories[0], categoryList.firstChild);
</script>

</body>
</html>


</script>

</body>
</html>

