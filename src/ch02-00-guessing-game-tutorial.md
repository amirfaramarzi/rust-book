# برنامه‌نویسی یک بازی حدس زدن

بیایید با کار روی یک پروژه عملی با هم به دنیای Rust وارد شویم! این فصل با نشان دادن نحوه استفاده از مفاهیم رایج Rust در یک برنامه واقعی، شما را با آن‌ها آشنا می‌کند. درباره `let`، `match`، متدها، توابع مرتبط (associated functions)، جعبه‌ها (crates)ی خارجی و موارد دیگر خواهید آموخت! در فصل‌های بعدی، این ایده‌ها را به طور مفصل بررسی خواهیم کرد. در این فصل، فقط اصول اولیه را تمرین می‌کنید.

ما یک مسئله کلاسیک برنامه‌نویسی برای مبتدیان را پیاده‌سازی خواهیم کرد: یک بازی حدس زدن. این بازی به این صورت عمل می‌کند: برنامه یک عدد صحیح تصادفی بین 1 تا 100 تولید می‌کند. سپس از بازیکن می‌خواهد که یک حدس وارد کند. پس از وارد کردن حدس، برنامه مشخص می‌کند که آیا حدس خیلی پایین است یا خیلی بالا. اگر حدس درست باشد، برنامه یک پیام تبریک چاپ می‌کند و از بازی خارج می‌شود.

## راه‌اندازی یک پروژه جدید

برای راه‌اندازی یک پروژه جدید، به دایرکتوری _projects_ که در فصل 1 ایجاد کردید بروید و یک پروژه جدید با استفاده از Cargo ایجاد کنید، به این صورت:

```console
$ cargo new guessing_game
$ cd guessing_game
```

دستور اول، `cargo new`، نام پروژه (`guessing_game`) را به عنوان آرگومان اول می‌گیرد. دستور دوم به دایرکتوری پروژه جدید منتقل می‌شود.

فایل _Cargo.toml_ تولیدشده را مشاهده کنید:

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/Cargo.toml}}
```

همان‌طور که در فصل 1 دیدید، `cargo new` یک برنامه "Hello, world!" برای شما تولید می‌کند. فایل _src/main.rs_ را بررسی کنید:

<span class="filename">Filename: src/main.rs</span>

```rust
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/src/main.rs}}
```

حالا این برنامه "Hello, world!" را کامپایل کرده و در همان مرحله با استفاده از دستور `cargo run` اجرا کنید:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-01-cargo-new/output.txt}}
```

دستور `run` زمانی که نیاز دارید به سرعت روی یک پروژه تکرار کنید مفید است، همان‌طور که در این بازی انجام خواهیم داد، و به سرعت هر مرحله را قبل از ادامه به مرحله بعدی آزمایش می‌کنیم.

فایل _src/main.rs_ را دوباره باز کنید. شما تمام کد را در این فایل خواهید نوشت.

## پردازش یک حدس

اولین بخش از برنامه بازی حدس زدن از کاربر درخواست ورودی می‌کند، آن ورودی را پردازش می‌کند و بررسی می‌کند که ورودی در قالب مورد انتظار باشد. برای شروع، به بازیکن اجازه می‌دهیم یک حدس وارد کند. کد موجود در لیستینگ 2-1 را در فایل _src/main.rs_ وارد کنید.

<Listing number="2-1" file-name="src/main.rs" caption="کدی که یک حدس از کاربر دریافت کرده و آن را چاپ می‌کند">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:all}}
```

</Listing>

این کد اطلاعات زیادی دارد، پس بیایید خط به خط آن را بررسی کنیم. برای گرفتن ورودی کاربر و سپس چاپ نتیجه به‌عنوان خروجی، نیاز داریم که کتابخانه ورودی/خروجی `io` را به دامنه بیاوریم. کتابخانه `io` از کتابخانه استاندارد که با نام `std` شناخته می‌شود، می‌آید:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:io}}
```

به‌طور پیش‌فرض، Rust مجموعه‌ای از آیتم‌ها را که در کتابخانه استاندارد تعریف شده‌اند به دامنه هر برنامه وارد می‌کند. این مجموعه _prelude_ نامیده می‌شود و می‌توانید همه چیز در آن را [در مستندات کتابخانه استاندارد][prelude] ببینید.

اگر نوعی که می‌خواهید استفاده کنید در prelude نباشد، باید آن نوع را به‌طور صریح با یک دستور `use` به دامنه بیاورید. استفاده از کتابخانه `std::io` به شما ویژگی‌های مفیدی مانند امکان پذیرش ورودی کاربر می‌دهد.

همان‌طور که در فصل 1 دیدید، تابع `main` نقطه ورود به برنامه است:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:main}}
```

نحو `fn` یک تابع جدید را اعلام می‌کند؛ پرانتزها `()` نشان می‌دهند که هیچ پارامتری وجود ندارد و کروشه باز `{` بدنه تابع را شروع می‌کند.

همچنین در فصل 1 آموختید که `println!` یک ماکرو است که یک رشته را به صفحه چاپ می‌کند:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print}}
```

این کد یک پیغام اعلام می‌کند که بازی چیست و از کاربر درخواست ورودی می‌کند.

### ذخیره مقادیر با متغیرها

سپس، یک _متغیر_ ایجاد می‌کنیم تا ورودی کاربر را ذخیره کند، مانند این:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:string}}
```

حالا برنامه جالب‌تر می‌شود! در این خط کوچک چیزهای زیادی در حال اتفاق است. ما از دستور `let` برای ایجاد متغیر استفاده می‌کنیم. در اینجا یک مثال دیگر آورده شده است:

```rust,ignore
let apples = 5;
```

این خط یک متغیر جدید به نام `apples` ایجاد می‌کند و آن را به مقدار 5 متصل می‌کند. در Rust، متغیرها به‌طور پیش‌فرض غیرقابل‌تغییر هستند، به این معنا که پس از اختصاص مقدار به متغیر، مقدار تغییر نخواهد کرد. این مفهوم را به‌طور مفصل در بخش [“متغیرها و تغییرپذیری”][variables-and-mutability]<!-- ignore --> در فصل 3 بررسی خواهیم کرد. برای متغیری که تغییرپذیر باشد، `mut` را قبل از نام متغیر اضافه می‌کنیم:

```rust,ignore
let apples = 5; // immutable
let mut bananas = 5; // mutable
```

> نکته: نحو `//` یک نظر (comment) را آغاز می‌کند که تا انتهای خط ادامه دارد. Rust همه چیز در نظرات را نادیده می‌گیرد. نظرات را در [فصل 3][comments]<!-- ignore --> با جزئیات بیشتری بررسی خواهیم کرد.

