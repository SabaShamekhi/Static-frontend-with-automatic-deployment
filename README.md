# Static-frontend-with-automatic-deployment
Learning about the principles and concepts of project version management and using git Learning about continuous integration and deployment 

# پاسخ سوالات: 

## ۱. پوشه .git چیست؟ چه اطلاعاتی در آن ذخیره می‌شود؟ چه دستوری برای ایجاد آن استفاده می‌شود؟
پوشه .git یک پوشه مخفی است که Git برای پیگیری تغییرات در یک مخزن (repository) از آن استفاده می‌کند. این پوشه شامل تمام اطلاعات متادیتا (metadata)، پیکربندی‌ها و تاریخچه پروژه است.
چه چیزهایی در داخل پوشه .git قرار دارد؟
- HEAD → اشاره‌گر به شاخه فعلی
- config → تنظیمات خاص مخزن
- index → پیگیری فایل‌هایی که برای کامیت بعدی در مرحله استیجینگ قرار دارند
- logs/ → تاریخچه همه ارجاعات (مانند شاخه‌ها، HEAD و ...)
- objects/ → ذخیره‌سازی تمام کامیت ها، درخت‌ها (trees) و blobها
- refs/ → ارجاعات به شاخه‌ها، مخازن remote و برچسب‌ها (tags)
دستور ایجاد پوشه .git:

```
git init
```

این دستور یک مخزن Git جدید را مقداردهی اولیه کرده و پوشه .git را در آن ایجاد می‌کند.
اگر در ریشه مخزن Git، دستور ls -a را اجرا کنیم، پوشه .git را میبینیم

## ۲. منظور از اتمی بودن (Atomicity) در commit اتمی و pull-request اتمی چیست؟
اتمی بودن یعنی یک عملیات یا به طور کامل انجام می‌شود یا اصلاً انجام نمی‌شود. این ویژگی تضمین می‌کند که هیچ تغییر ناقصی در مخزن باقی نمی‌ماند.
### Commit اتمی (Atomic Commit)

- یک commit باید شامل تمام تغییرات مرتبط با یکدیگر باشد.
- مثال: اگر یک باگ را رفع می‌کنید و مستندات آن را نیز به‌روزرسانی می‌کنیم ، هر دو باید در یک commit قرار بگیرند.
- روش اشتباه: رفع نصف باگ در یک commit و بقیه در commit بعدی.
مثال از دستور commit اتمی:

```
git commit -m "Fix login bug and update user validation"
```

 ### Pull Request اتمی (Atomic Pull Request)
- یک PR باید فقط شامل تغییرات منطقی مرتبط با یکدیگر باشد.
- روش اشتباه: یک PR که هم یک باگ را رفع کرده و هم یک قابلیت جدید اضافه می‌کند.
- روش درست: برای رفع باگ و افزودن قابلیت جدید، PRهای جداگانه ایجاد میکنیم.
پیش از merge، از squash کردن commitها با دستور git rebase -i استفاده می کنیم تا حالت اتمی حفظ شود.

## ۳. تفاوت بین دستورات fetch، pull، merge، rebase و cherry-pick را توضیح دهید.
### git fetch
تغییرات را از مخزن remote دریافت می‌کند بدون ادغام آن‌ها با شاخه فعلی.
زمانی که می‌خواهیم قبل از ادغام، تغییرات را بررسی کنیم کاربرد دارد و یا این که بخواهیم تمام برنچ ها را ببینیم که در مثال آمده
مثال:

```
git fetch origin
```


### git pull
تغییرات را از مخزن remote دریافت و بلافاصله با شاخه فعلی ادغام می‌کند.

```sh
git fetch origin
git merge origin/main
```


### git merge
به شکل زیر یک شاخه را با شاخه دیگر ادغام می‌کند و یک commit جدید برای ادغام ایجاد می‌شود.
 

```
git merge feature-branch
```

### git rebase
شاخه شما را به آخرین commit از شاخه‌ای دیگر منتقل می‌کندو مزیت آن این است که تاریخچه commitها را تمیزتر نگه می‌دارد بدون کامیت ادغام
 

```
git rebase main
```


