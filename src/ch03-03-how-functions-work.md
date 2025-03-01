## توابع

توابع در کدهای راست بسیار رایج هستند. شما تاکنون یکی از مهم‌ترین توابع در این زبان را دیده‌اید: تابع `main`، که نقطه ورود بسیاری از برنامه‌ها است. همچنین با کلمه کلیدی `fn` آشنا شدید که به شما امکان تعریف توابع جدید را می‌دهد.

کدهای راست از _حالت snake case_ به عنوان سبک متعارف برای نام‌گذاری توابع و متغیرها استفاده می‌کنند، که در آن تمام حروف کوچک هستند و کلمات با زیرخط از یکدیگر جدا می‌شوند. این یک برنامه است که شامل یک مثال از تعریف تابع می‌باشد:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-16-functions/src/main.rs}}
```

ما با وارد کردن `fn` به همراه نام تابع و یک مجموعه پرانتز، یک تابع را در راست تعریف می‌کنیم. کروشه‌های باز و بسته به کامپایلر می‌گویند که بدنه تابع از کجا شروع و پایان می‌یابد.

ما می‌توانیم هر تابعی را که تعریف کرده‌ایم با وارد کردن نام آن به همراه یک مجموعه پرانتز فراخوانی کنیم. از آنجا که `another_function` در برنامه تعریف شده است، می‌توان آن را از داخل تابع `main` فراخوانی کرد. توجه داشته باشید که ما `another_function` را _بعد از_ تابع `main` در کد منبع تعریف کردیم؛ همچنین می‌توانستیم آن را قبل از آن تعریف کنیم. راست اهمیتی نمی‌دهد که توابع شما کجا تعریف شده‌اند، فقط باید در محدوده‌ای باشند که توسط فراخوانی کننده قابل مشاهده باشد.

بیایید یک پروژه باینری جدید به نام _functions_ ایجاد کنیم تا توابع را بیشتر بررسی کنیم. مثال `another_function` را در فایل _src/main.rs_ قرار دهید و آن را اجرا کنید. باید خروجی زیر را مشاهده کنید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-16-functions/output.txt}}
```

دستورات به ترتیبی که در تابع `main` ظاهر شده‌اند اجرا می‌شوند. ابتدا پیام “Hello, world!” چاپ می‌شود و سپس `another_function` فراخوانی شده و پیام آن چاپ می‌شود.

### پارامترها

ما می‌توانیم توابعی تعریف کنیم که _پارامتر_ داشته باشند، که متغیرهای خاصی هستند که بخشی از امضای تابع محسوب می‌شوند. وقتی یک تابع پارامتر دارد، شما می‌توانید مقادیر مشخصی برای آن پارامترها ارائه دهید. از نظر فنی، به مقادیر مشخص _آرگومان_ گفته می‌شود، اما در مکالمات معمول، مردم معمولاً از کلمات _پارامتر_ و _آرگومان_ به جای یکدیگر استفاده می‌کنند، چه برای متغیرهای تعریف شده در یک تابع یا مقادیر مشخص هنگام فراخوانی تابع.

در این نسخه از `another_function`، ما یک پارامتر اضافه می‌کنیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/src/main.rs}}
```

این برنامه را اجرا کنید؛ باید خروجی زیر را ببینید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-17-functions-with-parameters/output.txt}}
```

اعلان `another_function` دارای یک پارامتر به نام `x` است. نوع `x` به عنوان `i32` مشخص شده است. وقتی ما مقدار `5` را به `another_function` می‌دهیم، ماکروی `println!` مقدار `5` را در جایی که جفت کروشه حاوی `x` در رشته فرمت بود، قرار می‌دهد.

در امضای توابع، شما _باید_ نوع هر پارامتر را اعلام کنید. این یک تصمیم عمدی در طراحی راست است: نیاز به حاشیه‌نویسی نوع در تعریف توابع به این معنا است که کامپایلر تقریباً هرگز نیازی به استفاده از آنها در جاهای دیگر کد برای فهمیدن نوع مورد نظر شما ندارد. کامپایلر همچنین قادر است پیام‌های خطای مفیدتری ارائه دهد اگر بداند تابع چه نوع‌هایی انتظار دارد.

