# عملگر Nullish coalescing '??'

[recent browser="new"]

عملگر nullish coalescing `??` یک سینتکس کوتاه شده است انتخاب نخستین متغیر تعریف شده است.

نتیجه‌ی گزاره‌ی `a ?? b` برابر است با:

- `a` به شرطی که `null` یا `undefined` نباشد,
- `b`, در غیر این صورت.

پس, `x = a ?? b` یک معادل کوتاه شده برای عبارت زیر است:

```js
x = a !== null && a !== undefined ? a : b;
```

حال به بررسی یک نمونه‌ی طولانی می‌پردازیم.

تصور کنین که ما یک کاربر داریم و متغیرهای, `firstName`, `lastName` یا `nickName` را برای نام، نام خانوادگی و نام مستعار داریم. همه‌ی آن‌ها ممکن است که تعریف نشده باشند(undefined) به شرطی که کاربر آن‌ها را وارد نکرده باشد.

ما می‌خواهیم که نام کاربر را نمایش دهیم: یکی از سه متغیر بالا, یا عبارت "ناشناس" به شرطی که چیزی تعریف نشده باشد.

از عملگر `??` برای انتخاب اولین متغیر تعریف شده استفاده می‌کنیم:

```js run
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

// نخستین مقداری که null یا undefined نیست را نمایش می‌دهد
*!*
alert(firstName ?? lastName ?? nickName ?? "ناشناس"); // Supercoder
*/!*
```

## مقایسه با ||

عملگر یا `||` هم می‌تواند به همان شکل عملگر `??` استفاده شود. در حقیقت ما می‌توانیم عملگر `??` را با عملگر `||` در کد بالا جایگزین کنیم و همان نتیجه را بگیریم. همانطور که در بخش قبلی توضیح داده شد [previous chapter](info:logical-operators#or-finds-the-first-truthy-value).

تفاوت ارزشمند این دو عبارت است از:

- `||` نخستین مقدار _truthy_ را باز می‌گرداند.
- `??` نخستین مقدار _defined_ را باز می‌گرداند.

این موضوع به خصوص هنگامی که می‌خواهیم بین `null/undefined` و `0` تفاوت داشته باشیم خودش را نشان می‌دهد.

برای نمونه در نظر بگیرید:

```js
height = height ?? 100;
```

در این‌جا به `height` مقدار `100` را در صورتی که مقدار تعریف شده‌ای نداشته باشد نسبت می‌دهیم.

مقایسه با `||`:

```js run
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

در این‌جا, `height || 100` با مقدار صفر همان‌گونه برخورد میکنه که با `null`، `undefined` و یا هر مقدار falsy دیگری. پس صفر تبدیل به `100` می‌شود.

عبارت `height ?? 100` مقدار `100` را در صورتی باز می‌گرداند که `height` دقیقا `null` و یا `undefined` باشد. پس صفر، "همانی که هست، می‌ماند".

    این‌که کدام رویکرد بهتر است، به موضوع ما بستگی دارد. اگر مقدار صفر برای `height` قابل قبول است، بهتر است از `??` استفاده کنیم.

## اولویت‌ها

اولویت عملگر `??` عموما پایین است: `7` در جدول [MDN table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table).

پس `??` پس از بسیاری از عملگرها وارد عمل می‌شود, البته پیش از `=` و `?`.

اگر تصمیم به استفاده از `??` در یک عبارت پیچیده گرفتید, حتما از پرانتز استفاده کنید:

```js run
let height = null;
let width = null;

// مهم: از پرانتز استفاده کنید
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```

در غیر این صورت, اگر پرانتز را فراموش کنیم, `*` اولویت بالاتری نسبت به `??` دارد . زودتر اجرا می‌شود.

این‌گونه خواهیم داشت:

```js
// احتمالا نتیجه‌ی نادرست می‌دهد
let area = height ?? 100 * width ?? 50;
```

هم‌چنین یک محدودیت سطح زبان (language-level) نیز برای این موضوع داریم.

**برای احتیاط, به کار بردن `??` همراه با `&&` و `||` ممنوع است.**

کد زیر یک خطای نگارشی (سینتکس) به ما خواهد داد:

```js run
let x = 1 && 2 ?? 3; // Syntax error خطای نگارشی
```

این محدودیت قابل بحث است, اما برای جلوگیری از اشتباهات برنامه‌نویسان به زبان اضافه شده است, که به مرور مردم از `??` به جای `||` استفاده خواهند کرد.

از پرانتز برای این کار استفاده کنید:

```js run
*!*
let x = (1 && 2) ?? 3; // به درستی کار می‌کند
*/!*

alert(x); // 2
```

## چکیده

- عملگر nullish coalescing `??` یک راه سریع برای مشخص کردن عبارت "تعریف شده (defined)" از یک لیست به کار می‌رود.

  از آن برای تعیین کردن مقدار پیش‌فرض برای متغیرها استفاده می‌شود:

  ```js
  // ست کردن height به 100, اگر برابر null یا undefined باشد
  height = height ?? 100;
  ```

- عملگر `??` ترتیب اولویت پایینی دارد, البته بیشتر از `?` و `=`.
- به کار بردن آن با `||` یا `&&` بدون به کار بردن پرانتز ممنوع است.