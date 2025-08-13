
<html lang="ru">
<head>
  title: "Гарант BYD"
description: "Официальный дилер BYD в Ташкенте"
<meta charset="UTF-8" />
<title>Меню — Фильтр</title>
<style>
  body { font-family: Arial, sans-serif; margin:0; padding:0; background:#f5f5f5; min-height:100vh; display:flex; flex-direction:column; }
  .main { display:flex; flex:1; }
  .sidebar { width:220px; background:#fff; padding:20px; box-shadow:2px 0 5px rgba(0,0,0,0.1); }
  .sidebar ul { list-style:none; padding:0; margin:0; }
  .sidebar li { padding:8px; cursor:pointer; transition:background .2s; border-radius:4px; }
  .sidebar li:hover { background:#eee; }
  .sidebar li.active { background:#d0d0d0; font-weight:700; }
  .content { flex:1; padding:20px; display:flex; flex-wrap:wrap; gap:15px; align-content:flex-start; }
  .product { width:200px; background:#fff; padding:12px; border-radius:8px; box-shadow:0 2px 5px rgba(0,0,0,0.08); cursor:pointer; display:flex; flex-direction:column; gap:8px; }
  .product img { width:100%; height:120px; object-fit:cover; border-radius:6px; display:block; }
  .product h3 { font-size:14px; margin:0; }
  .product p { margin:0; color:#333; font-weight:600; }

  /* Модал */
  .modal { display:none; position:fixed; z-index:1000; left:0; top:0; width:100%; height:100%; background:rgba(0,0,0,0.5); align-items:center; justify-content:center; }
  .modal.open { display:flex; }
  .modal-content { background:#fff; width:520px; max-width:95%; border-radius:10px; padding:18px; position:relative; text-align:center; }
  .modal-content img { width:100%; height:300px; object-fit:cover; border-radius:8px; }
  .close { position:absolute; right:12px; top:8px; font-size:22px; cursor:pointer; }

  .contacts { 
    background:#fff; 
    padding:12px 20px; 
    text-align:center; 
    border-top:1px solid #e0e0e0; 
    font-size:20px; 
    font-weight:bold;
  }
</style>
</head>
<body>

<div class="main">
  <aside class="sidebar">
    <h3>Категории</h3>
    <ul id="categoryList"></ul>
  </aside>

  <main class="content" id="productContainer"></main>
</div>

<!-- Модал -->
<div id="modal" class="modal" role="dialog" aria-modal="true">
  <div class="modal-content">
    <span class="close" id="closeModal">&times;</span>
    <img id="modalImg" src="" alt="">
    <h2 id="modalName"></h2>
    <p id="modalPrice"></p>
  </div>
</div>

<div class="contacts">
  Для покупки товаров контакты:<br>
  +998 90 999 99 94<br>
  +998 97 187 44 40
</div>

<script>
/* Данные */
const categories = ["Фильтр"];

const productsData = {
  "Фильтр": [
    { name: "83076 Фильтр салона BYD с углем  BYD Song Plus (83076)", price: "20 320 сум", img: "https://i.postimg.cc/44DYZ0wT/temp-Image-Mjy-Bn-J.avif$0" },
    { name: "84102 Фильтр салона с углем YUAN PLUS (84102)", price: "21 971 сум", img: "https://i.postimg.cc/ZnxCK8Th/temp-Imageh-BXRZO.avif$0" },
    { name: "89096 Фильтр салона BYD CHAZOR (С УГЛЕМ) (89096)", price: "21 844 сум", img: "https://i.postimg.cc/C1R3xm73/temp-Imagebx-K9-S1.avif$0" },
    { name: "87002 Фильтр салона BYD SEAGULL EV (87002)", price: "20 955 сум", img: "https://i.postimg.cc/3RhtP16j/temp-Image-Yc-Nsr-Y.avif$0" },
    { name: "83214 Фильтр салона BYD SONG PRO EV (83214)", price: "22 606 сум", img: "https://i.postimg.cc/HLrmrYsv/temp-Image6-Be6v-Z.avif$0" },
    { name: "95031 Фильтр салона с углем BYD E2 (95031)", price: "20 574 сум", img: "https://i.postimg.cc/PJ4QskZT/temp-Image-Q9txm-V.avif$0" },
    { name: "81014 Фильтр салона Уголь BYD HAN №1 (81014)", price: "20 574 сум", img: "https://i.postimg.cc/kg7BZbXr/temp-Image-YZ7-Wm-E.avif$0" },
    { name: "Фильтр салона с углем YUAN UP (84127)", price: "21 590 сум", img: "https://i.postimg.cc/yY9z919k/temp-Imagec2895-D.avif$0" }
  ]
};

/* Рендер списка категорий */
const categoryList = document.getElementById('categoryList');
categories.forEach((cat, idx) => {
  const li = document.createElement('li');
  li.textContent = cat;
  if (idx === 0) li.classList.add('active');
  li.addEventListener('click', () => showCategory(cat, li));
  categoryList.appendChild(li);
});

/* Показ категории */
function showCategory(categoryName, element) {
  document.querySelectorAll('#categoryList li').forEach(li => li.classList.remove('active'));
  if (element) element.classList.add('active');

  const productContainer = document.getElementById('productContainer');
  productContainer.innerHTML = '';

  const items = productsData[categoryName] || [];
  if (!items.length) {
    productContainer.innerHTML = '<p>Товары в этой категории отсутствуют.</p>';
    return;
  }

  items.forEach(prod => {
    const div = document.createElement('div');
    div.className = 'product';
    div.innerHTML = `
      <img src="${prod.img}" alt="${prod.name}"
           onerror="this.onerror=null; this.src='https://via.placeholder.com/200x150?text=Нет+изображения'">
      <h3>${prod.name}</h3>
      <p>${prod.price}</p>
    `;
    div.addEventListener('click', () => openModal(prod));
    productContainer.appendChild(div);
  });
}

/* Модал */
const modal = document.getElementById('modal');
const closeModal = document.getElementById('closeModal');

function openModal(product) {
  document.getElementById('modalImg').src = product.img;
  document.getElementById('modalName').textContent = product.name;
  document.getElementById('modalPrice').textContent = product.price;
  modal.classList.add('open');
}
closeModal.addEventListener('click', () => modal.classList.remove('open'));
modal.addEventListener('click', (e) => { if (e.target === modal) modal.classList.remove('open'); });

/* Старт */
const firstLi = categoryList.firstElementChild;
if (firstLi) {
  showCategory(categories[0], firstLi);
}
</script>

</body>
</html>
