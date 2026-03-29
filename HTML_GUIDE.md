# 📖 شرح ملف HTML - Mira Gold

## 🏗️ البنية الكاملة

```
<html>
  <head>
    ├── Meta Tags (معلومات الموقع)
    └── External Resources (المكتبات الخارجية)
  <body>
    ├── <header> (رأس الموقع)
    ├── <main> (المحتوى الرئيسي)
    │   ├── Hero Section (صورة البطل)
    │   ├── Dashboard (التحليلات)
    │   ├── Filters (أزرار التصفية)
    │   ├── Search Bar (حقل البحث)
    │   ├── Products Grid (قائمة المنتجات)
    │   └── Empty State (رسالة لا توجد نتائج)
    ├── Cart Drawer (السلة الجانبية)
    ├── Image Modal (نافذة تكبير الصور)
    └── Toast (الإشعارات)
```

---

## 📝 شرح كل قسم

### 1️⃣ **Head - قسم الرأس**

```html
<head>
  <meta charset="UTF-8">
  <!-- تحديد ترميز الحروف (UTF-8) للعربية ✓ -->
  
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- جعل الموقع متوافق مع الهواتف الذكية -->
  
  <title>ميرا جولد | التشكيلة الضخمة</title>
  <!-- العنوان الذي يظهر في تبويب المتصفح -->
```

---

### 2️⃣ **المكتبات الخارجية**

```html
<!-- Tailwind CSS CDN -->
<script src="https://cdn.tailwindcss.com"></script>
<!-- مكتبة تصميم قوية (آلاف الـ classes جاهزة) -->

<!-- Chart.js - رسوم بيانية -->
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<!-- تنشئ رسوم بيانية جميلة (Doughnut + Bar Charts) -->

<!-- Google Fonts - خط Cairo -->
<link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap">
<!-- خط عربي جميل (وزن 300 إلى 700) -->

<!-- CSS مخصص -->
<link rel="stylesheet" href="style.css">
<!-- ملف CSS خاص لإضافات مخصصة -->
```

---

### 3️⃣ **Body - الجسم**

#### 🔝 **Header (الرأس الثابت)**

```html
<header class="sticky top-0 z-50 glass-panel">
<!-- 
  sticky: يبقى في الأعلى عند التمرير
  z-50: فوق باقي العناصر
  glass-panel: تأثير زجاجي من style.css
-->

  <!-- الجزء الأيسر: الشعار -->
  <div class="flex items-center gap-3">
    <div class="w-10 h-10 rounded-full bg-gradient-to-br from-gold-300 to-gold-600">
      M <!-- حرف M بخلفية ذهبية درجات -->
    </div>
    <div>
      <h1>ميرا جولد</h1>        <!-- اسم المتجر -->
      <p>أكبر تشكيلة مجوهرات</p> <!-- الوصف -->
    </div>
  </div>

  <!-- الجزء الأيمن: الأيقونات -->
  <!-- زر التحليلات 📊 -->
  <button onclick="toggleDashboard()" class="hidden md:flex">
    📊 تحليلات
  </button>
  <!-- مخفي على الموبايل (hidden)، يظهر على Tablet فما فوق (md:) -->

  <!-- أيقونة السلة 🛍️ -->
  <div class="relative cursor-pointer" onclick="toggleCart()">
    <span>🛍️</span>
    <!-- شارة الكمية (الرقم الأحمر) -->
    <span id="cart-badge" class="absolute -top-1 -right-1 bg-red-500">
      0 <!-- عدد المنتجات -->
    </span>
  </div>
</header>
```

---

#### 🎨 **Hero Section (قسم البطل)**

```html
<section class="bg-stone-900 text-white py-16">
<!-- خلفية سوداء داكنة -->

  <!-- خلفية متدرجة -->
  <div class="absolute inset-0 opacity-20">
    <div class="bg-[radial-gradient(...)]">
      <!-- تدرج شعاعي من الذهب إلى الأسود -->
    </div>
  </div>

  <!-- النص -->
  <div class="relative z-10 text-center">
    <h2>فخامة لا تضاهى</h2>
    <p>استكشفي أكثر من قطعة مميزة...</p>
  </div>
</section>
```

---

#### 📊 **Dashboard Section (الرسوم البيانية)**

```html
<section id="dashboard-section" class="hidden">
<!-- مخفية في البداية (hidden) -->

  <div class="grid grid-cols-1 md:grid-cols-2">
    <!-- عمود واحد على الموبايل، عمودين على الشاشات الكبيرة -->
    
    <!-- الرسم الأول: توزيع المنتجات -->
    <div>
      <h4>توزيع المنتجات</h4>
      <canvas id="categoryChart"></canvas>
      <!-- Canvas = لوحة للرسم (Chart.js) -->
    </div>

    <!-- الرسم الثاني: متوسط الأسعار -->
    <div>
      <h4>متوسط الأسعار</h4>
      <canvas id="priceChart"></canvas>
    </div>
  </div>
</section>
```

