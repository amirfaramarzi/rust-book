## متغیرها و تغییرپذیری

همان‌طور که در بخش [“ذخیره مقادیر با استفاده از متغیرها”][storing-values-with-variables]<!-- ignore --> ذکر شد، به طور پیش‌فرض متغیرها در Rust غیرقابل‌تغییر هستند. این یکی از راه‌هایی است که Rust شما را به نوشتن کدی که از ایمنی و همزمانی آسان ارائه‌شده توسط این زبان بهره می‌برد، تشویق می‌کند. با این حال، شما همچنان گزینه‌ای دارید تا متغیرهای خود را قابل‌تغییر کنید. بیایید بررسی کنیم که چگونه و چرا Rust شما را تشویق به استفاده از غیرقابل‌تغییر بودن می‌کند و چرا ممکن است گاهی بخواهید این حالت را تغییر دهید.

وقتی یک متغیر غیرقابل‌تغییر است، وقتی مقداری به یک نام متصل شد، نمی‌توانید آن مقدار را تغییر دهید. برای نشان دادن این موضوع، یک پروژه جدید به نام _variables_ در دایرکتوری _projects_ خود ایجاد کنید با استفاده از دستور `cargo new variables`.

سپس، در دایرکتوری جدید _variables_ خود، فایل _src/main.rs_ را باز کنید و کد آن را با کد زیر جایگزین کنید، که هنوز کامپایل نخواهد شد:

<span class="filename">تام فایل: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/src/main.rs}}
```


فایل را ذخیره کنید و برنامه را با استفاده از `cargo run` اجرا کنید. باید یک پیام خطا در مورد غیرقابل‌تغییر بودن دریافت کنید، همان‌طور که در این خروجی نشان داده شده است:


```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-01-variables-are-immutable/output.txt}}
```


این مثال نشان می‌دهد که چگونه کامپایلر به شما کمک می‌کند تا خطاها را در برنامه‌های خود پیدا کنید. خطاهای کامپایلر ممکن است ناامیدکننده باشند، اما در واقع به این معنا هستند که برنامه شما هنوز به طور ایمن کاری را که می‌خواهید انجام نمی‌دهد؛ این به هیچ وجه به این معنا نیست که شما برنامه‌نویس خوبی نیستید! حتی برنامه‌نویسان باتجربه Rust نیز همچنان خطاهای کامپایلر دریافت می‌کنند.

شما پیام خطای `` cannot assign twice to immutable variable `x` `` را دریافت کردید زیرا سعی کردید مقدار دوم را به متغیر غیرقابل‌تغییر `x` تخصیص دهید.

این بسیار مهم است که ما خطاهای زمان کامپایل را دریافت کنیم وقتی سعی می‌کنیم مقدار یک متغیر غیرقابل‌تغییر را تغییر دهیم زیرا این وضعیت می‌تواند به باگ منجر شود. اگر یک بخش از کد ما با این فرض عمل کند که یک مقدار هرگز تغییر نمی‌کند و بخش دیگری از کد آن مقدار را تغییر دهد، ممکن است بخش اول کد کاری که برای انجام آن طراحی شده بود را به درستی انجام ندهد. علت این نوع باگ می‌تواند بعد از وقوع به سختی قابل‌ردیابی باشد، به‌ویژه وقتی که بخش دوم کد فقط _گاهی اوقات_ مقدار را تغییر می‌دهد. کامپایلر Rust تضمین می‌کند که وقتی بیان می‌کنید یک مقدار تغییر نخواهد کرد، واقعاً تغییر نخواهد کرد، بنابراین نیازی نیست که خودتان این موضوع را پیگیری کنید. به این ترتیب کد شما راحت‌تر قابل‌درک خواهد بود.

اما قابلیت تغییر می‌تواند بسیار مفید باشد و نوشتن کد را راحت‌تر کند. اگرچه متغیرها به طور پیش‌فرض غیرقابل‌تغییر هستند، می‌توانید با اضافه کردن `mut` قبل از نام متغیر آنها را قابل‌تغییر کنید، همان‌طور که در [فصل ۲][storing-values-with-variables]<!-- ignore --> انجام دادید. اضافه کردن `mut` همچنین به خوانندگان آینده کد نیت شما را نشان می‌دهد که قسمت‌های دیگر کد مقدار این متغیر را تغییر خواهند داد.

برای مثال، بیایید فایل _src/main.rs_ را به کد زیر تغییر دهیم:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/src/main.rs}}
```