هنگام تعریف چندین پارامتر، اعلام پارامترها را با کاما جدا کنید، مانند این:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/src/main.rs}}
```

این مثال یک تابع به نام `print_labeled_measurement` با دو پارامتر ایجاد می‌کند. پارامتر اول به نام `value` و از نوع `i32` است. پارامتر دوم به نام `unit_label` و از نوع `char` است. سپس تابع متنی حاوی هر دو `value` و `unit_label` را چاپ می‌کند.

بیایید این کد را اجرا کنیم. برنامه‌ای که در حال حاضر در فایل _src/main.rs_ پروژه _functions_ شما است را با مثال بالا جایگزین کنید و آن را با استفاده از `cargo run` اجرا کنید:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-18-functions-with-multiple-parameters/output.txt}}
```

از آنجا که ما تابع را با `5` به عنوان مقدار برای `value` و `'h'` به عنوان مقدار برای `unit_label` فراخوانی کردیم، خروجی برنامه شامل این مقادیر است.

### اظهارات و عبارات

بدنه توابع از یک سری اظهارات تشکیل شده است که به طور اختیاری با یک عبارت پایان می‌یابند. تاکنون، توابعی که پوشش داده‌ایم شامل یک عبارت پایانی نبوده‌اند، اما شما یک عبارت را به عنوان بخشی از یک اظهار دیده‌اید. از آنجا که راست یک زبان مبتنی بر عبارات است، این تمایز بسیار مهم است که درک شود. زبان‌های دیگر این تمایز را ندارند، بنابراین بیایید نگاهی به اظهارات و عبارات بیندازیم و ببینیم چگونه تفاوت‌های آنها بر بدن توابع تأثیر می‌گذارد.

- **اظهارات** دستورالعمل‌هایی هستند که یک عمل انجام می‌دهند و هیچ مقداری باز نمی‌گردانند.
- **عبارات** به یک مقدار نتیجه‌گیری می‌رسند. بیایید چند مثال را بررسی کنیم.

ما در واقع قبلاً از اظهارات و عبارات استفاده کرده‌ایم. ایجاد یک متغیر و اختصاص یک مقدار به آن با کلمه کلیدی `let` یک اظهار است. در لیستینگ ۳-۱، `let y = 6;` یک اظهار است.

<Listing number="3-1" file-name="src/main.rs" caption="تعریف تابع `main` که شامل یک اظهار است">

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/listing-03-01/src/main.rs}}
```

</Listing>

تعریف توابع نیز اظهارات هستند؛ کل مثال پیشین خود یک اظهار است. (همانطور که در زیر خواهیم دید، _فراخوانی_ یک تابع یک اظهار نیست.)

اظهارات هیچ مقداری باز نمی‌گردانند. بنابراین، نمی‌توانید یک اظهار `let` را به یک متغیر دیگر اختصاص دهید، همانطور که کد زیر سعی دارد انجام دهد؛ شما با یک خطا روبرو خواهید شد:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/src/main.rs}}
```

وقتی این برنامه را اجرا کنید، خطایی که دریافت خواهید کرد به شکل زیر خواهد بود:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-19-statements-vs-expressions/output.txt}}
```

اظهار `let y = 6` هیچ مقداری باز نمی‌گرداند، بنابراین چیزی برای اتصال به `x` وجود ندارد. این با آنچه در زبان‌های دیگر، مانند C و Ruby رخ می‌دهد، متفاوت است، جایی که تخصیص مقدار باز می‌گرداند. در آن زبان‌ها، می‌توانید `x = y = 6` بنویسید و هر دو `x` و `y` مقدار `6` را داشته باشند؛ این حالت در راست وجود ندارد.

عبارات به یک مقدار ارزیابی می‌شوند و بیشتر بقیه کدی که در راست می‌نویسید را تشکیل می‌دهند. به عنوان مثال یک عملیات ریاضی، مانند `5 + 6`، که یک عبارت است که به مقدار `11` ارزیابی می‌شود. عبارات می‌توانند بخشی از اظهارات باشند: در لیستینگ ۳-۱، مقدار `6` در اظهار `let y = 6;` یک عبارت است که به مقدار `6` ارزیابی می‌شود. فراخوانی یک تابع یک عبارت است. فراخوانی یک ماکرو یک عبارت است. یک بلوک جدید از دامنه که با کروشه‌های باز و بسته ایجاد شده است نیز یک عبارت است، برای مثال:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-20-blocks-are-expressions/src/main.rs}}
```

