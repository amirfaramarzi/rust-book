## ضمیمه ی - چگونگی توسعه Rust و "Rust Nightly"

این ضمیمه درباره چگونگی توسعه Rust و تأثیر آن بر شما به عنوان یک توسعه‌دهنده Rust است.

### ثبات بدون رکود

به عنوان یک زبان، Rust به _ثبات_ کد شما بسیار اهمیت می‌دهد. ما می‌خواهیم Rust یک پایه محکم و قابل اعتماد باشد که بتوانید بر روی آن بسازید، و اگر همه چیز به طور مداوم تغییر می‌کرد، این امکان‌پذیر نبود. در عین حال، اگر نتوانیم با ویژگی‌های جدید آزمایش کنیم، ممکن است مشکلات مهمی را تا بعد از انتشار آن‌ها کشف نکنیم، زمانی که دیگر نمی‌توان تغییراتی ایجاد کرد.

راه‌حل ما برای این مشکل چیزی است که ما آن را "ثبات بدون رکود" می‌نامیم، و اصل راهنمای ما این است: شما هرگز نباید از ارتقاء به یک نسخه جدید از Rust پایدار بترسید. هر ارتقاء باید بدون دردسر باشد، اما همچنین ویژگی‌های جدید، باگ‌های کمتر، و زمان‌های کامپایل سریع‌تر را برای شما به ارمغان بیاورد.

### چو، چو! کانال‌های انتشار و حرکت قطارها

توسعه Rust بر اساس یک _برنامه زمانی قطار_ عمل می‌کند. یعنی تمام توسعه‌ها در شاخه `master` مخزن Rust انجام می‌شود. انتشارها از مدل قطار انتشار نرم‌افزار پیروی می‌کنند، مدلی که توسط Cisco IOS و پروژه‌های نرم‌افزاری دیگر استفاده شده است. سه _کانال انتشار_ برای Rust وجود دارد:

- Nightly
- Beta
- Stable

</div>


بیشتر توسعه‌دهندگان Rust عمدتاً از کانال پایدار استفاده می‌کنند، اما کسانی که می‌خواهند ویژگی‌های آزمایشی جدید را امتحان کنند ممکن است از کانال‌های nightly یا beta استفاده کنند.

در اینجا مثالی از نحوه کار فرآیند توسعه و انتشار آورده شده است: فرض کنید تیم Rust روی انتشار نسخه Rust 1.5 کار می‌کند. آن انتشار در دسامبر 2015 اتفاق افتاد، اما اعداد نسخه‌ای واقعی به ما ارائه می‌دهد. یک ویژگی جدید به Rust اضافه می‌شود: یک commit جدید به شاخه `master` اضافه می‌شود. هر شب، یک نسخه جدید nightly از Rust تولید می‌شود. هر روز یک روز انتشار است، و این نسخه‌ها به طور خودکار توسط زیرساخت انتشار ما ایجاد می‌شوند. بنابراین با گذشت زمان، انتشارهای ما به این صورت خواهند بود، هر شب:

```text
nightly: * - - * - - *
```

هر شش هفته، زمان آماده‌سازی یک انتشار جدید است! شاخه `beta` مخزن Rust از شاخه `master` که برای nightly استفاده می‌شود منشعب می‌شود. اکنون دو نسخه وجود دارد:

```text
nightly: * - - * - - *
                     |
beta:                *
```

بیشتر کاربران Rust به طور فعال از نسخه‌های beta استفاده نمی‌کنند، اما در سیستم CI خود علیه beta تست می‌گیرند تا به Rust کمک کنند که مشکلات احتمالی را شناسایی کند. در همین حال، هنوز هر شب یک نسخه nightly منتشر می‌شود:

```text
nightly: * - - * - - * - - * - - *
                     |
beta:                *
```

فرض کنید یک مشکل (regression) پیدا شود. خوشبختانه ما زمانی برای تست نسخه beta داشتیم قبل از اینکه مشکل وارد نسخه پایدار شود! اصلاح به شاخه `master` اعمال می‌شود، بنابراین nightly اصلاح می‌شود، و سپس این اصلاح به شاخه `beta` بازگردانده می‌شود، و یک نسخه جدید از beta تولید می‌شود:

```text
nightly: * - - * - - * - - * - - * - - *
                     |
beta:                * - - - - - - - - *
```

شش هفته پس از ایجاد اولین نسخه beta، زمان انتشار نسخه پایدار است! شاخه `stable` از شاخه `beta` تولید می‌شود:

```text
nightly: * - - * - - * - - * - - * - - * - * - *
                     |
beta:                * - - - - - - - - *
                                       |
stable:                                *
```

هورا! Rust 1.5 آماده است! اما یک چیز را فراموش کرده‌ایم: چون شش هفته گذشته است، ما به نسخه beta جدیدی از _نسخه بعدی_ Rust، یعنی 1.6، نیاز داریم. بنابراین پس از اینکه شاخه `stable` از `beta` جدا شد، نسخه بعدی `beta` دوباره از `nightly` منشعب می‌شود:

```text
nightly: * - - * - - * - - * - - * - - * - * - *
                     |                         |
beta:                * - - - - - - - - *       *
                                       |
stable:                                *
```

این مدل "قطار" نامیده می‌شود، زیرا هر شش هفته، یک انتشار "ایستگاه را ترک می‌کند"، اما همچنان باید از کانال beta عبور کند تا به یک انتشار پایدار تبدیل شود.

انتشارهای Rust هر شش هفته، مانند ساعت دقیق انجام می‌شوند. اگر تاریخ یک انتشار Rust را بدانید، می‌توانید تاریخ انتشار بعدی را بدانید: شش هفته بعد. یکی از جنبه‌های خوب داشتن انتشارهای برنامه‌ریزی‌شده هر شش هفته این است که قطار بعدی به زودی می‌آید. اگر یک ویژگی به طور اتفاقی یک انتشار خاص را از دست بدهد، نیازی به نگرانی نیست: انتشار بعدی در مدت کوتاهی اتفاق می‌افتد! این امر به کاهش فشار برای افزودن ویژگی‌های احتمالاً ناقص نزدیک به مهلت انتشار کمک می‌کند.

با تشکر از این فرآیند، شما همیشه می‌توانید نسخه بعدی Rust را بررسی کرده و برای خود تأیید کنید که ارتقاء به آن آسان است: اگر یک نسخه beta مطابق انتظار عمل نکند، می‌توانید آن را به تیم گزارش دهید و قبل از اینکه انتشار پایدار بعدی انجام شود، آن را اصلاح کنید! شکستن در یک نسخه beta نسبتاً نادر است، اما `rustc` همچنان یک نرم‌افزار است و باگ‌ها وجود دارند.

### زمان نگهداری

پروژه Rust از آخرین نسخه پایدار پشتیبانی می‌کند. وقتی یک نسخه پایدار جدید منتشر می‌شود، نسخه قدیمی به پایان عمر خود (EOL) می‌رسد. این به این معنی است که هر نسخه برای شش هفته پشتیبانی می‌شود.

### ویژگی‌های ناپایدار

یک نکته دیگر در این مدل انتشار وجود دارد: ویژگی‌های ناپایدار. Rust از تکنیکی به نام "پرچم‌های ویژگی" (feature flags) استفاده می‌کند تا تعیین کند چه ویژگی‌هایی در یک انتشار فعال هستند. اگر یک ویژگی جدید تحت توسعه فعال باشد، روی شاخه `master` قرار می‌گیرد و بنابراین، در nightly، اما پشت یک _پرچم ویژگی_ قرار می‌گیرد. اگر به‌عنوان کاربر، مایلید ویژگی در حال توسعه را امتحان کنید، می‌توانید این کار را انجام دهید، اما باید از نسخه nightly Rust استفاده کرده و کد منبع خود را با پرچم مناسب برای فعال‌سازی آن علامت‌گذاری کنید.

اگر از نسخه beta یا پایدار Rust استفاده می‌کنید، نمی‌توانید از پرچم‌های ویژگی استفاده کنید. این نکته‌ای است که به ما اجازه می‌دهد از ویژگی‌های جدید به صورت عملی استفاده کنیم قبل از اینکه آن‌ها را برای همیشه پایدار اعلام کنیم. کسانی که مایلند از ویژگی‌های پیشرفته استفاده کنند، می‌توانند این کار را انجام دهند، و کسانی که تجربه‌ای پایدار و قابل اعتماد می‌خواهند می‌توانند با نسخه پایدار بمانند و مطمئن باشند که کد آن‌ها خراب نخواهد شد. ثبات بدون رکود.

این کتاب فقط شامل اطلاعات مربوط به ویژگی‌های پایدار است، زیرا ویژگی‌های در حال توسعه همچنان در حال تغییر هستند و مطمئناً بین زمانی که این کتاب نوشته شده و زمانی که در نسخه‌های پایدار فعال می‌شوند، متفاوت خواهند بود. می‌توانید مستندات مربوط به ویژگی‌هایی که فقط در nightly موجود هستند را به صورت آنلاین پیدا کنید.