وقتی اکنون برنامه را اجرا می‌کنیم، این خروجی را دریافت می‌کنیم:



```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-02-adding-mut/output.txt}}
```



ما اجازه داریم مقدار مرتبط با `x` را از `5` به `6` تغییر دهیم وقتی که از `mut` استفاده شود. در نهایت، تصمیم‌گیری در مورد استفاده یا عدم استفاده از قابلیت تغییر به عهده شما است و به این بستگی دارد که در آن موقعیت خاص چه چیزی واضح‌تر به نظر می‌رسد.

### ثابت ها

مانند متغیرهای غیرقابل‌تغییر، _ثابت‌ها_ مقادیری هستند که به یک نام متصل می‌شوند و اجازه تغییر ندارند، اما چند تفاوت بین ثابت‌ها و متغیرها وجود دارد.

اول، شما نمی‌توانید از `mut` با ثابت‌ها استفاده کنید. ثابت‌ها نه تنها به طور پیش‌فرض غیرقابل‌تغییر هستند، بلکه همیشه غیرقابل‌تغییر هستند. شما ثابت‌ها را با استفاده از کلیدواژه `const` به جای کلیدواژه `let` تعریف می‌کنید و نوع مقدار _باید_ مشخص شود. ما در بخش بعدی [“انواع داده”][data-types]<!-- ignore --> درباره انواع و حاشیه‌نویسی نوع صحبت خواهیم کرد، بنابراین نگران جزئیات آن در حال حاضر نباشید. فقط بدانید که همیشه باید نوع را مشخص کنید.

ثابت‌ها می‌توانند در هر دامنه‌ای، از جمله دامنه‌ی جهانی، تعریف شوند، که این ویژگی آنها را برای مقادیری که بخش‌های مختلف کد باید بدانند مفید می‌سازد.

آخرین تفاوت این است که ثابت‌ها فقط می‌توانند به یک عبارت ثابت تنظیم شوند، نه نتیجه‌ای که فقط می‌تواند در زمان اجرا محاسبه شود.

در اینجا یک مثال از تعریف ثابت آورده شده است:

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

نام ثابت `THREE_HOURS_IN_SECONDS` است و مقدار آن برابر با نتیجه ضرب ۶۰ (تعداد ثانیه‌ها در یک دقیقه) در ۶۰ (تعداد دقیقه‌ها در یک ساعت) در ۳ (تعداد ساعت‌هایی که می‌خواهیم در این برنامه شمارش کنیم) تنظیم شده است. قانون نام‌گذاری ثابت‌ها در Rust استفاده از حروف بزرگ با خط زیر (_) بین کلمات است. کامپایلر قادر است مجموعه محدودی از عملیات را در زمان کامپایل ارزیابی کند، که به ما این امکان را می‌دهد تا این مقدار را به صورتی بنویسیم که آسان‌تر قابل‌درک و بررسی باشد، به جای تنظیم این ثابت به مقدار ۱۰،۸۰۰. برای اطلاعات بیشتر در مورد اینکه چه عملیات‌هایی می‌توانند در زمان تعریف ثابت‌ها استفاده شوند، به [بخش ارزیابی ثابت‌ها در مرجع Rust][const-eval] مراجعه کنید.

ثابت‌ها برای تمام مدت اجرای یک برنامه، در دامنه‌ای که در آن تعریف شده‌اند، معتبر هستند. این ویژگی، ثابت‌ها را برای مقادیر موجود در دامنه برنامه شما که ممکن است بخش‌های مختلف برنامه نیاز به دانستن آنها داشته باشند، مانند حداکثر تعداد امتیازاتی که هر بازیکن یک بازی می‌تواند کسب کند یا سرعت نور، مفید می‌سازد.

نام‌گذاری مقادیر ثابت در سراسر برنامه شما به عنوان ثابت‌ها، در انتقال معنی آن مقدار به نگهدارندگان آینده کد شما مفید است. همچنین این کمک می‌کند که فقط یک مکان در کد وجود داشته باشد که اگر مقدار ثابت نیاز به به‌روزرسانی داشت، باید تغییر کند.