---

#### 🔍 **Filters Section (أزرار التصفية)**

```html
<section class="max-w-7xl mx-auto px-4 py-12">
<!-- محتوى محدود العرض (7xl)، padding من الجوانب -->

  <div class="sticky top-24 z-30 bg-stone-50/95 backdrop-blur-sm">
  <!-- 
    sticky: يبقى تحت الـ header عند التمرير
    top-24: مسافة من الأعلى
    backdrop-blur: تأثير زجاجي
  -->

    <!-- أزرار الفلاتر -->
    <div id="category-filters">
      <!-- تُملأ بـ JavaScript مثل: -->
      <!-- <button onclick="setFilter('all')">الكل</button> -->
      <!-- <button onclick="setFilter('sets')">أطقم</button> -->
      <!-- ... وهكذا -->
    </div>

    <!-- البحث والترتيب -->
    <div class="flex gap-4">
      <!-- حقل البحث -->
      <input id="search-input" type="text" placeholder="بحث...">
      <!-- عند الكتابة: تشتغل دالة البحث -->

      <!-- علبة الترتيب -->
      <select id="sort-select">
        <option value="default">الأحدث</option>
        <option value="price-asc">الأقل سعراً</option>
        <option value="price-desc">الأعلى سعراً</option>
      </select>
      <!-- عند التغيير: تُنَفَّذ دالة الترتيب -->
    </div>
  </div>

  <!-- عدد المنتجات -->
  <div class="flex justify-between">
    <span>عرض 
      <span id="showing-count">24</span> 
      قطعة
    </span>
  </div>
</section>
```

---

#### 🏪 **Products Grid (شبكة المنتجات)**

```html
<section class="max-w-7xl mx-auto px-4 py-12">

  <!-- شبكة المنتجات -->
  <div id="products-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
  <!-- 
    grid-cols-1: موبايل = عمود واحد
    sm:grid-cols-2: جوال كبير = عمودين
    lg:grid-cols-4: ويب = 4 أعمدة
  -->
    
    <!-- كل بطاقة منتج تحتوي على: -->
    <div class="bg-white rounded-2xl overflow-hidden border shadow-sm card-hover">
      
      <!-- الصورة (قابلة للتكبير) -->
      <div class="relative h-64 overflow-hidden bg-stone-100 cursor-pointer">
        <img src="..." alt="..." class="w-full h-full object-cover group-hover:scale-110">
        <!-- عند التمرير: الصورة تكبر قليلاً -->
        
        <!-- شارة الفئة (أعلى يسار) -->
        <span class="absolute top-3 right-3 bg-white/90">أطقم</span>
        
        <!-- أيقونة التكبير (تظهر عند التمرير) -->
        <div class="absolute inset-0 opacity-0 group-hover:opacity-100">
          <span>🔍 تكبير</span>
        </div>
      </div>

      <!-- معلومات المنتج -->
      <div class="p-5 flex-1 flex flex-col">
        <h3 class="font-bold text-lg">طقم الزهرة الذهبية</h3>
        <p class="text-sm text-stone-500">طقم كامل مطلي بالذهب عيار 18</p>
        
        <!-- السعر وزر الإضافة -->
        <div class="mt-auto pt-4 border-t flex justify-between">
          <span class="text-xl font-bold">450 <span class="text-sm">ج.م</span></span>
          <button onclick="addToCart(1)" class="bg-stone-900 text-white p-3 rounded-xl">
            +
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- رسالة "لا توجد نتائج" -->
  <div id="empty-state" class="hidden text-center py-20">
    <!-- تظهر فقط عندما تكون النتائج صفر -->
    <p>لم نجد منتجات تطابق بحثك 😢</p>
  </div>
</section>
```

---

### 4️⃣ **Cart Drawer (درج السلة)**

