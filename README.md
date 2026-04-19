<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<title>مخزن الإنتاج البحري</title>

<style>
body { font-family:Tahoma; background:#f5f5f5; padding:20px; }
input { width:100%; padding:10px; margin-bottom:15px; }
.card {
  background:#fff; padding:10px; margin:10px;
  display:inline-block; width:220px;
  border-radius:10px; box-shadow:0 0 5px #ccc;
}
.card img {
  width:100%; height:120px;
  object-fit:cover; cursor:pointer;
}
</style>

</head>

<body>

<h2>📦 مخزن الإنتاج البحري</h2>

<input type="text" id="search" placeholder="ابحث بالاسم أو اللوكيشن">

<div id="products">جاري التحميل...</div>

<script>

const API = "https://script.google.com/macros/s/AKfycbyh0czRkVK5fjUoDyWAA0JDi6V2hRBEH4bFlsl3n2GuLTiHYOVZoE1fZYpqVMBKRxt7/exec";

let products = [];

// 🔥 تنظيف النص
function normalize(text){
  return (text || "")
    .replace(/[أإا]/g, "ا")
    .replace(/ة/g, "ه")
    .replace(/ى/g, "ي")
    .toLowerCase();
}

// 🔥 جلب البيانات
fetch(API)
.then(res => res.json())
.then(data => {
  products = data;
  render(products);
})
.catch(err=>{
  document.getElementById("products").innerHTML="❌ في مشكلة في تحميل البيانات";
});

// 🔥 عرض
function render(list){
  let html = "";

  list.forEach((p,i)=>{
    html += `
    <div class="card">
      <img src="${p.image}" onclick="openDetails(${i})">
      <h4>${p.name}</h4>
      <p>📍 ${p.location}</p>
      <p>🔢 ${p.code}</p>
      <p>📦 ${p.qty}</p>
    </div>`;
  });

  document.getElementById("products").innerHTML = html;
}

// 🔥 تفاصيل
function openDetails(i){
  localStorage.setItem("product", JSON.stringify(products[i]));
  window.location.href = "details.html";
}

// 🔥 سيرش ذكي
document.getElementById("search").addEventListener("input", function(){
  let value = normalize(this.value);

  let filtered = products.filter(p =>
    normalize(p.name).includes(value) ||
    normalize(p.location).includes(value)
  );

  render(filtered);
});

</script>

</body>
</html>