### Shadowing

همان‌طور که در آموزش بازی حدس زدن در [فصل ۲][comparing-the-guess-to-the-secret-number]<!-- ignore --> دیدید، شما می‌توانید یک متغیر جدید با همان نام متغیر قبلی تعریف کنید. Rustaceanها می‌گویند که متغیر اول توسط متغیر دوم _سایه انداخته شده است_، به این معنا که متغیر دوم چیزی است که کامپایلر وقتی از نام متغیر استفاده می‌کنید می‌بیند. در واقع، متغیر دوم متغیر اول را تحت‌الشعاع قرار می‌دهد، استفاده‌های مربوط به نام متغیر را به خود اختصاص می‌دهد تا زمانی که یا خودش تحت‌الشعاع قرار بگیرد یا دامنه تمام شود. ما می‌توانیم یک متغیر را با استفاده از همان نام متغیر و تکرار استفاده از کلیدواژه `let` به شرح زیر سایه‌اندازی کنیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/src/main.rs}}
```

این برنامه ابتدا `x` را به مقدار `۵` متصل می‌کند. سپس یک متغیر جدید `x` با تکرار `let x =` ایجاد می‌کند و مقدار اصلی را می‌گیرد و `۱` اضافه می‌کند، بنابراین مقدار `x` به `۶` تغییر می‌کند. سپس، در یک دامنه داخلی که با آکولادها ایجاد شده است، عبارت سوم `let` نیز `x` را سایه‌اندازی می‌کند و یک متغیر جدید ایجاد می‌کند که مقدار قبلی را در `۲` ضرب می‌کند و به `x` مقدار `۱۲` می‌دهد. وقتی آن دامنه تمام می‌شود، سایه‌اندازی داخلی پایان می‌یابد و `x` به مقدار `۶` بازمی‌گردد. وقتی این برنامه را اجرا می‌کنیم، خروجی زیر را دریافت می‌کنیم:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-03-shadowing/output.txt}}
```

سایه‌اندازی با علامت‌گذاری متغیر به‌عنوان `mut` متفاوت است، زیرا اگر به طور تصادفی سعی کنید به این متغیر بدون استفاده از کلیدواژه `let` مقدار جدیدی تخصیص دهید، یک خطای زمان کامپایل دریافت می‌کنید. با استفاده از `let`، ما می‌توانیم چند تبدیل روی یک مقدار انجام دهیم، اما متغیر بعد از اتمام این تبدیل‌ها غیرقابل تغییر باقی می‌ماند.

تفاوت دیگر بین `mut` و سایه‌اندازی این است که به دلیل اینکه ما عملاً یک متغیر جدید ایجاد می‌کنیم وقتی دوباره از کلیدواژه `let` استفاده می‌کنیم، می‌توانیم نوع مقدار را تغییر دهیم اما همان نام را دوباره استفاده کنیم. برای مثال، فرض کنید برنامه ما از یک کاربر می‌خواهد تا نشان دهد که چند فاصله می‌خواهد بین متن‌های خاص داشته باشد با وارد کردن کاراکترهای فاصله، و سپس می‌خواهیم آن ورودی را به‌عنوان یک عدد ذخیره کنیم:

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-04-shadowing-can-change-types/src/main.rs:here}}
```

اولین متغیر `spaces` یک نوع رشته است و دومین متغیر `spaces` یک نوع عدد است. سایه‌اندازی در نتیجه ما را از نیاز به یافتن نام‌های مختلف، مانند `spaces_str` و `spaces_num` نجات می‌دهد. به جای آن، می‌توانیم از نام ساده‌تر `spaces` استفاده کنیم. با این حال، اگر سعی کنیم برای این کار از `mut` استفاده کنیم، همان‌طور که در اینجا نشان داده شده است، یک خطای زمان کامپایل دریافت می‌کنیم:

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/src/main.rs:here}}
```

خطا می‌گوید که مجاز نیستیم نوع متغیر را تغییر دهیم:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-05-mut-cant-change-types/output.txt}}
```

حال که بررسی کردیم متغیرها چگونه کار می‌کنند، بیایید نگاهی به انواع داده‌های بیشتری بیندازیم که متغیرها می‌توانند داشته باشند.