بازگشت به برنامه بازی حدس زدن: اکنون می‌دانید که `let mut guess` یک متغیر تغییرپذیر به نام `guess` معرفی می‌کند. علامت مساوی (`=`) به Rust می‌گوید که می‌خواهیم چیزی را به این متغیر متصل کنیم. در سمت راست علامت مساوی، مقداری قرار دارد که `guess` به آن متصل می‌شود، که نتیجه فراخوانی `String::new` است، یک تابع که یک نمونه جدید از نوع `String` بازمی‌گرداند. [`String`][string]<!-- ignore --> یک نوع رشته‌ای ارائه‌شده توسط کتابخانه استاندارد است که بخشی از متن قابل رشد و با رمزگذاری UTF-8 است.

نحو `::` در خط `::new` نشان می‌دهد که `new` یک تابع مرتبط با نوع `String` است. یک _تابع مرتبط_ تابعی است که روی یک نوع پیاده‌سازی شده است، در اینجا `String`. این تابع `new` یک رشته جدید و خالی ایجاد می‌کند. شما در بسیاری از انواع یک تابع `new` پیدا خواهید کرد، زیرا این نام معمولاً برای تابعی که یک مقدار جدید از یک نوع خاص ایجاد می‌کند استفاده می‌شود.

در مجموع، خط `let mut guess = String::new();` یک متغیر تغییرپذیر ایجاد کرده است که در حال حاضر به یک نمونه جدید و خالی از `String` متصل شده است. خوب!

### دریافت ورودی کاربر

به یاد آورید که با `use std::io;` در اولین خط برنامه، قابلیت ورودی/خروجی را از کتابخانه استاندارد اضافه کردیم. اکنون تابع `stdin` را از ماژول `io` فراخوانی می‌کنیم که به ما امکان مدیریت ورودی کاربر را می‌دهد:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:read}}
```

اگر کتابخانه `io` را با `use std::io;` در ابتدای برنامه وارد نکرده بودیم، همچنان می‌توانستیم تابع را با نوشتن `std::io::stdin` فراخوانی کنیم. تابع `stdin` یک نمونه از نوع [`std::io::Stdin`][iostdin]<!-- ignore --> بازمی‌گرداند که یک نوع برای مدیریت ورودی استاندارد ترمینال شما است.

در خط بعدی، متد `.read_line(&mut guess)` را روی handle ورودی استاندارد فراخوانی می‌کنیم تا ورودی کاربر را دریافت کنیم. همچنین `&mut guess` را به‌عنوان آرگومان به `read_line` ارسال می‌کنیم تا به آن بگوییم ورودی کاربر را در چه رشته‌ای ذخیره کند. وظیفه کامل `read_line` این است که هر چیزی را که کاربر در ورودی استاندارد تایپ می‌کند به رشته‌ای اضافه کند (بدون بازنویسی محتوای آن)، بنابراین این رشته را به‌عنوان آرگومان ارسال می‌کنیم. آرگومان رشته باید تغییرپذیر باشد تا متد بتواند محتوای رشته را تغییر دهد.

علامت `&` نشان می‌دهد که این آرگومان یک _ارجاع_ است، که به شما راهی می‌دهد تا به چندین بخش از کد اجازه دهید به یک قطعه داده دسترسی داشته باشند بدون اینکه نیاز به کپی کردن آن داده در حافظه چندین بار داشته باشید. ارجاعات یک ویژگی پیچیده هستند و یکی از مزایای اصلی Rust این است که استفاده از ارجاعات ایمن و آسان است. نیازی نیست جزئیات زیادی درباره آن بدانید تا این برنامه را کامل کنید. فعلاً، تنها چیزی که باید بدانید این است که، مانند متغیرها، ارجاعات به‌طور پیش‌فرض غیرقابل تغییر هستند. بنابراین، باید `&mut guess` بنویسید به‌جای `&guess` تا آن را تغییرپذیر کنید. (فصل 4 ارجاعات را به‌طور کامل توضیح خواهد داد.)

### مدیریت خطای احتمالی با `Result`

ما همچنان روی همین خط کد کار می‌کنیم. اکنون در حال بحث درباره خط سوم هستیم، اما توجه داشته باشید که این هنوز بخشی از یک خط منطقی از کد است. قسمت بعدی این متد است:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:expect}}
```

ما می‌توانستیم این کد را به این صورت بنویسیم:

```rust,ignore
io::stdin().read_line(&mut guess).expect("Failed to read line");
```

با این حال، یک خط طولانی خواندن آن را دشوار می‌کند، بنابراین بهتر است آن را تقسیم کنیم. اغلب توصیه می‌شود یک خط جدید و فضای سفید معرفی کنید تا خطوط طولانی را هنگام فراخوانی متدی با نحو `.method_name()` تقسیم کنید. حالا بیایید ببینیم این خط چه می‌کند.

همان‌طور که قبلاً ذکر شد، `read_line` هر چیزی که کاربر وارد می‌کند را در رشته‌ای که به آن ارسال می‌کنیم قرار می‌دهد، اما همچنین یک مقدار `Result` بازمی‌گرداند. [`Result`][result]<!-- ignore --> یک [_enumeration_][enums]<!-- ignore --> است که اغلب به عنوان _enum_ نامیده می‌شود و نوعی است که می‌تواند در یکی از چندین حالت ممکن باشد. ما هر حالت ممکن را یک _متغیر_ (variant) می‌نامیم.

[فصل 6][enums]<!-- ignore --> به جزئیات بیشتری در مورد enumها خواهد پرداخت. هدف از انواع `Result` رمزگذاری اطلاعات مدیریت خطا است.

متغیرهای `Result` شامل `Ok` و `Err` هستند. متغیر `Ok` نشان می‌دهد که عملیات موفقیت‌آمیز بوده و مقداری که با موفقیت تولید شده است را در خود دارد. متغیر `Err` به معنای این است که عملیات شکست خورده و اطلاعاتی درباره چگونگی یا دلیل شکست عملیات در خود دارد.

