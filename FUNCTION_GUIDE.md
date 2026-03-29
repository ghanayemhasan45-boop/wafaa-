# 📖 شرح الوظائف التفصيلي - Mira Gold

## 🎯 الوظائف الأساسية (Core Functions)

### 1. `init()` - تهيئة التطبيق
```javascript
function init() {
  renderFilters();      // عرض أزرار الفلاتر
  renderGrid();         // عرض المنتجات
  setupEventListeners();// تفعيل الأحداث (بحث، ترتيب)
}
```
**متى تشتغل؟** عند فتح الموقع (آخر سطر في script.js)

---

### 2. `renderFilters()` - عرض أزرار التصفية
```javascript
// تنشئ HTML للأزرار التالية:
// [الكل] [أطقم] [سلاسل] [خواتم] [أساور] [أقراط]
// الزر المختار يكون (ذهبي + أبيض)
// الأزرار الأخرى (أبيض + رمادي)
```

**الكود:**
```javascript
filtersEl.innerHTML = categories.map(cat => `
  <button onclick="setFilter('${cat.id}')" 
    class="... ${
      state.currentFilter === cat.id 
      ? 'bg-gold-500 text-white'  // مختار
      : 'bg-white text-stone-600' // غير مختار
    }">
    ${cat.label}
  </button>
`).join('');
```

---

### 3. `renderGrid()` - عرض قائمة المنتجات
```javascript
// الخطوات:
// 1️⃣ تصفية المنتجات:
//    - حسب الفئة (حالياً current filter)
//    - وفقاً لنص البحث
// 2️⃣ ترتيب المنتجات:
//    - الأحدث (الافتراضي)
//    - الأقل سعراً
//    - الأعلى سعراً
// 3️⃣ عرض النتائج:
//    - بطاقات جميلة تتحتوي على:
//      * الصورة (قابلة للتكبير)
//      * الاسم والوصف
//      * الفئة (شارة علوية)
//      * السعر وزر الإضافة
```

**مثال على النتيجة:**
```
┌─────────────────────┐
│   [صورة المنتج]      │
│   طقم الزهرة الذهبية │
│   450 ج.م           │
│  [زر الإضافة +]      │
└─────────────────────┘
```

---

### 4. `setFilter(id)` - تغيير الفئة المختارة
```javascript
function setFilter(id) {
  state.currentFilter = id;  // تحديث الفئة
  renderFilters();           // تحديث عرض الأزرار
  renderGrid();              // تحديث قائمة المنتجات
}
```

**مثال:**
```
المستخدم يضغط → "أطقم"
setFilter('sets') يشتغل
↓
يعرض فقط: { category: 'sets' }
```

---

### 5. `setupEventListeners()` - ربط الأحداث
```javascript
// ربط حدث "input" على حقل البحث
searchInput.addEventListener('input', (e) => {
  state.searchQuery = e.target.value;
  // عند الكتابة: "ذهب" → فلترة فقط المنتجات اللي فيها "ذهب"
  renderGrid();
});

// ربط حدث "change" على قائمة الترتيب
sortSelect.addEventListener('change', (e) => {
  state.sortMethod = e.target.value;
  // عند تغيير الترتيب: معاد ترتيب المنتجات
  renderGrid();
});
```

---

## 🛒 وظائف السلة (Cart Functions)

### 6. `addToCart(id)` - إضافة منتج للسلة
```javascript
function addToCart(id) {
  const product = products.find(p => p.id === id);  // البحث عن المنتج
  const existing = state.cart.find(item => item.id === id); // هل موجود؟
  
  if (existing) {
    existing.qty++;  // إذا موجود: زيادة الكمية
  } else {
    state.cart.push({ ...product, qty: 1 }); // إلا: إضافة جديد
  }
  
  updateCartUI(); // تحديث السلة
  showToast(`تمت إضافة "${product.name}" للسلة`); // إشعار
  toggleCart(true); // فتح درج السلة
}
```

**مثال:**
```
قبل: state.cart = []
بعد الضغط على "طقم الزهرة": 
state.cart = [
  { id: 1, name: "طقم الزهرة", price: 450, qty: 1 }
]
الضغط مرة ثانية:
state.cart = [
  { id: 1, name: "طقم الزهرة", price: 450, qty: 2 }
]
```

---

### 7. `updateCartQty(id, change)` - تعديل الكمية
```javascript
function updateCartQty(id, change) {
  const item = state.cart.find(item => item.id === id);
  if (item) {
    item.qty += change;  // +1 أو -1
    if (item.qty <= 0) {
      state.cart = state.cart.filter(i => i.id !== id); // حذف إذا صفر
    }
  }
  updateCartUI();
}
```