این عبارت:

```rust,ignore
{
    let x = 3;
    x + 1
}
```

یک بلوک است که در این مورد به مقدار `4` ارزیابی می‌شود. آن مقدار به عنوان بخشی از اظهار `let` به `y` متصل می‌شود. توجه داشته باشید که خط `x + 1` در انتها یک نقطه ویرگول ندارد، که برخلاف اکثر خطوطی است که تاکنون دیده‌اید. عبارات شامل نقطه ویرگول انتهایی نمی‌شوند. اگر به انتهای یک عبارت یک نقطه ویرگول اضافه کنید، آن را به یک اظهار تبدیل می‌کنید و دیگر مقداری باز نمی‌گرداند. این نکته را در ذهن داشته باشید زیرا در ادامه به بررسی مقادیر بازگشتی توابع و عبارات می‌پردازیم.

### توابع با مقادیر بازگشتی

توابع می‌توانند مقادیری را به کدی که آنها را فراخوانی کرده است بازگردانند. ما به مقادیر بازگشتی نام نمی‌دهیم، اما باید نوع آنها را بعد از یک فلش (`->`) اعلام کنیم. در راست، مقدار بازگشتی تابع معادل مقدار عبارت نهایی در بلوک بدنه تابع است. شما می‌توانید با استفاده از کلمه کلیدی `return` و مشخص کردن یک مقدار، زودتر از یک تابع بازگردید، اما بیشتر توابع به طور ضمنی مقدار آخرین عبارت را بازمی‌گردانند. در اینجا یک مثال از یک تابع که مقدار بازمی‌گرداند آورده شده است:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/src/main.rs}}
```

هیچ فراخوانی تابع، ماکرو یا حتی اظهار `let` در تابع `five` وجود ندارد—فقط عدد `5` به تنهایی. این یک تابع کاملاً معتبر در راست است. توجه کنید که نوع مقدار بازگشتی تابع نیز به صورت `-> i32` مشخص شده است. این کد را اجرا کنید؛ خروجی باید به صورت زیر باشد:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-21-function-return-values/output.txt}}
```

عدد `5` در `five` مقدار بازگشتی تابع است، به همین دلیل نوع بازگشتی `i32` است. بیایید این موضوع را با جزئیات بیشتری بررسی کنیم. دو نکته مهم وجود دارد: اول، خط `let x = five();` نشان می‌دهد که ما از مقدار بازگشتی یک تابع برای مقداردهی یک متغیر استفاده می‌کنیم. چون تابع `five` مقدار `5` را بازمی‌گرداند، این خط مشابه خط زیر است:

```rust
let x = 5;
```

دوم، تابع `five` هیچ پارامتری ندارد و نوع مقدار بازگشتی را تعریف می‌کند، اما بدنه تابع یک عدد تنها `5` بدون نقطه ویرگول است زیرا این یک عبارت است که مقدار آن را می‌خواهیم بازگردانیم.

بیایید به مثال دیگری نگاه کنیم:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-22-function-parameter-and-return/src/main.rs}}
```

اجرای این کد مقدار `The value of x is: 6` را چاپ خواهد کرد. اما اگر یک نقطه ویرگول به انتهای خط حاوی `x + 1` اضافه کنیم و آن را از یک عبارت به یک اظهار تبدیل کنیم، با خطا مواجه خواهیم شد:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/src/main.rs}}
```

کامپایل این کد خطایی به شرح زیر تولید می‌کند:

```console
{{#include ../listings/ch03-common-programming-concepts/no-listing-23-statements-dont-return-values/output.txt}}
```

پیام خطای اصلی، `mismatched types`، مسئله اصلی این کد را آشکار می‌کند. تعریف تابع `plus_one` می‌گوید که این تابع یک `i32` بازمی‌گرداند، اما اظهارات به یک مقدار ارزیابی نمی‌شوند، که با `()`، نوع واحد، نشان داده می‌شود. بنابراین، چیزی بازگردانده نمی‌شود که با تعریف تابع تناقض دارد و باعث ایجاد خطا می‌شود. در این خروجی، راست پیامی ارائه می‌دهد تا شاید به حل این مشکل کمک کند: پیشنهاد حذف نقطه ویرگول، که این خطا را برطرف می‌کند.