مقادیر نوع `Result`، مانند مقادیر هر نوع دیگری، متدهایی تعریف‌شده بر روی خود دارند. یک نمونه از `Result` یک [متد `expect`][expect]<!-- ignore --> دارد که می‌توانید آن را فراخوانی کنید. اگر این نمونه از `Result` یک مقدار `Err` باشد، `expect` باعث می‌شود برنامه متوقف شده و پیغام خطایی که به‌عنوان آرگومان به `expect` پاس داده‌اید را نمایش دهد. اگر متد `read_line` یک `Err` بازگرداند، احتمالاً به دلیل خطایی از سیستم‌عامل زیربنایی است. اگر این نمونه از `Result` یک مقدار `Ok` باشد، `expect` مقدار بازگشتی که `Ok` در خود دارد را می‌گیرد و فقط آن مقدار را بازمی‌گرداند تا بتوانید از آن استفاده کنید. در این مورد، آن مقدار تعداد بایت‌های ورودی کاربر است.

اگر `expect` را فراخوانی نکنید، برنامه کامپایل می‌شود، اما هشداری دریافت خواهید کرد:

```console
{{#include ../listings/ch02-guessing-game-tutorial/no-listing-02-without-expect/output.txt}}
```

Rust هشدار می‌دهد که از مقدار `Result` بازگشتی از `read_line` استفاده نکرده‌اید، که نشان می‌دهد برنامه یک خطای ممکن را مدیریت نکرده است.

روش درست برای جلوگیری از هشدار این است که واقعاً کد مدیریت خطا بنویسید، اما در مورد ما فقط می‌خواهیم وقتی مشکلی پیش آمد این برنامه متوقف شود، بنابراین می‌توانیم از `expect` استفاده کنیم. درباره بازیابی از خطاها در [فصل 9][recover]<!-- ignore --> خواهید آموخت.

### چاپ مقادیر با جای‌نگهدارهای `println!`

علاوه بر کروشه بسته، فقط یک خط دیگر برای بحث در کدی که تاکنون نوشته‌ایم باقی مانده است:

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-01/src/main.rs:print_guess}}
```

این خط رشته‌ای را که اکنون ورودی کاربر را در خود دارد چاپ می‌کند. مجموعه `{}` از کروشه‌های باز و بسته یک جای‌نگهدار است: به `{}` به‌عنوان پنجه‌های کوچک خرچنگی فکر کنید که یک مقدار را در جای خود نگه می‌دارند. هنگام چاپ مقدار یک متغیر، نام متغیر می‌تواند داخل کروشه‌ها قرار گیرد. هنگام چاپ نتیجه ارزیابی یک عبارت، کروشه‌های باز و بسته خالی را در رشته فرمت قرار دهید، سپس رشته فرمت را با لیستی از عبارات جداشده با کاما دنبال کنید تا در هر جای‌نگهدار خالی به همان ترتیب چاپ شوند. چاپ یک متغیر و نتیجه یک عبارت در یک فراخوانی `println!` به این صورت خواهد بود:

```rust
let x = 5;
let y = 10;

println!("x = {x} and y + 2 = {}", y + 2);
```

این کد `x = 5 and y + 2 = 12` را چاپ می‌کند.

### آزمایش بخش اول

بیایید بخش اول بازی حدس زدن را آزمایش کنیم. با استفاده از دستور `cargo run` آن را اجرا کنید:

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 6.44s
     Running `target/debug/guessing_game`
Guess the number!
Please input your guess.
6
You guessed: 6
```

در این مرحله، بخش اول بازی تمام شده است: ما ورودی را از صفحه‌کلید می‌گیریم و سپس آن را چاپ می‌کنیم.

## تولید یک عدد مخفی

در مرحله بعد، باید یک عدد مخفی تولید کنیم که کاربر سعی خواهد کرد آن را حدس بزند. عدد مخفی باید هر بار متفاوت باشد تا بازی بارها قابل بازی و لذت‌بخش باشد. از یک عدد تصادفی بین 1 تا 100 استفاده می‌کنیم تا بازی خیلی سخت نباشد. Rust هنوز قابلیت تولید اعداد تصادفی را در کتابخانه استاندارد خود ندارد. با این حال، تیم Rust یک [crate `rand`][randcrate] با این قابلیت ارائه می‌دهد.

### استفاده از یک crate برای دسترسی به قابلیت‌های بیشتر

به یاد داشته باشید که یک crate مجموعه‌ای از فایل‌های کد منبع Rust است. پروژه‌ای که ما در حال ساخت آن هستیم یک _crate دودویی_ است که یک فایل اجرایی است. crate `rand` یک _crate کتابخانه‌ای_ است که حاوی کدی است که قرار است در برنامه‌های دیگر استفاده شود و به تنهایی قابل اجرا نیست.

هماهنگی Cargo با جعبه‌ها (crates)ی خارجی یکی از نقاط قوت آن است. قبل از اینکه بتوانیم کدی بنویسیم که از `rand` استفاده کند، باید فایل _Cargo.toml_ را تغییر دهیم تا crate `rand` را به عنوان وابستگی اضافه کنیم. اکنون آن فایل را باز کنید و خط زیر را به انتهای آن، زیر بخش `[dependencies]` که Cargo برای شما ایجاد کرده است، اضافه کنید. مطمئن شوید که `rand` را دقیقاً همان‌طور که در اینجا آمده است با این شماره نسخه مشخص کنید، وگرنه مثال‌های کد در این آموزش ممکن است کار نکنند:

<span class="filename">Filename: Cargo.toml</span>

```toml
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-02/Cargo.toml:8:}}
```

در فایل _Cargo.toml_، هر چیزی که بعد از یک سرآیند بیاید بخشی از آن بخش است و تا زمانی که بخش دیگری شروع نشود ادامه می‌یابد. در `[dependencies]` به Cargo می‌گویید پروژه شما به کدام جعبه‌ها (crates)ی خارجی وابسته است و کدام نسخه از آن جعبه‌ها (crates) را نیاز دارید. در این مورد، ما crate `rand` را با مشخص‌کننده نسخه `0.8.5` مشخص می‌کنیم. Cargo [نسخه‌بندی معنایی][semver]<!-- ignore --> (گاهی اوقات _SemVer_ نامیده می‌شود) را درک می‌کند، که یک استاندارد برای نوشتن شماره نسخه‌ها است. مشخص‌کننده `0.8.5` در واقع مخفف `^0.8.5` است که به این معناست که هر نسخه‌ای که حداقل 0.8.5 باشد ولی کمتر از 0.9.0 باشد.

