# مثال على console.log() في الجافاسكريبت - كيف تطبع محتوى في وحدة التحكم (console) في لغة الجافاسكريبت

![JavaScript Console.log() Example – How to Print to the Console in JS](https://www.freecodecamp.org/news/content/images/size/w2000/2020/09/fCC_-Console.log.png)

تعتبر الطباعة في الـ console وسيلة بسيطة للغاية لتحليل و اكتشاف و إصلاح المشاكل الصغيرة ف الكود الخاص بك.

لكن هل تعلم أن الـ `console` به أكثر من مجرد الـ `log`؟ في هذا المقال سأعرض لك كيف تطبع في الـ console في الجافاسكريبت كما سأعرض لك كل ما لم تكن تعرف أن الـ `console` قادر على فِعلُه.

## المحرر متعدد الأسطر في الـ console الخاص بمتصفح Firefox

إذا لم تستخدم من قبل وضعية المحرر متعدد الأسطر في متصفح فايرفوكس، فعليك أن تجربها الآن!

كل ما عليك هو فتح الـ console عن الطريق الضغط على `F12` أو `Ctrl+Shift+K` و ستجد في أعلى اليمين زر مكتوب عليه "Switch to multi-line editor mode". أو يمكنك بدلًا من ذلك أن تضغط `Ctrl+B`.

و هو ما يوفر لك محرر أكواد متعدد الأسطر داخل متصفح Firefox نفسُه.

## console.log

فلنبدأ بمثال log بسيط.

```js
let x = 1;
console.log(x);
```

اكتب ذلك الكود في console متصفّح Firefox و قم بتشغيل الكود عن طريق الضغط على زر "Run" أو الضغط على `Ctrl+Enter`.

من المفترض في هذا المثال أن نري "1" مطبوع في الـ console.  
واضح للغاية، أليس كذلك؟

## عرض قيم متعددة

هل تعلم أيضًا أنه يمكنك إضافة قيم متعددة؟ أضف نص (string) في البداية كي تميّز القيمة التي تطبعها؟

```js
let x = 1;
console.log("x:", x);
```

ماذا لو كان لدينا قيم متعددة نريد عرضها؟

```js
let x = 1;
let y = 2;
let z = 3;
```

بدلًا من كتابة `console.log()` ثلاثة مرات، يمكننا أن نكتبهم جميعًا في مرة واحدة مع إضافة نص (string) قبل كل منهم إن أردنا ذلك.

```js
let x = 1;
let y = 2;
let z = 3;
console.log("x:", x, "y:", y, "z:", z);
```

لكن هذا يتطلب الكثير من العمل. يمكنك فقط أن تضع المتغيرات بين قوسين معقوصين (curly braces) و الآن تعرض كائن (object) يحتوى على قيم المتغيرات بأسمائها.

```js
let x = 1;
let y = 2;
let z = 3;
console.log({ x, y, z });
```

![Console Output](https://www.freecodecamp.org/news/content/images/2020/09/image-34.png)  
Console Output

و يمكنك القيام بنفس الشئ لعرض كائن (object).

```js
let user = {
  name: "Jesse",
  contact: {
    email: "codestackr@gmail.com",
  },
};
console.log(user);
console.log({ user });
```

أول سطر log سيطبع فقط الخصائص الموجودة بداخل الكائن (object properties). بينما السطر الثاني سيعرض الكائن user مع توضيح اسمُه و يعرض الخصائص (properties) الموجودة بداخله.

في حال عرضك للكثير من البيانات (data) في الـ console فتلك الطريقة ستساعدك لتتعرف على كلّ منها.

## المتغيّرات بداخل الـ log

هل تعلم أن يمكنك استخدام أجزاء من الـ log كمتغيّرات؟

```js
console.log("%s is %d years old.", "John", 29);
```

في هذا المثال، `%s` تشير إلى قيمة نصيّة (string) مضافة بعد القيمة الأولى (initial value). هنا مثلًا القيمة النصية هي "John".

بينما `%d` تشير إلى قيمة رقمية (digit) مضافة بعد القيمة الأولى (initial value). و هي هنا تشير إلى 29.

لتكون القيمة المطبوعة من هذه الجملة البرمجية (statement) هي "John is 29 years old.".

## الأشكال المختلفة للـ logs

هناك بعض الأشكال المختلفة من الـ logs، أكثرها استخدامًا هي جملة `console.log()` و لكن يوجد المزيد:

```js
console.log("Console Log");
console.info("Console Info");
console.debug("Console Debug");
console.warn("Console Warn");
console.error("Console Error");
```

هذه الجمل (statments) المختلفة ستضيف بغض التغييرات الشكلية (styling) للـ logs الظاهرة في الـ console. فمثلًا جمل `warn` ستظهر باللون الأصفر بينما جمل `error` ستظهر باللون الأحمر.

و لاحظ أن تلك التغييرات الشكلية قد تختلف تبعًا للمتصفح.

## الـ logs الاختيارية

يمكننا عرض رسائل معينة في الـ console إن تحقق شرط معيّن باستخدام `console.assert()`.

```js
let isItWorking = false;
console.assert(isItWorking, "this is the reason why");
```

إن كانت القيمة الأولى (first argument) الممررة إلى `console.assert()` هي قيمة false، ستظهر الرسالة في الـ console.

و لن تظهر الرسالة إن غيرنا القيمة الممررة إلى قيمة true.

## العد (Counting)

هل تعلم أن يمكنك العد (counting) في الـ console.

```js
for (i = 0; i < 10; i++) {
  console.count();
}
```

في كل تكرار (iteration) من هذه الحلقة (loop) سيتم طباعة رقم العدّة (count) في الـ console. سترى الرسائل المطبوعة هي "default: 1, default: 2" حتى يصل للعدّة رقم 10.

و إن استخدمت نفس الحلقة (loop) مرة أخرى فسترى أن العد يكمل من حيث انتهى من 11 إلى 20.

و لكي تعيد العداد (counter) إلى الصفر يمكنك استخدام `console.countReset()`.

و إن أردت إضافة اسم مختلف عن الاسم الافتراضي للعداد فيمكنك ذلك!

```js
for (i = 0; i < 10; i++) {
  console.count("Counter 1");
}
console.countReset("Counter 1");
```

و الآن بما أننا أضفنا عنوان للعداد فستكون الرسائل المطبوعة "Counter 1, Counter 2" و هكذا.

و لإعادة العداد لقيمته الأساسية نحتاج لتمرير اسمُه إلى `countReset`. بهذه الطريقة تتمكن من استخدام أكتر من counter في نفس الوقت بينما تعيد بعضهم فقط إلى قيمهم الأوليّة.

## تتبُّع الوقت

بالإضافة لاستخدام الـ log في العَد يمكننا حساب الوقت مثل الـ stopwatch.

لبداية احتساب الوقت يمكننا استخدام `console.time()`، لن ترى أي اختلاف بدون كود نتمكن من حساب مدة عمله لذا سوف نستخدم `setTimeout()` في هذا المثال لنحاكي وجود الكود. و بداخل الدالة (function) المتضمنة في `setTimeout()` سوف نوقف احتساب الوقت باستخدام `console.timeEnd()`.

```js
console.time();
setTimeout(() => {
  console.timeEnd();
}, 5000);
```

كما من الممكن أن تتوقع، بعد خمس ثوان سيوقف المؤقت و يعرض المدة المأخوذة و هي خمس ثوان.

و يمكننا أيضًا عرض المدة المأخوذة حتى لحظة معينة أثناء عمل المؤقت و بدون إيقافه باستخدام `console.timeLog()`.

```js
console.time();

setTimeout(() => {
  console.timeEnd();
}, 5000);

setTimeout(() => {
  console.timeLog();
}, 2000);
```

في هذا المثال سوف يُعرَض ثانيتين من خلال `timeLog` و بعدها يُعرَض 5 ثوان من خلال `timeEnd`.

و تمامًا مثل العدادات (counters) يمكننا تسمية المؤقتات (timers) و تشغيل عدة منهم في نفس الوقت.

## مجموعات الـ logs

كما يمكنك أيضًا تجميع أسطر الـ `log` سوية.

نبدأ مجموعة الـ logs باستخدام `console.group()`؛ و ننهيها باستخدام `console.groupEnd()`.

```js
console.log("I am not in a group");

console.group();
console.log("I am in a group");
console.log("I am also in a group");
console.groupEnd();

console.log("I am not in a group");
```

مجموعة الـ logs تلك ستظهر سويّا و يمكن طيّها (collapsible group) و هو ما يجعل التعرف مجموعات الـ logs المتشابهة أسهل.

لن تظهر المجموعة مطوية بشكل افتراضي و لكن يمكنك طبعها بشكل مطوي عن طريق `console.groupCollapsed()` بدلًا من `console.group()`.

و يمكن إضافة عناوين إلى أسطر `group()` للتعرف عليها بشكل أسهل.

## تتبّع المكدّس (Stack)

يمكنك كذلك تتبع تنفيذ عمليات الـ stack عن طريق الـ console بإضافتها إلى دالة (function).

```js
function one() {
  two();
}
function two() {
  three();
}
function three() {
  console.trace();
}
one();
```

في هذا المثال، لدينا مجموعة من الدوال (functions) البسيطة التي ينادي بعضها البعض. و في الدالة الأخيرة ننادي `console.trace()`.

![Console Output](https://www.freecodecamp.org/news/content/images/2020/09/image-35.png)  
Console Output

## الجداول (Tables)

و إليك واحدة من أكثر استخدامات الـ console المذهلة `console.table()`.

في البداية، لنجهز بعض البيانات (data) لعرضها:

```js
let devices = [
  {
    name: "iPhone",
    brand: "Apple",
  },
  {
    name: "Galaxy",
    brand: "Samsung",
  },
];
```

و الآن لنعرض تلك البيانات (data) باستخدام `console.table(devices)`.

![Console Output](https://www.freecodecamp.org/news/content/images/2020/09/image-36.png)  
Console Output

و يمكنك كذلك تحسين تجربة عرض البيانات في الجداول.

إن أردنا فقط عرض الـ 'brands'، علينا فقط أن نكتب `console.table(devices, ['brand'])`!

![Console Output](https://www.freecodecamp.org/news/content/images/2020/09/image-37.png)  
Console Output

ما رأيك أن نجرب مثالًا أعقَد؟ في هذا المثال سنستخدم jsonplaceholder.

```js
async function getUsers() {
  let response = await fetch("https://jsonplaceholder.typicode.com/users");
  let data = await response.json();

  console.table(data, ["name", "email"]);
}

getUsers();
```

و ها نحن ذا نعرض فقط الـ "name" و الـ "email". إن استخدمت `console.log` لعرض كل البيانات فستجد أن هناك الكثير من الخصائص (properties) الأخرى لكل user.

## التغييرات الشكلية (Styling)

هل تعلم أنه يمكنك استخدام أوراق الأنماط المتتالية (Cascading Style Sheets CSS) لتعديل أنماط الـ logs؟

لتحقيق ذلك نستخدم `%c` لنوضح أن لدينا أنماط نحتاج لتطبيقها و نمرر الأنماط كنص (string) في القيمة الثانية (second argument).

```js
console.log(
  "%c This is yellow text on a blue background.",
  "color:yellow; background-color:blue"
);
```

يمكنك استخدام تلك الخاصية لجعل الـ logs الخاصة بك مميزة.

## إزالة محتوى الـ console

عندما تحاول استكشاف مشكلة ما باستخدام الـ logs، قد تحتاج لتحديث الصفحة أكثر من مرة و هو ما يجعل الـ console ملئ بالبيانات (data) الجديدة و القديمة بدون ترتيب.

و لكن بإضافة `console.clear()` قبل بداية الـ code الخاص بك ستحصل على console فارغ من المحتوى في كل مرة تحدّث فيها الصفحة.
حاول فقط ألا تضعه في نهاية الكود 😃

## شكرًا لقراءتك!

إن أردت أن تعيد معرفة المفاهيم الواردة في هذا المقال من خلال فيديو، يمكنك الإطلاع على [نسخة صنعها كاتب المقال الأصلي باللغة الإنجليزية](https://youtu.be/_-bHhEGcDiQ).