### git cherry-pick
یک commit خاص را از یک شاخه برداشته و به شاخه فعلی منتقل می‌کند.
 مثال:

```
git cherry-pick <commit-hash>
```

 از دستور git log --oneline --graph برای مشاهده تصویری تفاوت‌ها می توان استفاده کرد

## ۴. تفاوت بین دستورات reset، revert، restore، switch و checkout را توضیح دهید.
### git reset
شاخص (pointer) شاخه را به commit قبلی بازمی‌گرداند.
 مثال: لغو آخرین commit بدون از دست دادن تغییرات:

```
git reset --soft HEAD~1
```

### git revert
یک commit جدید ایجاد می‌کند که اثرات یک commit قبلی را برعکس می‌کند.
 مثال:

```
git revert <commit-hash>
```

### git restore
فایل‌ها را از آخرین commit بازگردانی می‌کند، بدون اینکه commitهای قبلی را تغییر دهد.
 مثال:

```
git restore index.html
```

### git switch
تغییر شاخه به شاخه‌ای دیگر (جایگزین مدرن‌تر برای checkout).
 مثال:
```
git switch feature-branch
```

### git checkout
برای تغییر شاخه یا بازگردانی فایل‌ها استفاده می‌شود (روش قدیمی‌تر).
 مثال:

```
git checkout main
```


## ۵. منظور از Stage یا Index چیست؟ دستور stash چه کاری انجام می‌دهد؟
ناحیه Stage (یا Index) ناحیه‌ای است که تغییرات در آن قبل از commit شدن آماده‌سازی می‌شوند.
 مثال:
```
git add index.html
git commit -m "Update homepage"
```

### Stash
تغییراتی که هنوز commit نشده‌اند را به‌صورت موقت ذخیره می‌کند بدون اینکه آن‌ها را commit کند.
 مثال:
```
git stash
git pull origin main
git stash pop
```
می توانیم از دستور git stash list برای مشاهده لیست تغییرات stash شده استفاده کنیم.

 
## ۶. مفهوم Snapshot چیست و چه ارتباطی با commit دارد؟

گیت در هر commit یک تصویر (snapshot) کامل از پروژه ذخیره می‌کند و برخلاف سیستم‌های کنترل نسخه سنتی که فقط تفاوت‌ها را ذخیره می‌کنند، Git نسخه‌ی کامل فایل‌هایی که تغییر کرده‌اند را ذخیره می‌کند.

به طوری که یک commit در واقع یک snapshot از تمام تغییراتی است که در مرحله staging قرار گرفته‌اند.
 
```
git commit -m "Refactor navbar UI"
```
 از دستور git log --stat برای مشاهده snapshot های مربوط به commit‌ها استفاده میکنیم.


## ۷. تفاوت بین مخزن محلی (Local Repository) و مخزن راه‌دور (Remote Repository) چیست؟
### Local Repository
این مخزن روی کامپیوتر شخصی قرار دارد و تنها برای آن شخص قابل دسترسی است و نیازی به اینترنت ندارد و تغییرات به صورت محلی ذخیره می‌شود.
از دستورات git commit و git reset برای مدیریت این مخزن استفاده می‌شود. 

### Remote Repository
این مخزن روی یک سرور قرار دارد (مثلاً GitHub، GitLab) و همه  ی اعضای تیم می‌توانند به آن دسترسی داشته باشند.
برای هماهنگ‌سازی تغییرات با مخزن ریموت، باید از دستورات git push و git pull استفاده کنیم .
از دستورات git push و git fetch برای مدیریت این مخزن استفاده می‌شود.
برای مرج کردن از برنچ های ریموت به برنچ staging  در این تمرین ابتدا مجبور بودیم با git chechout یک بار به آن برنچ ریموت برویم و بعد بتوانیم آن برنچ را با برنچ staging مرج کنیم , قبل از آن می توانیم یا git fetch origin   تمام برنج ها را مشاهده کنیم 
مثال از Workflow
کار محلی:
```
git add .
git commit -m "Fix homepage CSS"
```
پوش به مخزن ریموت

```
git push origin main
```

پول تغییرات توسط تیم به برنچ staging : 
```
git pull origin staging
```