Cargo این نسخه‌ها را دارای API عمومی سازگار با نسخه 0.8.5 در نظر می‌گیرد و این مشخصه تضمین می‌کند که آخرین نسخه patch را دریافت خواهید کرد که همچنان با کد موجود در این فصل کامپایل می‌شود. هیچ تضمینی وجود ندارد که نسخه 0.9.0 یا بالاتر همان API را داشته باشد که مثال‌های زیر استفاده می‌کنند.

اکنون، بدون تغییر هیچ کدی، بیایید پروژه را بسازیم، همان‌طور که در لیستینگ 2-2 نشان داده شده است.

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-02/
rm Cargo.lock
cargo clean
cargo build -->

<Listing number="2-2" caption="خروجی اجرای `cargo build` پس از افزودن crate rand به عنوان وابستگی">


```console
$ cargo build
    Updating crates.io index
     Locking 16 packages to latest compatible versions
      Adding wasi v0.11.0+wasi-snapshot-preview1 (latest: v0.13.3+wasi-0.2.2)
      Adding zerocopy v0.7.35 (latest: v0.8.9)
      Adding zerocopy-derive v0.7.35 (latest: v0.8.9)
  Downloaded syn v2.0.87
  Downloaded 1 crate (278.1 KB) in 0.16s
   Compiling proc-macro2 v1.0.89
   Compiling unicode-ident v1.0.13
   Compiling libc v0.2.161
   Compiling cfg-if v1.0.0
   Compiling byteorder v1.5.0
   Compiling getrandom v0.2.15
   Compiling rand_core v0.6.4
   Compiling quote v1.0.37
   Compiling syn v2.0.87
   Compiling zerocopy-derive v0.7.35
   Compiling zerocopy v0.7.35
   Compiling ppv-lite86 v0.2.20
   Compiling rand_chacha v0.3.1
   Compiling rand v0.8.5
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.69s
```

</Listing>

ممکن است نسخه‌های متفاوتی را ببینید (اما همه آن‌ها با کد سازگار خواهند بود، به لطف SemVer!) و خطوط متفاوتی (بسته به سیستم‌عامل) داشته باشید، و این خطوط ممکن است به ترتیب متفاوتی ظاهر شوند.

وقتی یک وابستگی خارجی اضافه می‌کنیم، Cargo جدیدترین نسخه‌های هر چیزی که آن وابستگی نیاز دارد را از _رجیستری_ دریافت می‌کند، که یک کپی از داده‌های [Crates.io][cratesio] است. Crates.io جایی است که افراد در اکوسیستم Rust پروژه‌های منبع‌باز Rust خود را برای استفاده دیگران ارسال می‌کنند.

پس از به‌روزرسانی رجیستری، Cargo بخش `[dependencies]` را بررسی می‌کند و هر crateی را که در لیست نیست و هنوز دانلود نشده است دانلود می‌کند. در این مورد، اگرچه ما فقط `rand` را به‌عنوان یک وابستگی لیست کرده‌ایم، Cargo سایر جعبه‌ها (crates)یی را که `rand` برای کارکردن به آن‌ها وابسته است نیز دریافت کرده است. پس از دانلود جعبه‌ها (crates)، Rust آن‌ها را کامپایل می‌کند و سپس پروژه را با وابستگی‌های موجود کامپایل می‌کند.

اگر بلافاصله دوباره دستور `cargo build` را اجرا کنید بدون اینکه هیچ تغییری ایجاد کرده باشید، خروجی‌ای به‌جز خط `Finished` دریافت نخواهید کرد. Cargo می‌داند که قبلاً وابستگی‌ها را دانلود و کامپایل کرده است، و شما هیچ تغییری در فایل _Cargo.toml_ خود نداده‌اید. Cargo همچنین می‌داند که شما هیچ تغییری در کد خود نداده‌اید، بنابراین آن را هم دوباره کامپایل نمی‌کند. وقتی کاری برای انجام دادن وجود ندارد، فقط خارج می‌شود.

اگر فایل _src/main.rs_ را باز کنید، یک تغییر جزئی در آن ایجاد کنید، و سپس آن را ذخیره کرده و دوباره بسازید، فقط دو خط خروجی خواهید دید:

```console
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
```

این خطوط نشان می‌دهند که Cargo فقط با تغییر کوچک شما در فایل _src/main.rs_ بیلد را به‌روزرسانی کرده است. وابستگی‌های شما تغییری نکرده‌اند، بنابراین Cargo می‌داند که می‌تواند از آنچه قبلاً دانلود و کامپایل کرده است استفاده مجدد کند.

#### اطمینان از بیلدهای قابل بازتولید با فایل _Cargo.lock_

Cargo مکانیزمی دارد که اطمینان می‌دهد شما یا هر کس دیگری بتوانید هر بار که کد خود را بیلد می‌کنید، همان نتیجه را دریافت کنید: Cargo تنها از نسخه‌هایی از وابستگی‌ها که مشخص کرده‌اید استفاده می‌کند، مگر اینکه خلاف آن را اعلام کنید. برای مثال، فرض کنید هفته آینده نسخه 0.8.6 از crate `rand` منتشر می‌شود و آن نسخه شامل یک رفع باگ مهم است، اما همچنین شامل یک برگشت (regression) است که کد شما را خراب می‌کند. برای مدیریت این موضوع، Rust فایل _Cargo.lock_ را در اولین باری که `cargo build` را اجرا می‌کنید ایجاد می‌کند، بنابراین اکنون این فایل در دایرکتوری _guessing_game_ وجود دارد.

وقتی برای اولین بار پروژه‌ای را بیلد می‌کنید، Cargo همه نسخه‌های وابستگی‌هایی که با معیارها تطابق دارند را پیدا می‌کند و سپس آن‌ها را به فایل _Cargo.lock_ می‌نویسد. وقتی در آینده پروژه خود را بیلد می‌کنید، Cargo می‌بیند که فایل _Cargo.lock_ وجود دارد و از نسخه‌های مشخص‌شده در آن استفاده می‌کند، به جای اینکه تمام کار پیدا کردن نسخه‌ها را دوباره انجام دهد. این کار به شما اجازه می‌دهد که به‌طور خودکار یک بیلد قابل بازتولید داشته باشید. به عبارت دیگر، پروژه شما در نسخه 0.8.5 باقی خواهد ماند تا زمانی که به صورت صریح آن را به‌روزرسانی کنید، به لطف فایل _Cargo.lock_. چون فایل _Cargo.lock_ برای بیلدهای قابل بازتولید مهم است، معمولاً همراه با بقیه کد پروژه در سیستم کنترل نسخه (source control) ذخیره می‌شود.