```html
<!-- الغطاء (خلفية داكنة) -->
<div id="cart-overlay" class="fixed inset-0 bg-black/50 hidden z-40" 
     onclick="toggleCart()">
</div>

<!-- الدرج الجانبي -->
<div id="cart-drawer" class="fixed top-0 right-0 h-full w-80 bg-white shadow-2xl 
     transform -translate-x-full transition-transform duration-300 z-50">
<!-- 
  fixed: ثابت على الشاشة
  top-0 right-0: الزاوية العلوية اليمين
  -translate-x-full: خارج الشاشة (مخفي)
  عند الفتح: يصبح translate-x-0 (يظهر)
-->

  <!-- رأس الدرج -->
  <div class="p-4 border-b flex justify-between items-center">
    <h3 class="font-bold">سلة التسوق</h3>
    <button onclick="toggleCart()" class="text-2xl">✕</button>
  </div>

  <!-- محتويات السلة -->
  <div id="cart-items" class="flex-1 overflow-y-auto p-4">
    <!-- كل منتج في السلة يظهر هكذا: -->
    <!-- 
      <div>
        <img src="..." alt="">
        <h4>اسم المنتج</h4>
        <p>السعر × الكمية</p>
        <button>-</button> [الكمية] <button>+</button>
        <button>حذف</button>
      </div>
    -->
  </div>

  <!-- الإجمالي -->
  <div class="border-t p-4 font-bold">
    الإجمالي: <span id="cart-total">0 ج.م</span>
  </div>

  <!-- زر الدفع -->
  <div class="p-4">
    <button onclick="checkout()" class="w-full bg-gold-500 text-white py-3 rounded-lg">
      🛒 إتمام الشراء عبر WhatsApp
    </button>
  </div>
</div>
```

---

### 5️⃣ **Image Modal (نافذة الصورة)**

```html
<div id="image-modal" class="fixed inset-0 bg-black/80 hidden z-50 
     flex items-center justify-center hidden opacity-0 transition-opacity duration-300"
     onclick="closeImageModal()">
<!-- 
  opacity-0: في البداية (شفاف = غير مرئي)
  عند الفتح: opacity-100
-->

  <!-- الصورة -->
  <img id="modal-img" src="" alt="" class="max-w-90vh max-h-90vh object-contain 
       scale-95 transition-transform duration-300">
  <!-- 
    scale-95: في البداية (صغيرة قليلاً)
    عند الفتح: scale-100
  -->
</div>
```

---

### 6️⃣ **Toast Notification (الإشعارات)**

```html
<!-- رسالة التأكيد العائمة -->
<div id="toast" class="fixed bottom-4 left-4 bg-green-500 text-white p-4 rounded-lg
     opacity-0 translate-y-20 transition-all duration-300 z-50">
<!-- 
  opacity-0: مخفية في البداية
  translate-y-20: أسفل الشاشة
  عند الظهور: opacity-100 + translate-y-0
  تختفي بعد 3 ثوان
-->

  <span id="toast-message">تمت إضافة المنتج للسلة</span>
</div>
```

---

## 🎯 الفئات الرئيسية المستخدمة

### Tailwind Utilities:

| الفئة | الوصف |
|------|--------|
| `grid-cols-*` | تعديل عدد الأعمدة |
| `md:`, `lg:`, `sm:` | Breakpoints (نقاط توقف) |
| `hidden` / `flex` | إظهار/إخفاء العناصر |
| `sticky` / `fixed` / `relative` | طرق التموضع |
| `opacity-0` | شفافية (رقم من 0-100) |
| `scale-*` | تكبير/تصغير |
| `translate-*` | حركة وانزلاق |
| `hover:` / `group-hover:` | عند التمرير بالماوس |
| `transition` | حركات سلسة |
| `shadow-*` | ظلال |
| `bg-*` / `text-*` | ألوان |

---

## 🔄 تدفق المستخدم في الواجهة

```
1. المستخدم يفتح الموقع
   ↓
2. يرى:
   - Header (البحث + السلة)
   - صورة البطل
   - أزرار الفلاتر
   - شبكة المنتجات
   ↓
3. المستخدم يختار فئة أو يبحث
   ↓
4. القائمة تتحدث (renderGrid)
   ↓
5. ينقر على منتج (صورة) → Modal يظهر
   ↓
6. ينقر على "+" → addToCart يضيفه
   ↓
7. ينقر على سلة → Cart Drawer يظهر
   ↓
8. ينقر على "الشراء" → WhatsApp يفتح
```

---

## 📱 التوافق وعناصر التجاوب

```
📱 الموبايل (< 640px):
   ├── عمود واحد (grid-cols-1)
   ├── بحث بحجم صغير
   └── السلة بعرض 80%

📱 التابلت (640px - 1024px):
   ├── عمودين (sm:grid-cols-2)
   ├── بعض الأزرار تظهر (md:flex)
   └── سلة بعرض 20rem

💻 الويب (> 1024px):
   ├── 4 أعمدة (lg:grid-cols-4)
   ├── جميع الأزرار (lg:flex)
   └── سلة بعرض معياري
```

---

**تم شرح جميع أجزاء ملف HTML بالتفصيل ✨**
