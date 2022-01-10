# دالة forEach في لغة جافاسكريبت - كيف تمر على كل عناصر مصفوفة (Array) في الجافاسكريبت

![JavaScript forEach – How to Loop Through an Array in JS](https://cdn-media-2.freecodecamp.org/w1280/5f9c99d8740569d1a4ca2204.jpg)

تعتبر دالة forEach واحدة من عدة طرق تمكنك من المرور على عناصر مصفوفة معينة (Array). لكل طريقة منهُم عدة مزايا و يعود لك اختيار أيهم الطريقة الأنسب تبعًا للمهمة المطلوبة.

في هذا المقال، سنلقى نظرة مفصّلة على دالة forEach في الجافاسكريبت.

في حال كان لدينا المصفوفة التالية:

```javascript
const numbers = [1, 2, 3, 4, 5];
```

استخدام طريقة "for loop" للمرور على عناصر المصفوفة (Array) سيكون كالتالي:

```javascript
for (i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}
```

## ما الذي يجعل طريقة forEach( ) مختلفة؟

تُستخدم طريقة forEach كذلك للمرور على عناصر الـ Array و لكنها تستخدم بشكل مختلف عن طريقة "for loop" المعتادة.

باستخدام forEach فنحن نُمرّر [دالة نداء عكسي callback function](https://www.freecodecamp.org/news/javascript-callback-functions-what-are-callbacks-in-js-and-how-to-use-them/) لكل عنصر من عناصر الـ Array أثناء المرور عليه تحتوى على قيم parameters الآتية:

- القيمة الحالية Current Value (اجباري) - و هي قيمة العنصر الذي مُررت إليه دالة الـ callback
- مؤشر موضع العنصر Element Index (اختياري) - و هو المؤشر الذي يحدد موضع العنصر في الـ Array
- المصفوفة Array (اختياري) - و هي المصفوفة الأساسية التي ينتمى إليها العنصر

و الآن سأشرح قيم الـ parameters تلك بالتفصيل.

في البداية، كي أمر على عناصر مصفوفة باستخدام forEach نحتاج إلى دالة نداء عكسي callback function (أو دالة مجهولة anonymous function):

```javascript
numbers.forEach(function () {
  // code
});
```

سيتم تنفيذ الدالة مرة لكل عنصر في المصفوفة. و يجب أن تضم parameter واحد على الأقل و الذي يمثل ذلك العنصر نفسُه.

```javascript
numbers.forEach(function (number) {
  console.log(number);
});
```

و هذا هو كل ما سنحتاج كي نمر على كل عناصر المصفوفة:

![](https://www.freecodecamp.org/news/content/images/2020/06/Ads-z-2.png)

و يمكنك استخدام صورة الدالة السَهمية (arrow function) المذكورة ضمن مفاهيم ES6 بدلًا من الدالة المعتادة لتبسيط الكود.

```javascript
numbers.forEach((number) => console.log(number));
```

صورة الدالة السهمية (Arrow Function)

## متغيرات الـ Parameters الاختيارية

### مؤشر موضع العنصر (Index)

و الآن لنكمل مع الـ parameters الاختيارية و نبدأ بالـ Index و الذي يمثّل موضع العنصر ضمن المصفوفة.

يمكننا ببساطة الحصول على مؤشر موضع العنصر (index) إذا ما أدرجنا متغيّر parameter ثاني.

```javascript
numbers.forEach((number, index) => {
  console.log("Index: " + index + " Value: " + number);
});
```

![](https://www.freecodecamp.org/news/content/images/2020/06/Ads-z-3.png)

### المصفوفة (Array)

متغيّر الـ Array يعطينا نسخة من المصفوفة نفسها. و هو كذلك اختياري لكن يمكن استخدامه في حال الحاجة إليه لتنفيذ بعض العمليات؛ و إذا ما ناديناه فسيطبع لنا المصفوفة كاملة بعدد العناصر الموجودة بها.

```javascript
numbers.forEach((number, index, array) => {
  console.log(array);
});
```

![](https://www.freecodecamp.org/news/content/images/2020/07/Ads-z.png)

يمكنك في ذلك الفيديو مشاهدة مثال للمرور على عناصر مصفوفة باستخدام طريقة forEach( ):

[رابط فيديو الشرح المستخدم من قِبل الكاتب باللغة الإنجليزية](https://www.youtube.com/watch?v=E2GawbHDFfs)

## دعم المتصفحات

دالة forEach الخاصة بالمصفوفات [مدعومة](https://caniuse.com/#search=Array.foreach) في كل المتصفحات عدا متصفح Internet Exploere (IE) حتى نسخته الثامنة.

![](https://www.freecodecamp.org/news/content/images/2020/06/Ads-z.png)

[caniuse.com](https://caniuse.com/)

**إذا ما أردت تعلّم المزيد عن تطوير الويب، يمكنك زيارة [قناة كاتب المقال الأصلي على اليوتيوب](https://www.youtube.com/channel/UC1EgYPCvKCXFn8HlpoJwY3Q?view_as=subscriber).**

و شكرًا على قراءتك!