#### به‌روزرسانی یک crate برای دریافت نسخه جدید

وقتی _می‌خواهید_ یک crate را به‌روزرسانی کنید، Cargo دستور `update` را فراهم می‌کند که فایل _Cargo.lock_ را نادیده می‌گیرد و تمام نسخه‌های جدیدی که با مشخصات شما در فایل _Cargo.toml_ سازگار هستند را پیدا می‌کند. سپس Cargo آن نسخه‌ها را به فایل _Cargo.lock_ می‌نویسد. در این مورد، Cargo تنها به دنبال نسخه‌هایی می‌گردد که بالاتر از 0.8.5 و کمتر از 0.9.0 باشند. اگر crate `rand` دو نسخه جدید 0.8.6 و 0.9.0 را منتشر کرده باشد، با اجرای `cargo update` چنین چیزی را خواهید دید:

```console
$ cargo update
    Updating crates.io index
    Updating rand v0.8.5 -> v0.8.6
```

Cargo نسخه 0.9.0 را نادیده می‌گیرد. در این مرحله، شما همچنین تغییری در فایل _Cargo.lock_ مشاهده می‌کنید که نشان می‌دهد نسخه crate `rand` که اکنون استفاده می‌کنید 0.8.6 است. برای استفاده از نسخه 0.9.0 `rand` یا هر نسخه‌ای در سری 0.9._x_، باید فایل _Cargo.toml_ را به این شکل تغییر دهید:

```toml
[dependencies]
rand = "0.9.0"
```

دفعه بعد که `cargo build` را اجرا کنید، Cargo رجیستری جعبه‌ها (crates)ی موجود را به‌روزرسانی می‌کند و نیازمندی‌های شما برای `rand` را بر اساس نسخه جدیدی که مشخص کرده‌اید ارزیابی می‌کند.

چیزهای بیشتری درباره [Cargo][doccargo]<!-- ignore --> و [اکوسیستم آن][doccratesio]<!-- ignore --> وجود دارد که در فصل 14 بحث خواهیم کرد، اما فعلاً این تمام چیزی است که باید بدانید. Cargo استفاده از کتابخانه‌ها را بسیار آسان می‌کند، بنابراین Rustaceans می‌توانند پروژه‌های کوچک‌تری بنویسند که از تعدادی بسته تشکیل شده‌اند.

### تولید یک عدد تصادفی

بیایید استفاده از `rand` را برای تولید یک عدد برای حدس زدن شروع کنیم. مرحله بعد به‌روزرسانی فایل _src/main.rs_ است، همان‌طور که در لیستینگ 2-3 نشان داده شده است.

<Listing number="2-3" file-name="src/main.rs" caption="اضافه کردن کدی برای تولید یک عدد تصادفی">


```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-03/src/main.rs:all}}
```

</Listing>

ابتدا خط `use rand::Rng;` را اضافه می‌کنیم. صفت (trait) `Rng` متدهایی را تعریف می‌کند که تولیدکنندگان اعداد تصادفی پیاده‌سازی می‌کنند، و این صفت باید در دامنه باشد تا بتوانیم از آن متدها استفاده کنیم. فصل 10 به‌طور مفصل به بررسی صفت‌ها خواهد پرداخت.

سپس دو خط در وسط اضافه می‌کنیم. در خط اول، تابع `rand::thread_rng` را فراخوانی می‌کنیم که تولیدکننده اعداد تصادفی خاصی را که می‌خواهیم استفاده کنیم به ما می‌دهد: تولیدکننده‌ای که محلی برای نخ فعلی اجرا است و توسط سیستم‌عامل seed می‌شود. سپس متد `gen_range` را روی تولیدکننده اعداد تصادفی فراخوانی می‌کنیم. این متد توسط صفت `Rng` که با دستور `use rand::Rng;` وارد دامنه کردیم، تعریف شده است. متد `gen_range` یک عبارت بازه‌ای را به‌عنوان آرگومان می‌گیرد و یک عدد تصادفی در آن بازه تولید می‌کند. نوع عبارت بازه‌ای که در اینجا استفاده می‌کنیم به صورت `start..=end` است و شامل حد پایین و بالا می‌شود، بنابراین باید `1..=100` را مشخص کنیم تا عددی بین 1 تا 100 درخواست کنیم.

> نکته: شما نمی‌توانید به‌طور پیش‌فرض بدانید که کدام صفت‌ها را باید استفاده کنید و کدام متدها و توابع را از یک crate فراخوانی کنید، بنابراین هر crate دارای مستنداتی با دستورالعمل‌هایی برای استفاده از آن است. ویژگی جالب دیگر Cargo این است که اجرای دستور `cargo doc --open` مستندات ارائه‌شده توسط تمام وابستگی‌های شما را به‌صورت محلی می‌سازد و در مرورگر شما باز می‌کند. اگر به دیگر قابلیت‌های crate `rand` علاقه‌مند هستید، برای مثال دستور `cargo doc --open` را اجرا کنید و روی `rand` در نوار کناری سمت چپ کلیک کنید.

خط جدید دوم عدد مخفی را چاپ می‌کند. این خط در حین توسعه برنامه برای آزمایش آن مفید است، اما در نسخه نهایی آن را حذف خواهیم کرد. اگر برنامه به محض شروع پاسخ را چاپ کند، خیلی بازی هیجان‌انگیزی نخواهد بود!

برنامه را چند بار اجرا کنید:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-03/
cargo run
4
cargo run
5
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 7
Please input your guess.
4
You guessed: 4

