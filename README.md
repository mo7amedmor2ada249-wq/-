<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>نظام إدارة الرعاية الصحية المتطور</title>
    <style>
        /* تحسين التصميم ليكون عصرياً ومريحاً للعين */
        body { font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; background: #f0f4f8; margin: 0; padding: 20px; color: #333; }
        .container { max-width: 500px; margin: auto; }
        .box { background: #fff; padding: 25px; margin-bottom: 20px; border-radius: 12px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); display: none; }
        h2, h3 { color: #2c7be5; margin-top: 0; }
        input { width: 100%; padding: 12px; margin: 10px 0; border: 1px solid #ddd; border-radius: 6px; box-sizing: border-box; }
        button { width: 100%; padding: 12px; background: #2c7be5; color: #fff; border: none; border-radius: 6px; cursor: pointer; font-size: 16px; transition: 0.3s; }
        button:hover { background: #1a5ab5; }
        .secondary-btn { background: #6c757d; margin-top: 10px; }
        .patient-card { background: #f8f9fa; padding: 10px; border-right: 4px solid #2c7be5; margin-top: 10px; border-radius: 4px; }
    </style>
</head>
<body>

<div class="container">
    <!-- قسم التسجيل -->
    <div class="box" id="registerSection" style="display:block">
        <h2>إنشاء حساب جديد</h2>
        <input id="ru" placeholder="اسم المستخدم">
        <input id="rp" type="password" placeholder="كلمة المرور">
        <button onclick="register()">تسجيل</button>
        <button class="secondary-btn" onclick="showSection('loginSection')">لديك حساب؟ دخول</button>
    </div>

    <!-- قسم تسجيل الدخول -->
    <div class="box" id="loginSection">
        <h2>تسجيل الدخول</h2>
        <input id="lu" placeholder="اسم المستخدم">
        <input id="lp" type="password" placeholder="كلمة المرور">
        <button onclick="login()">دخول</button>
    </div>

    <!-- لوحة التحكم -->
    <div class="box" id="dashboard">
        <h2>لوحة التحكم</h2>
        <p>مرحباً بك في النظام الصحي</p>
        <button onclick="showSection('patientForm')">تسجيل مريض جديد</button>
        <button onclick="showSection('appointForm')" style="margin-top:10px">حجز موعد</button>
        <button onclick="viewResults()" style="margin-top:10px">عرض المرضى المسجلين</button>
        <button class="secondary-btn" onclick="location.reload()">تسجيل الخروج</button>
    </div>

    <!-- نموذج المريض -->
    <div class="box" id="patientForm">
        <h3>إضافة بيانات مريض</h3>
        <input id="pName" placeholder="اسم المريض">
        <input id="pAge" type="number" placeholder="العمر">
        <input id="pDisease" placeholder="الحالة التشخيصية">
        <button onclick="savePatient()">حفظ البيانات</button>
        <button class="secondary-btn" onclick="showSection('dashboard')">رجوع</button>
    </div>

    <!-- نموذج المواعيد -->
    <div class="box" id="appointForm">
        <h3>حجز موعد جديد</h3>
        <input id="appDate" type="date">
        <input id="appTime" type="time">
        <button onclick="confirmAppointment()">تأكيد الموعد</button>
        <button class="secondary-btn" onclick="showSection('dashboard')">رجوع</button>
    </div>

    <!-- عرض النتائج -->
    <div class="box" id="resultView">
        <h3>قائمة المرضى والبيانات</h3>
        <div id="patientsList"></div>
        <button class="secondary-btn" onclick="showSection('dashboard')">رجوع</button>
    </div>
</div>

<script>
    // وظيفة التنقل بين الأقسام (تم إصلاح الخطأ السابق)
    function showSection(id) {
        document.querySelectorAll('.box').forEach(box => box.style.display = "none");
        document.getElementById(id).style.display = "block";
    }

    // التسجيل
    function register() {
        const u = document.getElementById('ru').value;
        const p = document.getElementById('rp').value;
        if(u && p) {
            localStorage.setItem('user_' + u, p);
            alert("تم التسجيل بنجاح!");
            showSection('loginSection');
        } else {
            alert("يرجى ملء كافة الحقول");
        }
    }

    // تسجيل الدخول
    function login() {
        const u = document.getElementById('lu').value;
        const p = document.getElementById('lp').value;
        if(localStorage.getItem('user_' + u) === p) {
            showSection('dashboard');
        } else {
            alert("خطأ في اسم المستخدم أو كلمة المرور");
        }
    }

    // حفظ بيانات المريض فعلياً (تعديل 2026)
    function savePatient() {
        const patient = {
            name: document.getElementById('pName').value,
            age: document.getElementById('pAge').value,
            disease: document.getElementById('pDisease').value
        };
        
        let patients = JSON.parse(localStorage.getItem('patients') || "[]");
        patients.push(patient);
        localStorage.setItem('patients', JSON.stringify(patients));
        
        alert("تم حفظ بيانات المريض بنجاح!");
        showSection('dashboard');
    }

    // عرض قائمة المرضى المخزنة
    function viewResults() {
        const listDiv = document.getElementById('patientsList');
        const patients = JSON.parse(localStorage.getItem('patients') || "[]");
        
        listDiv.innerHTML = patients.length === 0 ? "<p>لا يوجد مرضى مسجلين.</p>" : "";
        
        patients.forEach(p => {
            listDiv.innerHTML += `
                <div class="patient-card">
                    <strong>الاسم:</strong> ${p.name}<br>
                    <strong>العمر:</strong> ${p.age}<br>
                    <strong>الحالة:</strong> ${p.disease}
                </div>`;
        });
        showSection('resultView');
    }

    function confirmAppointment() {
        const date = document.getElementById('appDate').value;
        const time = document.getElementById('appTime').value;
        if(date && time) {
            alert(`تم تأكيد الموعد بتاريخ ${date} الساعة ${time}`);
            showSection('dashboard');
        } else {
            alert("يرجى اختيار التاريخ والوقت");
        }
    }
</script>

</body>
</html>