### Rustup و نقش Rust Nightly

ابزار Rustup تغییر بین کانال‌های مختلف انتشار Rust را، به صورت جهانی یا بر اساس هر پروژه، آسان می‌کند. به طور پیش‌فرض، Rust پایدار نصب خواهد بود. برای نصب نسخه nightly، به عنوان مثال:

```console
$ rustup toolchain install nightly
```

همچنین می‌توانید تمام _ابزارهای موجود_ (نسخه‌های Rust و اجزای مرتبط) که با `rustup` نصب کرده‌اید را ببینید. در اینجا مثالی از یک کامپیوتر ویندوزی یکی از نویسندگان آورده شده است:

```powershell
> rustup toolchain list
stable-x86_64-pc-windows-msvc (default)
beta-x86_64-pc-windows-msvc
nightly-x86_64-pc-windows-msvc
```

همان‌طور که می‌بینید، ابزار stable به طور پیش‌فرض تنظیم شده است. بیشتر کاربران Rust بیشتر وقت خود از stable استفاده می‌کنند. ممکن است بخواهید بیشتر وقت خود از stable استفاده کنید، اما در یک پروژه خاص از nightly استفاده کنید، زیرا به یک ویژگی پیشرفته علاقه دارید. برای انجام این کار، می‌توانید از `rustup override` در دایرکتوری آن پروژه استفاده کنید تا ابزار nightly را به‌عنوان ابزار مورد استفاده `rustup` در آن دایرکتوری تنظیم کنید:

```console
$ cd ~/projects/needs-nightly
$ rustup override set nightly
```

اکنون، هر بار که در دایرکتوری _~/projects/needs-nightly_ دستور `rustc` یا `cargo` را فراخوانی کنید، `rustup` اطمینان حاصل می‌کند که شما از Rust nightly استفاده می‌کنید، نه نسخه پایدار پیش‌فرض. این ویژگی زمانی که پروژه‌های زیادی با Rust دارید، بسیار مفید است!

### فرآیند RFC و تیم‌ها

چگونه می‌توانید درباره این ویژگی‌های جدید اطلاعات کسب کنید؟ مدل توسعه Rust از یک فرآیند _درخواست نظرات (RFC)_ پیروی می‌کند. اگر بهبود خاصی در Rust می‌خواهید، می‌توانید یک پیشنهاد بنویسید که به آن RFC گفته می‌شود.

هر کسی می‌تواند RFC بنویسد تا Rust را بهبود دهد، و این پیشنهادها توسط تیم Rust که از چندین زیرتیم موضوعی تشکیل شده است، بررسی و بحث می‌شوند. لیست کامل تیم‌ها [در وب‌سایت Rust](https://www.rust-lang.org/governance) موجود است، که شامل تیم‌هایی برای هر بخش از پروژه می‌شود: طراحی زبان، پیاده‌سازی کامپایلر، زیرساخت، مستندات و موارد دیگر. تیم مربوطه پیشنهاد و نظرات را می‌خواند، نظرات خود را می‌نویسد، و در نهایت، توافقی برای پذیرش یا رد ویژگی حاصل می‌شود.

اگر ویژگی پذیرفته شود، یک issue در مخزن Rust باز می‌شود و کسی می‌تواند آن را پیاده‌سازی کند. فردی که آن را پیاده‌سازی می‌کند، ممکن است همان فردی نباشد که ویژگی را ابتدا پیشنهاد داده است! وقتی پیاده‌سازی آماده شد، روی شاخه `master` پشت یک پرچم ویژگی قرار می‌گیرد، همان‌طور که در بخش [“ویژگی‌های ناپایدار”](#unstable-features)<!-- ignore --> بحث شد.

پس از مدتی، زمانی که توسعه‌دهندگان Rust که از نسخه‌های nightly استفاده می‌کنند توانسته‌اند ویژگی جدید را امتحان کنند، اعضای تیم درباره این ویژگی، نحوه عملکرد آن در nightly و تصمیم‌گیری می‌کنند که آیا باید وارد Rust پایدار شود یا نه. اگر تصمیم بر ادامه باشد، پرچم ویژگی حذف می‌شود و ویژگی اکنون پایدار تلقی می‌شود! سپس این ویژگی وارد نسخه پایدار جدید Rust می‌شود.
