---
title: Event Emitters
localeTitle: بواعث الحدث
---
## الأحداث والجداول

تقليديا ، في خادم ويب ، لمعالجة البيانات في شكل ملف عن طريق القراءة والكتابة يستهلك الكثير من الذاكرة لأن طرق المعالجة هذه تحتاج إلى تحميل الملف في كل مرة يكون فيها لقراءة أو كتابة هذا الملف. هذا يعتبر مضيعة للموارد. فكر في ذلك ، من حيث قابلية التوسع والبيانات الضخمة إذا كنا نهدر الموارد ، فإننا سوف نتنازل كثيرًا. توفر طبيعة Node.js غير المتزامنة خيارين مناسبين لنا للعمل مع توفير تدفق البيانات التي تستهلك موارد أقل نظرًا لأن Node.js يعتمد على نموذج غير متزامن غير مقيد. هم ينبعث منها الأحداث والجداول. في هذا القسم ، سنلقي نظرة على كل منهما.

## فئة EventEmitter

EventEmitters هي واحدة من الأفكار الأساسية وراء بنية البرمجة التي تعتمد على الحدث أو البرمجة غير المتزامنة في Node.js. EventEmitter هو كائن وفي Node.js أي كائن ينبعث الحدث هو مثيل لفئة EventEmitter. ما هو الحدث؟ يعتبر كل إجراء تم اتخاذه بواسطة برنامج Node.js مثل بدء خادم الويب وإغلاق البرنامج عند عدم وجود طلب متبقٍ بمثابة حدثين منفصلين.

للوصول إلى فئة EventEmitter في برنامج Node.js ، يجب عليك طلب وحدة `events` من واجهة برمجة التطبيقات Node.js. لإنشاء الكائن ، نقوم بإنشاء مثيل لفئة EventEmitter من خلال استدعاء دالة منشئه.

 `const events = require('events'); 
 
 const eventEmitter = new events.EventEmitter(); 
` 

أو يمكنك مباشرة الوصول إلى فئة فرعية EventEmitter مثل:

 `const EventEmitter = require('events'); 
 
 const eventEmitter = new EventEmitter(); 
` 

توفر فئة EventEmitter العديد من الأساليب المحددة مسبقًا للعمل مع الأحداث. هذه الأساليب هي `.on` و `.emit` و `.error` . يمكن إجراء بث لحدث من دالة مما يؤدي إلى وظيفة رد اتصال يمكن الوصول إليها من خلال أي وظيفة أخرى في JavaScript. هذا مثل البث.

يمكن تنفيذ القدرة على تشغيل حدث باتباع البنية:

 `eventEmitter.emit(eventName, optionalData); 
` 

ويتم ذلك القدرة على إرفاق ظيفة المستمع وتحديد اسم الحدث المحدد الذي `.on` .

 `eventEmitter.emit(eventName, callback); 
` 

سنقوم بمحاكاة الوظائف الجديدة التي تعلمناها للتو مع مثال. قم بإنشاء ملف جديد يسمى `eventemitter.js` وقم بلصق التعليمة البرمجية التالية:

 `const EventEmitter = require('events'); 
 
 const eventEmitter = new EventEmitter(); 
 
 const callback = () => { 
    console.log('callback runs'); 
 }; 
 
 eventEmitter.on('invoke', callback); 
 
 eventEmitter.emit('invoke'); 
 eventEmitter.emit('invoke'); 
` 

الآن قم بتشغيل المثال أعلاه باستخدام الأمر `node` ويجب أن تحصل على الإخراج التالي.

 `callback runs 
 callback runs 
` 

نبدأ من خلال خلق مثيل eventEmitter نستطيع من خلالها الحصول على `.on` هذه الطريقة. تضيف طريقة `.on` الحدث عن طريق تحديد `invoke` الاسم الذي نستخدمه لاحقًا في `.emit` للاتصال باستدعاء وظيفة رد الاتصال المرتبطة به.

هناك دالة أخرى توفرها فئة EventEmitter التي تسمى `.once` . تستدعي هذه الطريقة وظيفة رد الاتصال المرتبطة بحدث للمرة الأولى عند إصدار الحدث. النظر في المثال أدناه.

 `const EventEmitter = require('events'); 
 
 const eventEmitter = new EventEmitter(); 
 
 const callback = () => { 
    console.log('callback runs'); 
 }; 
 
 eventEmitter.once('invoke', callback); 
 
 eventEmitter.emit('invoke'); 
 eventEmitter.emit('invoke'); 
` 

انتاج |

 `callback runs 
` 

`.once` الاحتفاظ بمسارات الأحداث عندما يتم تشغيلها وعدد المرات التي يتم تشغيلها بخلاف طريقة `.on` التي لا تتبع مثل هذا المسار. هذا هو الفرق الرئيسي بين الاثنين.

## فهم تيارات

توفر Node.js طريقة أخرى للعمل مع البيانات بدلاً من استهلاك كمية كبيرة من موارد الذاكرة وجعلها فعالة من حيث التكلفة. هذا ما تفعله التدفقات. في الأساس ، تدفقات تسمح لك بقراءة البيانات من مصدر واحد ووضعها في الوجهة. تعمل بيانات عمليات التدفق على قطع بدلاً من تجميعها في وقت واحد ، مما يجعلها مناسبة للعمل مع مجموعات البيانات الكبيرة. تستخدم العديد من الوحدات النمطية Node.js المضمنة دفق تحت غطاء محرك الأقراص. على سبيل المثال ، طلب HTTP والاستجابة ، ومقابس TCP ، و zlib ، و crypto ، و fs قراءة وتدفق الدفق ، إلخ.

