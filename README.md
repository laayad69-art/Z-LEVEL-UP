# Z-Level Up 🦸‍♂️

تطبيق ويب بسيط وممتع لتتبع العادات اليومية بطريقة تشبه الألعاب (Gamification). كل عادة تكملها تمنحك نقاط خبرة (XP)، وعندما تجمع 100 نقطة ترتقي إلى مستوى أعلى!

### المميزات الرئيسية
- تتبع عادات يومية بسيطة
- كسب 20 نقطة خبرة لكل عادة مكتملة
- شريط تقدم ومستوى يرتفع تلقائيًا عند الوصول إلى 100 نقطة
- إحصائيات بسيطة (عدد المهام المكتملة منذ البداية)
- إضافة عادات جديدة بسهولة
- حفظ البيانات محليًا باستخدام `localStorage` (لا يُمسح إلا بمسح بيانات المتصفح)
- تصميم عصري ومتجاوب مع الهواتف المحمولة
- دعم كامل للغة العربية والاتجاه من اليمين إلى اليسار (RTL)

### لقطة شاشة (تخيلية)
(يمكنك فتح الملف في المتصفح لرؤية التصميم الفعلي)

### كيفية الاستخدام
1. احفظ الكود أدناه في ملف باسم `index.html`
2. افتح الملف في أي متصفح (Chrome، Firefox، Safari...)
3. ابدأ بإكمال العادات اليومية بالضغط على زر التحقق ✅
4. أضف عادات جديدة بالضغط على زر الـ (+) في الأسفل

### العادات الافتراضية
- شرب لتر ماء
- قراءة 5 صفحات
- تمرين ضغط سريع

يمكنك حذفها أو إضافتها حسب رغبتك (بالضغط على الزر مرة أخرى تُلغى العادة وتُخصم النقاط).

### التقنيات المستخدمة
- HTML5
- CSS3 (مع متغيرات CSS)
- JavaScript نقي (Vanilla JS)
- Font Awesome للأيقونات
- خط Changa من Google Fonts
- لا توجد مكتبات خارجية أو إطار عمل (Framework)

### ملاحظات
- التطبيق يعمل بدون إنترنت تمامًا بعد التحميل الأول (بسبب تحميل الخطوط والأيقونات من CDN).
- البيانات محفوظة في المتصفح فقط، فإذا مسحت الكاش أو غيرت الجهاز ستفقد تقدمك.
- يمكن تطويره لاحقًا بإضافة ميزات مثل: إعادة تعيين يومي تلقائي، إشعارات، تصدير البيانات، إلخ.

### الكود الكامل
انسخ الكود التالي وحفظه في ملف `index.html`:

```html
<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Z-Level Up</title>
<link href="https://fonts.googleapis.com/css2?family=Changa:wght@300;500;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
:root {
  --bg-body: #f0f2f5;
  --card-bg: #ffffff;
  --primary: #4f46e5;
  --accent: #f59e0b;
  --text-main: #1e293b;
}
body {
  font-family: 'Changa', sans-serif;
  background-color: var(--bg-body);
  margin: 0;
  padding-bottom: 80px;
}
header {
  background: var(--primary);
  color: white;
  padding: 20px;
  text-align: center;
  border-radius: 0 0 25px 25px;
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
.xp-bar-container {
  background: rgba(255,255,255,0.2);
  height: 10px;
  border-radius: 5px;
  margin-top: 15px;
  overflow: hidden;
}
#xp-progress {
  background: var(--accent);
  height: 100%;
  width: 0%;
  transition: width 0.5s ease;
}
.card {
  background: var(--card-bg);
  margin: 15px;
  padding: 20px;
  border-radius: 20px;
  box-shadow: 0 2px 10px rgba(0,0,0,0.05);
  transition: transform 0.2s;
}
.card:active { transform: scale(0.98); }
.habit-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid #eee;
  padding: 10px 0;
}
.habit-info h4 { margin: 0; color: var(--text-main); }
.check-btn {
  background: #eef2ff;
  color: var(--primary);
  border: none;
  width: 45px;
  height: 45px;
  border-radius: 12px;
  cursor: pointer;
  font-size: 1.2rem;
}
.check-btn.done {
  background: #10b981;
  color: white;
}
nav.bottom-nav {
  position: fixed;
  bottom: 0;
  width: 100%;
  background: white;
  display: flex;
  justify-content: space-around;
  padding: 12px 0;
  box-shadow: 0 -2px 15px rgba(0,0,0,0.1);
  z-index: 100;
}
.nav-item {
  color: #94a3b8;
  text-decoration: none;
  font-size: 0.8rem;
  text-align: center;
}
.nav-item.active { color: var(--primary); }
.nav-item i { font-size: 1.4rem; display: block; }
.section { display: none; padding: 10px; }
.section.active { display: block; animation: fadeIn 0.3s; }
@keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
.btn-add {
  position: fixed;
  bottom: 100px;
  left: 20px;
  background: var(--accent);
  color: white;
  width: 60px;
  height: 60px;
  border-radius: 50%;
  border: none;
  box-shadow: 0 4px 15px rgba(245, 158, 11, 0.4);
  font-size: 1.5rem;
  z-index: 99;
}
</style>
</head>
<body>

<header>
  <h2 style="margin:0">مستوى البطل <span id="level-tag">1</span></h2>
  <div class="xp-bar-container">
    <div id="xp-progress"></div>
  </div>
  <small>النقاط: <span id="xp-count">0</span> / 100</small>
</header>

<main>
  <section id="habits-sec" class="section active">
    <h3 style="margin: 15px;">عاداتك اليومية</h3>
    <div id="habit-list"></div>
  </section>

  <section id="stats-sec" class="section">
    <div class="card" style="text-align: center;">
      <i class="fas fa-trophy" style="font-size: 3rem; color: var(--accent);"></i>
      <h3>إنجازاتك</h3>
      <p id="total-tasks">أكملت 0 مهمة منذ البداية</p>
    </div>
  </section>
</main>

<button class="btn-add" onclick="addNewHabit()"><i class="fas fa-plus"></i></button>

<nav class="bottom-nav">
  <a href="#" class="nav-item active" onclick="switchTab('habits-sec', this)">
    <i class="fas fa-check-circle"></i>عاداتي
  </a>
  <a href="#" class="nav-item" onclick="switchTab('stats-sec', this)">
    <i class="fas fa-chart-line"></i>إحصائيات
  </a>
</nav>

<script>
let userData = JSON.parse(localStorage.getItem('zlevel_data')) || {
    xp: 0,
    level: 1,
    totalCompleted: 0,
    habits: [
      { id: 1, name: "شرب لتر ماء", done: false },
      { id: 2, name: "قراءة 5 صفحات", done: false },
      { id: 3, name: "تمرين ضغط سريع", done: false }
    ]
};

function saveData() {
  localStorage.setItem('zlevel_data', JSON.stringify(userData));
  render();
}

function render() {
  document.getElementById('level-tag').innerText = userData.level;
  document.getElementById('xp-count').innerText = userData.xp;
  document.getElementById('xp-progress').style.width = userData.xp + "%";
  document.getElementById('total-tasks').innerText = `أكملت ${userData.totalCompleted} مهمة منذ البداية`;

  const list = document.getElementById('habit-list');
  list.innerHTML = '';

  userData.habits.forEach(habit => {
    list.innerHTML += `
      <div class="card habit-row">
        <div class="habit-info">
          <h4>${habit.name}</h4>
        </div>
        <button class="check-btn \( {habit.done ? 'done' : ''}" onclick="toggleHabit( \){habit.id})">
          <i class="fas ${habit.done ? 'fa-check-double' : 'fa-check'}"></i>
        </button>
      </div>`;
  });
}

function toggleHabit(id) {
  const habit = userData.habits.find(h => h.id === id);
  if (!habit.done) {
    habit.done = true;
    userData.xp += 20;
    userData.totalCompleted++;
    if (userData.xp >= 100) {
      userData.level++;
      userData.xp = 0;
      alert("يا بطل! ارتقيت للمستوى " + userData.level);
    }
  } else {
    habit.done = false;
    userData.xp = Math.max(0, userData.xp - 20);
    userData.totalCompleted--;
  }
  saveData();
}

function addNewHabit() {
  const name = prompt("ما هي العادة الجديدة؟");
  if (name && name.trim() !== "") {
    userData.habits.push({ id: Date.now(), name: name, done: false });
    saveData();
  }
}

function switchTab(id, el) {
  document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
  document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
  document.getElementById(id).classList.add('active');
  el.classList.add('active');
}

render();
</script>
</body>
</html>
