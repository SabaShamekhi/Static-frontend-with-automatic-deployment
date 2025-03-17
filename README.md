# Static-frontend-with-automatic-deployment
Learning about the principles and concepts of project version management and using git Learning about continuous integration and deployment 

پاسخ سوالات: 
۱. پوشه .git چیست؟ چه اطلاعاتی در آن ذخیره می‌شود؟ چه دستوری برای ایجاد آن استفاده می‌شود؟
پوشه .git یک پوشه مخفی است که Git برای پیگیری تغییرات در یک مخزن (repository) از آن استفاده می‌کند. این پوشه شامل تمام اطلاعات متادیتا (metadata)، پیکربندی‌ها و تاریخچه پروژه است.
چه چیزهایی در داخل پوشه .git قرار دارد؟
- HEAD → اشاره‌گر به شاخه (branch) فعلی
- config → تنظیمات خاص مخزن
- index → پیگیری فایل‌هایی که برای commit بعدی در مرحله staging قرار دارند
- logs/ → تاریخچه همه ارجاعات (مانند شاخه‌ها، HEAD و ...)
- objects/ → ذخیره‌سازی تمام commitها، درخت‌ها (trees) و blobها
- refs/ → ارجاعات به شاخه‌ها، مخازن remote و برچسب‌ها (tags)
دستور ایجاد پوشه .git:

`git init`

این دستور یک مخزن Git جدید را مقداردهی اولیه کرده و پوشه .git را در آن ایجاد می‌کند.
اگر در ریشه مخزن Git، دستور ls -a را اجرا کنیم، پوشه .git را میبینیم