$ cargo run
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 83
Please input your guess.
5
You guessed: 5
```

شما باید اعداد تصادفی متفاوتی دریافت کنید و تمام آن‌ها باید بین 1 تا 100 باشند. عالی!

## مقایسه حدس با عدد مخفی

حالا که ورودی کاربر و یک عدد تصادفی داریم، می‌توانیم آن‌ها را مقایسه کنیم. این مرحله در لیستینگ 2-4 نشان داده شده است. توجه داشته باشید که این کد هنوز کامپایل نخواهد شد، همان‌طور که توضیح خواهیم داد.

<Listing number="2-4" file-name="src/main.rs" caption="مدیریت مقادیر بازگشتی ممکن از مقایسه دو عدد">

```rust,ignore,does_not_compile
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-04/src/main.rs:here}}
```

</Listing>

ابتدا یک دستور `use` دیگر اضافه می‌کنیم تا نوعی به نام `std::cmp::Ordering` را از کتابخانه استاندارد وارد دامنه کنیم. نوع `Ordering` یک enum دیگر است و دارای متغیرهای `Less`، `Greater` و `Equal` است. این‌ها سه نتیجه ممکن هنگام مقایسه دو مقدار هستند.

سپس پنج خط جدید در انتهای کد اضافه می‌کنیم که از نوع `Ordering` استفاده می‌کنند. متد `cmp` دو مقدار را مقایسه می‌کند و می‌تواند روی هر چیزی که قابل مقایسه باشد فراخوانی شود. این متد یک ارجاع به مقداری که می‌خواهید مقایسه کنید می‌گیرد: در اینجا مقایسه بین `guess` و `secret_number` است. سپس یکی از متغیرهای enum `Ordering` که با دستور `use` به دامنه آوردیم را بازمی‌گرداند. از یک عبارت [`match`][match]<!-- ignore --> برای تصمیم‌گیری در مورد اقدام بعدی بر اساس اینکه کدام متغیر `Ordering` از فراخوانی `cmp` با مقادیر `guess` و `secret_number` بازگشته است استفاده می‌کنیم.

یک عبارت `match` از _شاخه‌ها (arms)_ تشکیل شده است. یک شاخه شامل یک _الگو_ برای مطابقت است و کدی که باید اجرا شود اگر مقدار داده‌شده به `match` با الگوی آن شاخه تطابق داشته باشد. Rust مقدار داده‌شده به `match` را گرفته و به ترتیب هر الگوی شاخه را بررسی می‌کند. الگوها و سازه `match` از ویژگی‌های قدرتمند Rust هستند: آن‌ها به شما اجازه می‌دهند موقعیت‌های مختلفی که کد شما ممکن است با آن‌ها روبرو شود را بیان کنید و اطمینان حاصل کنید که همه آن‌ها را مدیریت می‌کنید. این ویژگی‌ها به‌طور مفصل در فصل 6 و فصل 19 پوشش داده خواهند شد.

بیایید با یک مثال از عبارت `match` که در اینجا استفاده کرده‌ایم، آن را بررسی کنیم. فرض کنید کاربر 50 را حدس زده و عدد مخفی که این بار به‌طور تصادفی تولید شده 38 است.

وقتی کد 50 را با 38 مقایسه می‌کند، متد `cmp` مقدار `Ordering::Greater` را بازمی‌گرداند زیرا 50 بزرگ‌تر از 38 است. عبارت `match` مقدار `Ordering::Greater` را گرفته و شروع به بررسی هر الگوی شاخه می‌کند. به الگوی شاخه اول، `Ordering::Less` نگاه می‌کند و می‌بیند که مقدار `Ordering::Greater` با `Ordering::Less` تطابق ندارد، بنابراین کد موجود در آن شاخه را نادیده می‌گیرد و به شاخه بعدی می‌رود. الگوی شاخه بعدی `Ordering::Greater` است که با `Ordering::Greater` تطابق دارد! کد مرتبط با آن شاخه اجرا شده و عبارت `Too big!` را روی صفحه چاپ می‌کند. عبارت `match` پس از اولین تطابق موفقیت‌آمیز پایان می‌یابد، بنابراین در این سناریو به شاخه آخر نگاه نمی‌کند.

با این حال، کد موجود در لیستینگ 2-4 هنوز کامپایل نخواهد شد. بیایید آن را امتحان کنیم:

<!--
The error numbers in this output should be that of the code **WITHOUT** the
anchor or snip comments
-->

```console
{{#include ../listings/ch02-guessing-game-tutorial/listing-02-04/output.txt}}
```

هسته خطا بیان می‌کند که _انواع ناسازگار_ وجود دارند. Rust دارای یک سیستم نوع قوی و ایستا است. با این حال، همچنین دارای استنباط نوع است. وقتی `let mut guess = String::new()` نوشتیم، Rust توانست استنباط کند که `guess` باید یک `String` باشد و نیازی نبود که نوع را به‌صورت صریح بنویسیم. از طرف دیگر، `secret_number` یک نوع عددی است. چند نوع عددی در Rust می‌توانند مقداری بین 1 و 100 داشته باشند: `i32`، یک عدد 32 بیتی؛ `u32`، یک عدد بدون علامت 32 بیتی؛ `i64`، یک عدد 64 بیتی؛ و دیگران. مگر اینکه خلاف آن مشخص شده باشد، Rust به‌طور پیش‌فرض از `i32` استفاده می‌کند، که نوع `secret_number` است مگر اینکه اطلاعات نوع دیگری اضافه کنید که باعث شود Rust نوع عددی دیگری را استنباط کند. دلیل خطا این است که Rust نمی‌تواند یک رشته و یک نوع عددی را مقایسه کند.

در نهایت، می‌خواهیم `String` که برنامه به‌عنوان ورودی می‌خواند را به یک نوع عددی تبدیل کنیم تا بتوانیم آن را به‌صورت عددی با عدد مخفی مقایسه کنیم. این کار را با اضافه کردن این خط به بدنه تابع `main` انجام می‌دهیم:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/src/main.rs:here}}
```

خط موردنظر این است:

```rust,ignore
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

ما یک متغیر به نام `guess` ایجاد می‌کنیم. اما صبر کنید، آیا برنامه قبلاً یک متغیر به نام `guess` ندارد؟ دارد، اما Rust به‌طور مفیدی به ما اجازه می‌دهد مقدار قبلی `guess` را با یک مقدار جدید پوشش دهیم. _پوشش‌دهی_ به ما اجازه می‌دهد که از نام متغیر `guess` دوباره استفاده کنیم، به‌جای اینکه مجبور شویم دو متغیر منحصربه‌فرد مانند `guess_str` و `guess` ایجاد کنیم. این موضوع را در [فصل 3][shadowing]<!-- ignore --> با جزئیات بیشتری بررسی خواهیم کرد، اما فعلاً بدانید که این ویژگی اغلب زمانی استفاده می‌شود که بخواهید مقدار را از یک نوع به نوع دیگری تبدیل کنید.

ما این متغیر جدید را به عبارت `guess.trim().parse()` متصل می‌کنیم. `guess` در این عبارت به متغیر اصلی `guess` که ورودی به‌صورت رشته‌ای بود اشاره دارد. متد `trim` روی یک نمونه `String` تمام فضای سفید در ابتدا و انتهای رشته را حذف می‌کند، که قبل از تبدیل رشته به `u32` که فقط می‌تواند داده‌های عددی داشته باشد، باید این کار را انجام دهیم. کاربر باید کلید <kbd>enter</kbd> را فشار دهد تا `read_line` مقدار ورودی را دریافت کند، که یک کاراکتر newline به رشته اضافه می‌کند. برای مثال، اگر کاربر کلید <kbd>5</kbd> را تایپ کند و <kbd>enter</kbd> را فشار دهد، `guess` به این شکل خواهد بود: `5\n`. `\n` نشان‌دهنده "خط جدید" است. (در ویندوز، فشار دادن <kbd>enter</kbd> منجر به carriage return و newline، یعنی `\r\n` می‌شود.) متد `trim` `\n` یا `\r\n` را حذف می‌کند و نتیجه فقط `5` است.

متد [`parse` روی رشته‌ها][parse]<!-- ignore --> یک رشته را به نوع دیگری تبدیل می‌کند. اینجا از آن برای تبدیل یک رشته به عدد استفاده می‌کنیم. باید به Rust نوع عدد دقیق موردنظرمان را با استفاده از `let guess: u32` بگوییم. علامت `:` بعد از `guess` به Rust می‌گوید که نوع متغیر را مشخص خواهیم کرد. Rust چند نوع عدد داخلی دارد؛ `u32` که اینجا دیده می‌شود، یک عدد صحیح 32 بیتی بدون علامت است. این یک انتخاب پیش‌فرض خوب برای یک عدد مثبت کوچک است. درباره دیگر انواع عددی در [فصل 3][integers]<!-- ignore --> خواهید آموخت.

علاوه بر این، حاشیه‌نویسی `u32` در این برنامه نمونه و مقایسه با `secret_number` به این معناست که Rust استنباط خواهد کرد که `secret_number` نیز باید یک `u32` باشد. بنابراین اکنون مقایسه بین دو مقدار از یک نوع خواهد بود!

متد `parse` فقط روی کاراکترهایی کار می‌کند که منطقی بتوان آن‌ها را به اعداد تبدیل کرد و بنابراین به‌راحتی می‌تواند باعث خطا شود. برای مثال، اگر رشته‌ای شامل `A👍%` باشد، هیچ راهی برای تبدیل آن به عدد وجود ندارد. چون ممکن است این عملیات شکست بخورد، متد `parse` نوع `Result` را برمی‌گرداند، دقیقاً مانند متد `read_line` (که قبلاً در [“مدیریت خطای احتمالی با `Result`”](#handling-potential-failure-with-result)<!-- ignore --> بحث کردیم). ما این `Result` را همان‌طور که قبلاً انجام دادیم با استفاده مجدد از متد `expect` مدیریت خواهیم کرد. اگر `parse` متغیر `Err` از نوع `Result` را برگرداند زیرا نتوانست یک عدد از رشته ایجاد کند، فراخوانی `expect` بازی را متوقف کرده و پیام مشخص‌شده را چاپ می‌کند. اگر `parse` بتواند با موفقیت رشته را به عدد تبدیل کند، متغیر `Ok` از نوع `Result` را برمی‌گرداند و `expect` عدد مورد نظر را از مقدار `Ok` بازمی‌گرداند.


<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-03-convert-string-to-number/
touch src/main.rs
cargo run
  76
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.26s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

عالی! حتی با اینکه قبل از حدس کاربر فاصله‌هایی اضافه شده بود، برنامه همچنان تشخیص داد که کاربر عدد 76 را حدس زده است. برنامه را چند بار اجرا کنید تا رفتارهای مختلف را با انواع مختلف ورودی بررسی کنید: عدد را درست حدس بزنید، عددی که خیلی بزرگ است حدس بزنید، و عددی که خیلی کوچک است را حدس بزنید.

اکنون بیشتر بخش‌های بازی کار می‌کند، اما کاربر فقط می‌تواند یک حدس بزند. بیایید این موضوع را با اضافه کردن یک حلقه تغییر دهیم!

## اجازه دادن به چندین حدس با استفاده از حلقه

کلمه کلیدی `loop` یک حلقه بی‌نهایت ایجاد می‌کند. ما یک حلقه اضافه می‌کنیم تا به کاربران فرصت‌های بیشتری برای حدس زدن عدد بدهیم:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-04-looping/src/main.rs:here}}
```

همان‌طور که می‌بینید، ما همه چیز از درخواست ورودی حدس به بعد را داخل یک حلقه قرار داده‌ایم. مطمئن شوید که خطوط داخل حلقه را چهار فاصله دیگر تورفتگی (indentation) بدهید و برنامه را دوباره اجرا کنید. اکنون برنامه به‌طور بی‌پایان از شما حدس می‌خواهد، که در واقع یک مشکل جدید ایجاد می‌کند. به نظر می‌رسد که کاربر نمی‌تواند از برنامه خارج شود!

کاربر همیشه می‌تواند برنامه را با استفاده از میانبر صفحه‌کلید <kbd>ctrl</kbd>-<kbd>c</kbd> متوقف کند. اما راه دیگری برای فرار از این هیولای سیری‌ناپذیر وجود دارد، همان‌طور که در بحث `parse` در [“مقایسه حدس با عدد مخفی”](#comparing-the-guess-to-the-secret-number)<!-- ignore --> ذکر شد: اگر کاربر پاسخی غیرعددی وارد کند، برنامه متوقف می‌شود. می‌توانیم از این موضوع استفاده کنیم تا به کاربر اجازه دهیم خارج شود، همان‌طور که در اینجا نشان داده شده است:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/no-listing-04-looping/
touch src/main.rs
cargo run
(too small guess)
(too big guess)
(correct guess)
quit
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.23s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

تایپ کردن `quit` باعث خروج از بازی می‌شود، اما همان‌طور که متوجه خواهید شد، وارد کردن هر ورودی غیرعددی دیگر نیز همین کار را انجام می‌دهد. این رفتار چندان بهینه نیست؛ ما می‌خواهیم بازی همچنین وقتی عدد درست حدس زده شد متوقف شود.

### خروج پس از حدس درست

بیایید برنامه را طوری تنظیم کنیم که وقتی کاربر برنده می‌شود، با افزودن یک دستور `break` از بازی خارج شود:

<span class="filename">Filename: src/main.rs</span>

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/no-listing-05-quitting/src/main.rs:here}}
```

اضافه کردن خط `break` بعد از `You win!` باعث می‌شود که برنامه وقتی کاربر عدد مخفی را به‌درستی حدس می‌زند، از حلقه خارج شود. خروج از حلقه همچنین به معنای خروج از برنامه است، زیرا حلقه آخرین بخش از `main` است.

### مدیریت ورودی نامعتبر

برای بهبود بیشتر رفتار بازی، به جای اینکه برنامه هنگام ورود ورودی غیرعددی توسط کاربر متوقف شود، بیایید بازی را طوری تنظیم کنیم که ورودی غیرعددی را نادیده بگیرد تا کاربر بتواند به حدس زدن ادامه دهد. این کار را می‌توان با تغییر خطی که در آن `guess` از یک `String` به یک `u32` تبدیل می‌شود انجام داد، همان‌طور که در لیستینگ 2-5 نشان داده شده است.

<Listing number="2-5" file-name="src/main.rs" caption="نادیده گرفتن یک حدس غیرعددی و درخواست یک حدس دیگر به جای متوقف کردن برنامه">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-05/src/main.rs:here}}
```

</Listing>

ما از یک فراخوانی `expect` به یک عبارت `match` تغییر می‌دهیم تا به جای متوقف کردن برنامه در صورت خطا، خطا را مدیریت کنیم. به یاد داشته باشید که `parse` یک نوع `Result` بازمی‌گرداند و `Result` یک enum است که دارای متغیرهای `Ok` و `Err` است. ما در اینجا از یک عبارت `match` استفاده می‌کنیم، همان‌طور که با نتیجه `Ordering` از متد `cmp` انجام دادیم.

اگر `parse` بتواند رشته را با موفقیت به یک عدد تبدیل کند، یک مقدار `Ok` بازمی‌گرداند که عدد تولیدشده را در خود دارد. مقدار `Ok` با الگوی شاخه اول مطابقت خواهد داشت و عبارت `match` فقط مقدار `num` که `parse` تولید کرده و در داخل مقدار `Ok` قرار داده است را بازمی‌گرداند. آن عدد در همان جایی که می‌خواهیم، در متغیر جدید `guess` که ایجاد می‌کنیم، قرار می‌گیرد.

اگر `parse` _نتواند_ رشته را به عدد تبدیل کند، یک مقدار `Err` بازمی‌گرداند که اطلاعات بیشتری درباره خطا دارد. مقدار `Err` با الگوی `Ok(num)` در شاخه اول `match` مطابقت ندارد، اما با الگوی `Err(_)` در شاخه دوم مطابقت دارد. کاراکتر زیرخط، `_`، یک مقدار کلی است؛ در این مثال، ما می‌گوییم که می‌خواهیم تمام مقادیر `Err` را بدون توجه به اطلاعات داخل آن‌ها مطابقت دهیم. بنابراین برنامه کد شاخه دوم، `continue` را اجرا می‌کند، که به برنامه می‌گوید به تکرار بعدی `loop` برود و یک حدس دیگر درخواست کند. بنابراین، برنامه به‌طور مؤثر تمام خطاهایی که `parse` ممکن است با آن‌ها مواجه شود را نادیده می‌گیرد!

حالا همه چیز در برنامه باید طبق انتظار کار کند. بیایید آن را امتحان کنیم:

<!-- manual-regeneration
cd listings/ch02-guessing-game-tutorial/listing-02-05/
cargo run
(too small guess)
(too big guess)
foo
(correct guess)
-->

```console
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 61
Please input your guess.
10
You guessed: 10
Too small!
Please input your guess.
99
You guessed: 99
Too big!
Please input your guess.
foo
Please input your guess.
61
You guessed: 61
You win!
```

عالی! با یک تغییر کوچک نهایی، بازی حدس زدن را کامل خواهیم کرد. به یاد داشته باشید که برنامه همچنان عدد مخفی را چاپ می‌کند. این کار برای آزمایش خوب بود، اما بازی را خراب می‌کند. بیایید دستور `println!` که عدد مخفی را خروجی می‌دهد حذف کنیم. لیستینگ 2-6 کد نهایی را نشان می‌دهد.

<Listing number="2-6" file-name="src/main.rs" caption="کد کامل بازی حدس زدن">

```rust,ignore
{{#rustdoc_include ../listings/ch02-guessing-game-tutorial/listing-02-06/src/main.rs}}
```

</Listing>

در این مرحله، شما با موفقیت بازی حدس زدن را ساخته‌اید. تبریک می‌گویم!

## خلاصه

این پروژه یک روش عملی برای معرفی بسیاری از مفاهیم جدید Rust به شما بود: `let`، `match`، توابع، استفاده از جعبه‌ها (crates)ی خارجی، و موارد دیگر. در چند فصل بعدی، این مفاهیم را با جزئیات بیشتری یاد خواهید گرفت. فصل 3 مفاهیمی را که بیشتر زبان‌های برنامه‌نویسی دارند، مانند متغیرها، انواع داده و توابع را پوشش می‌دهد و نشان می‌دهد چگونه از آن‌ها در Rust استفاده کنید. فصل 4 مالکیت را بررسی می‌کند، ویژگی‌ای که Rust را از زبان‌های دیگر متمایز می‌کند. فصل 5 ساختارها و نحو متدها را مورد بحث قرار می‌دهد و فصل 6 توضیح می‌دهد که enumها چگونه کار می‌کنند.

[prelude]: https://doc.rust-lang.org/std/prelude/index.html
[variables-and-mutability]: ch03-01-variables-and-mutability.html#variables-and-mutability
[comments]: ch03-04-comments.html
[string]: https://doc.rust-lang.org/std/string/struct.String.html
[iostdin]: https://doc.rust-lang.org/std/io/struct.Stdin.html
[read_line]: https://doc.rust-lang.org/std/io/struct.Stdin.html#method.read_line
[result]: https://doc.rust-lang.org/std/result/enum.Result.html
[enums]: ch06-00-enums.html
[expect]: https://doc.rust-lang.org/std/result/enum.Result.html#method.expect
[recover]: ch09-02-recoverable-errors-with-result.html
[randcrate]: https://crates.io/crates/rand
[semver]: http://semver.org
[cratesio]: https://crates.io/
[doccargo]: https://doc.rust-lang.org/cargo/
[doccratesio]: https://doc.rust-lang.org/cargo/reference/publishing.html
[match]: ch06-02-match.html
[shadowing]: ch03-01-variables-and-mutability.html#shadowing
[parse]: https://doc.rust-lang.org/std/primitive.str.html#method.parse
[integers]: ch03-02-data-types.html#integer-types