### نوع من تيارات

يوجد في Node.js أربعة أنواع من التدفقات:

*   مقروء
*   للكتابة
*   دوبلكس
*   تحول

الأكثر شيوعا من هذه هي تيارات قابل للقراءة وقابل للكتابة. يتم استخدام التدفقات المقروءة لقراءة البيانات من المصدر ويتم استخدام التدفقات القابلة للكتابة لأداء عملية كتابة تلك البيانات إلى الوجهة. يمكن استخدام التدفقات المزدوجة لإجراء عمليات القراءة والكتابة. Transform عبارة عن مجموعة كبيرة من تدفقات الطباعة على الوجهين مع الاختلاف الوحيد في أنه يمكن تعديل البيانات عند القراءة أو الكتابة.

### نوع أحداث Stream

كل أنواع الدفق هذه هي أمثلة لفئة EventEmitter التي تعني أنها تنبعث من القراءة وتكتب الأحداث. يمكن لكل نوع من أنواع البث أن ينشر الأحداث التالية.

*   data: يتم تشغيل هذا الحدث عندما تكون البيانات متاحة للقراءة بواسطة الدفق القابل للقراءة
*   خطأ: يتم تشغيل هذا الحدث عند وجود خطأ في قراءة البيانات أو كتابتها
*   النهاية: يتم إطلاق هذا الحدث عند عدم وجود بيانات متبقية

## تيارات مقروءة

يسمح لك الدفق المقروء بقراءة البيانات من المصدر. يمكن أن يكون هذا المصدر أي شيء من مخزن مؤقت أو ملف أو ما إلى ذلك. أولاً ، قم بإنشاء ملف نصي بسيط للملف الذي سنقوم من خلاله بقراءة البيانات باستخدام الدفق.

 `I am Text file that contains data. 
` 

الآن ، قم بإنشاء ملف جديد يسمى read.js والذي سينفذ وظيفة قراءة البيانات من هذا الملف النصي باستخدام دفق مقروء.

 `const fs = require('fs'); 
 const readableStream = fs.createReadStream('abc.txt'); 
 let data = ''; 
 
 readableStream.on('data', function(chunk) { 
    data += chunk; 
 }); 
 
 readableStream.on('end', function() { 
    console.log(data); 
 }); 
` 

إذا قمت بتشغيل البرنامج أعلاه ، ستحصل على الإخراج التالي:

 ` $ node test.js 
 I am Text file that contains data. 
` 

وهو ما كتبناه داخل الملف النصي. لقراءة البيانات باستخدام الدفق ، نستخدم دالة تسمى `createReadStream()` من وحدة نظام الملفات `fs` .

عند عدم وجود بيانات متبقية للقراءة بواسطة الدفق القابل للقراءة ، فإنه ينتهي تلقائيًا من وظيفة رد الاتصال. أسلوب `.on` هو ما تعلمناه في القسم السابق من فئة EventEmitter. هذا يدل على أن تدفقات استخدام الطبقة EventEmitter وراء الكواليس.

## تيار قابل للكتابة

يتم استخدام التدفقات القابلة للكتابة لكتابة أو إدراج أو إلحاق البيانات إلى وجهة. مثل دفق مقروء ، يتم توفيرها أيضا من قبل وحدة `fs` . أنشئ ملفًا جديدًا باسم `wrtte.js` فيه دفقًا مقروءًا لقراءة البيانات من المصدر وكتابتها إلى وجهة عن طريق إنشاء ملف `.txt` جديد.

 `const fs = require('fs'); 
 const readableStream = fs.createReadStream('./abc.txt'); 
 const writeableStream = fs.createWriteStream('./dest.txt'); 
 let data = ''; 
 
 readableStream.on('data', function(chunk) { 
    writeableStream.write(chunk); 
 }); 
` 

عندما تقوم بتشغيل هذا البرنامج ، سيتم إنشاء ملف جديد من خلال الدفق القابل للكتابة حيث أنه لديه إمكانية الوصول إلى وحدة نظام الملفات. يستخدم دفق للكتابة `.write()` طريقة لإخراج البيانات على الوجهة. في المثال أعلاه ، نقوم بإنشاء ملف جديد باسم `dest.txt` يحتوي على نفس البيانات مثل `abc.txt` .

## الأنابيب

الأنابيب هو الآلية التي يمكنك أن تقرأ البيانات من مصدر والكتابة إلى الوجهة دون كتابة الكثير من التعليمات البرمجية كما فعلنا أعلاه وأنه لا يستخدم `.on` أو `.write` الأساليب.

إذا أردنا كتابة المثال أعلاه باستخدام الأنبوب ، فسوف نكتب كما يلي:

 `const fs = require('fs'); 
 const readableStream = fs.createReadStream('./abc.txt'); 
 const writeableStream = fs.createWriteStream('./dest.txt'); 
 
 readableStream.pipe(writeableStream); 
` 

لاحظ عدد أسطر الرمز التي أزلناها. أيضا ، نحن الآن بحاجة فقط إلى مسارات الملفات المصدر والوجهة ولقراءة وكتابة البيانات التي نستخدمها `.pipe()` .