**أزرار في السلة:**
```
[-] [1] [+]  ← الأزرار لتعديل الكمية
```

---

### 8. `updateCartUI()` - تحديث واجهة السلة
```javascript
function updateCartUI() {
  // 1️⃣ عرض المنتجات في السلة (أو "السلة فارغة")
  cartItemsEl.innerHTML = ...
  
  // 2️⃣ حساب الإجمالي
  const total = state.cart.reduce((sum, item) => 
    sum + (item.price * item.qty), 0
  );
  cartTotalEl.innerText = total + " ج.م";
  
  // 3️⃣ تحديث الشارة (الرقم على الأيقونة)
  const count = state.cart.reduce((c, item) => c + item.qty, 0);
  cartBadge.innerText = count;
  
  // 4️⃣ إظهار/إخفاء الشارة
  if (count > 0) {
    cartBadge.classList.remove('scale-0'); // عرض
  } else {
    cartBadge.classList.add('scale-0');    // إخفاء
  }
}
```

---

### 9. `toggleCart(forceOpen)` - فتح/إغلاق درج السلة
```javascript
function toggleCart(forceOpen = false) {
  const isHidden = cartDrawer.classList.contains('-translate-x-full');
  
  if (isHidden || forceOpen) {
    // فتح السلة (انزلاق من اليمين)
    cartDrawer.classList.remove('-translate-x-full');
    cartOverlay.classList.remove('hidden');
  } else {
    // إغلاق السلة (انزلاق نحو اليمين)
    cartDrawer.classList.add('-translate-x-full');
    cartOverlay.classList.add('opacity-0');
  }
}
```

---

### 10. `removeItem(id)` - حذف منتج من السلة
```javascript
function removeItem(id) {
  state.cart = state.cart.filter(i => i.id !== id);
  updateCartUI();
}
```

---

## 💳 وظائف الدفع (Checkout Functions)

### 11. `checkout()` - إتمام الطلب عبر WhatsApp
```javascript
function checkout() {
  if (state.cart.length === 0) return; // إذا السلة فارغة
  
  const phone = "201104436931"; // رقم WhatsApp
  let msg = "مرحباً ميرا جولد، أود طلب المنتجات التالية:\n\n";
  
  // بناء الرسالة بصيغة:
  // - طقم الزهرة الذهبية (2 قطعة) - 900 ج.م
  // - عقد اللؤلؤ (1 قطعة) - 180 ج.م
  state.cart.forEach(item => {
    msg += `- ${item.name} (${item.qty} قطعة) - ${item.price * item.qty} ج.م\n`;
  });
  
  msg += `\nالإجمالي: ${cartTotalEl.innerText}`;
  
  // فتح WhatsApp بالرسالة
  window.open(`https://wa.me/${phone}?text=${encodeURIComponent(msg)}`, '_blank');
}
```

**النتيجة:**
```
يفتح WhatsApp برسالة:
"مرحباً ميرا جولد، أود طلب المنتجات التالية:

- طقم الزهرة الذهبية (2 قطعة) - 900 ج.م
- عقد اللؤلؤ (1 قطعة) - 180 ج.م

