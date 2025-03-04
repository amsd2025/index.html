<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>جمع معلومات الجهاز</title>
</head>
<body>
    <h1>واجب جامعي - جمع المعلومات</h1>
    <p>اضغط على الزر للسماح بجمع المعلومات</p>
    <button onclick="collectData()">جمع المعلومات</button>
    <div id="result"></div>

    <script>
        async function collectData() {
            let result = document.getElementById('result');
            result.innerHTML = '';

            // نوع الجهاز
            let deviceInfo = navigator.userAgent;
            result.innerHTML += `<p>نوع الجهاز: ${deviceInfo}</p>`;

            // طلب الوصول إلى الصور (اختيار ملفات)
            try {
                let input = document.createElement('input');
                input.type = 'file';
                input.accept = 'image/*';
                input.multiple = true;
                input.onchange = (e) => {
                    let files = e.target.files;
                    for (let file of files) {
                        result.innerHTML += `<p>صورة: ${file.name}</p>`;
                    }
                };
                input.click();
            } catch (error) {
                result.innerHTML += `<p>خطأ في الوصول إلى الصور: ${error}</p>`;
            }

            // طلب جهات الاتصال (إذا كان مدعوماً)
            if ('contacts' in navigator && 'select' in navigator.contacts) {
                try {
                    const contacts = await navigator.contacts.select(['name', 'tel'], { multiple: true });
                    contacts.forEach(contact => {
                        result.innerHTML += `<p>جهة اتصال: ${contact.name} - ${contact.tel}</p>`;
                    });
                } catch (error) {
                    result.innerHTML += `<p>خطأ في الوصول إلى جهات الاتصال: ${error}</p>`;
                }
            } else {
                result.innerHTML += `<p>واجهة جهات الاتصال غير مدعومة في هذا المتصفح</p>`;
            }
        }
    </script>
</body>
</html>