الإجمالي: 1080 ج.م"
```

---

## 🔔 وظائف الإشعارات (Notification Functions)

### 12. `showToast(msg)` - إظهار إشعار مؤقت
```javascript
function showToast(msg) {
  const toast = document.getElementById('toast');
  document.getElementById('toast-message').innerText = msg;
  
  // عرض الإشعار
  toast.classList.remove('opacity-0', 'translate-y-20');
  
  // إخفاء بعد 3 ثواني
  setTimeout(() => {
    toast.classList.add('opacity-0', 'translate-y-20');
  }, 3000);
}
```

**مثال:**
```
┌─────────────────────────────────┐
│ ✓ تمت إضافة "طقم الزهرة" للسلة  │
└─────────────────────────────────┘
(يختفي بعد 3 ثواني)
```

---

## 🖼️ وظائف الصور (Image Functions)

### 13. `openImageModal(src, alt)` - تكبير الصورة
```javascript
function openImageModal(src, alt) {
  const modal = document.getElementById('image-modal');
  const img = document.getElementById('modal-img');
  
  img.src = src;   // تعيين الصورة
  img.alt = alt;   // النص البديل
  
  modal.classList.remove('hidden');  // إظهار
  
  // إضافة تأثير الظهور (fade-in + zoom-in)
  setTimeout(() => {
    modal.classList.remove('opacity-0');
    img.classList.add('scale-100');
  }, 10);
}
```

---

### 14. `closeImageModal()` - إغلاق الصورة المكبرة
```javascript
function closeImageModal() {
  const modal = document.getElementById('image-modal');
  const img = document.getElementById('modal-img');
  
  // إضافة تأثير الاختفاء (fade-out + zoom-out)
  modal.classList.add('opacity-0');
  img.classList.remove('scale-100');
  img.classList.add('scale-95');
  
  // إخفاء بعد التأثير
  setTimeout(() => {
    modal.classList.add('hidden');
    img.src = '';
  }, 300);
}
```

---

## 📊 وظائف الرسوم البيانية (Dashboard Functions)

### 15. `toggleDashboard()` - عرض/إخفاء التحليلات
```javascript
function toggleDashboard() {
  // تبديل (toggle) إظهار/إخفاء قسم التحليلات
  dashboardSection.classList.toggle('hidden');
  
  // إذا كان الأول مرة: إنشاء الرسوم البيانية
  if (!dashboardSection.classList.contains('hidden') && !state.chartsInitialized) {
    initCharts();
    state.chartsInitialized = true;
  }
}
```

---

### 16. `initCharts()` - إنشاء الرسوم البيانية
```javascript
function initCharts() {
  // 1️⃣ حساب عدد المنتجات لكل فئة
  const cats = {};
  products.forEach(p => {
    cats[p.categoryLabel] = (cats[p.categoryLabel] || 0) + 1;
  });
  // النتيجة: { أطقم: 25, سلاسل: 20, خواتم: 30, ... }
  
  // 2️⃣ حساب متوسط السعر لكل فئة
  const prices = {};
  products.forEach(p => {
    if (!prices[p.categoryLabel]) 
      prices[p.categoryLabel] = { sum: 0, count: 0 };
    prices[p.categoryLabel].sum += p.price;
    prices[p.categoryLabel].count += 1;
  });
  const avgPrices = labels.map(l => 
    Math.round(prices[l].sum / prices[l].count)
  );
  
  // 3️⃣ رسم Chart.js (Doughnut)
  new Chart(document.getElementById('categoryChart').getContext('2d'), {
    type: 'doughnut',
    data: {
      labels: ['أطقم', 'سلاسل', 'خواتم', 'أساور', 'أقراط'],
      datasets: [{
        data: [25, 20, 30, 18, 22],
        backgroundColor: ['#f59e0b', '#d97706', ...] // ألوان ذهبية
      }]
    }
  });
  
  // 4️⃣ رسم Chart.js (Bar)
  new Chart(document.getElementById('priceChart').getContext('2d'), {
    type: 'bar',
    data: {
      labels: ['أطقم', 'سلاسل', 'خواتم', 'أساور', 'أقراط'],
      datasets: [{
        label: 'متوسط السعر',
        data: [850, 200, 250, 250, 200],
        backgroundColor: '#f59e0b' // لون ذهبي
      }]
    }
  });
}
```

---

## 🔍 وظائف إضافية (Helper Functions)

### 17. `resetFilters()` - مسح جميع الفلاتر
```javascript
function resetFilters() {
  state.currentFilter = 'all';      // الفئة: الكل
  state.searchQuery = '';            // البحث: فارغ
  searchInput.value = '';            // تفريغ حقل البحث
  renderFilters();                   // تحديث الأزرار
  renderGrid();                      // عرض جميع المنتجات
}
```

---

## 📈 مسار البيانات (Data Flow)

```
المستخدم يفتح الموقع
        ↓
   init() تشتغل
        ↓
render Filters + render Grid
        ↓
المستخدم يبحث / يصفي
        ↓
renderGrid() مرة ثانية (تصفية جديدة)
        ↓
المستخدم يضيف للسلة
        ↓
addToCart() تضيف للـ state.cart
        ↓
updateCartUI() تحدث الواجهة
        ↓
المستخدم يشتري
        ↓
checkout() فتح WhatsApp 📱
```

---

## 💡 ملاحظات مهمة

1. **state** هو "قلب البرنامج" - يحفظ كل المعلومات المتغيرة
2. **renderGrid** و **renderFilters** هي "الدوال الرئيسية" - تُعيد رسم الواجهة
3. **القائمة مرتبطة بـ products[]** - لتغيير الظهور غيّر البيانات هناك
4. **Tailwind CSS** تتحكم بالتصميم - لتغيير الألوان غيّر الـ classes

---

**تم إعداده بـ ❤️ لفهم أعمق للمشروع**
