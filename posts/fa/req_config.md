---
title: 'پیکربندی درخواست ها'
prev_title: 'نمونه سازی از Axios'
prev_link: '/docs/instance'
next_title: 'الگوی پاسخ ها'
next_link: '/docs/res_schema'
---


موارد زیر گزینه های پیکربندی موجود برای ایجاد درخواست هستند. فقط `url` اجباری است. اگر `method` مشخص نشده باشد، درخواست ها به طور پیش فرض از نوع `GET` قرار می گیرند.

```js
{
  // `url` آدرس سروری است که برای درخواست استفاده می شود 
  url: '/user',

  // `method` نوع درخواست است که هنگام ایجاد درخواست استفاده می شود 
  method: 'get', // default

  // `baseURL` به `url` اضافه می شود مگر اینکه `url` یک آدرس مطلق باشد. 
  // می توان به راحتی `baseURL` را برای نمونه ای از axios تنظیم کرد تا آدرس های نسبی را به روش های آن نمونه منتقل کند.
  baseURL: 'https://some-domain.com/api',

  // `transformRequest` اجازه می دهد تا داده های درخواست قبل از ارسال به سرور تغییر کند
  // این فقط برای درخواستهایی از نوع 'PUT', 'POST', 'PATCH' و 'DELETE' می باشد
  // آخرین تابع در آرایه باید یک رشته یا نمونه ای از Buffer, ArrayBuffer, FormData یا Stream باشد
  // می توانید شی هدر را تغییر دهید.
  transformRequest: [function (data, headers) {
    // در اینجا برای تبدیل داده ها هر کاری می خواهید انجام دهید

    return data;
  }],

  // اجازه می دهد تا تغییرات داده های پاسخ قبل از ارسال به then/catch انجام شود
  transformResponse: [function (data) {
    // در اینجا برای تبدیل داده ها هر کاری می خواهید انجام دهید

    return data;
  }],

  // `headers` داده های هدر سفارشی هستند که باید ارسال شوند 
  headers: {'X-Requested-With': 'XMLHttpRequest'},

  // `params` پارامترهای URL هستند که با درخواست ارسال می شوند 
  // باید یک شیء ساده یا یک شیء URLSearchParams باشد 
  // نکته: پارامترهای خالی یا تعریف نشده در URL ارائه نمی شوند. 
  params: {
    ID: 12345
  },

  // 'paramsSerializer' یک پیکربندی اختیاری است که به شما امکان می‌دهد سریال‌سازی 'params' را سفارشی کنید
  paramsSerializer: {

    //عملکرد رمزگذار سفارشی که جفت های کلید/مقدار را به صورت تکراری ارسال می کند.
    encode?: (param: string): string => { /* عملیات سفارشی را در اینجا انجام دهید و رشته تبدیل شده را برگردانید */ }, 
    
    // عملکرد سریالساز سفارشی برای کل پارامتر. به کاربر امکان تقلید از رفتار قبل از 1.x را می دهد.
    serialize?: (params: Record<string, any>, options?: ParamsSerializerOptions ), 
    
    //پیکربندی برای قالب بندی شاخص های آرایه در پارامترها.
    indexes: false // سه گزینه موجود: (1) indexes: null (منجر به عدم وجود براکت می شود), 
    // (2) (default) indexes: false (منجر به پرانتزهای خالی می شود),
    // (3) indexes: true (منجر به براکت هایی با شاخص می شود).    
  },

  // `data` داده ای است که به عنوان بدنه درخواست ارسال می شود 
  // این فقط برای درخواستهایی از نوع 'PUT', 'POST', 'DELETE , و 'PATCH'  می باشد
  // وقتی هیچ `transformRequest` تنظیم نشده باشد ، باید یکی از انواع زیر باشد:
  // - string, plain object, ArrayBuffer, ArrayBufferView, URLSearchParams
  // - فقط مرورگر: FormData, File, Blob
  // - فقط سمت سرور: Stream, Buffer
  data: {
    firstName: 'Fred'
  },
  
  // جایگزین نحوه ارسال داده ها به بدنه درخواست
  // در نوع درخواست post
  // فقط مقدار ارسال می شود ، نه کلید
  data: 'Country=Brasil&City=Belo Horizonte',

  // `timeout` تعداد میلی ثانیه قبل از به پایان رسیدن زمان درخواست را مشخص می کند.
  // اگر درخواست بیش از مقدار `timeout` طول بکشد، درخواست لغو می شود. 
  timeout: 1000, // مقدار پیش فرض `0` است (بدون محدودیت زمان برای پایان درخواست)

  // `withCredentials` نشان می دهد که آیا درخواست های کنترل دسترسی از طریق سایت وجود دارد یا خیر
  // باید با استفاده از اعتبارنامه انجام شود
  withCredentials: false, // پیش فرض

  // `adapter` امکان مدیریت سفارشی درخواست‌ها را می دهد که تست کردن را آسان‌تر می‌کند.
  // یک پرامیس (promise) را برگرداند و یک پاسخ معتبر ارائه می دهد (این فایل را ببینید lib/adapters/README.md).
  adapter: function (config) {
    /* ... */
  },

  // `auth` نشان می دهد که باید از اعتبارنامه پایه HTTP استفاده شود و اعتبارنامه را ارائه می دهد.
  // با این کار یک هدر یا سربرگ `Authorization` تنظیم می کند و هر موجودی را بازنویسی می کند 
  // `Authorization` هدرهای سفارشی هستند که با استفاده از`headers` تنظیم کرده اید.
  // لطفاً توجه داشته باشید که فقط اعتبارنامه پایه HTTP از طریق این پارامتر قابل تنظیم است.
  // برای توکن های Bearer و موارد دیگر ، به جای آن از هدرهای سفارشی `Authorization` استفاده کنید.
  auth: {
    username: 'janedoe',
    password: 's00pers3cret'
  },

  // `responseType` نوع داده ای را که سرور با آن پاسخ خواهد داد را نشان می دهد
  // گزینه ها عبارتند از: 'arraybuffer', 'document', 'json', 'text', 'stream'
  //   فقط مرورگر: 'blob'
  responseType: 'json', // پیش فرض

  // `responseEncoding` نوع کدگذاری که برای استفاده رمزگشایی پاسخ‌ها استفاده می شود را نشان می‌دهد (فقط در Node.js)
  // نکته: برای درخواست‌های `responseType` در 'stream' یا درخواست‌های سمت سرویس گیرنده نادیده گرفته می شود
  responseEncoding: 'utf8', // پیش فرض

  // `xsrfCookieName` نام کوکی است که برای استفاده به عنوان مقدار رمز توکن xsrf استفاده می شود.
  xsrfCookieName: 'XSRF-TOKEN', // پیش فرض

  // `xsrfHeaderName` نام هدر http است که مقدار توکن xsrf در آن قرار دارد.
  xsrfHeaderName: 'X-XSRF-TOKEN', // پیش فرض

  // `onUploadProgress` امکان مدیریت پیشرفت رویدادها را برای آپلودها فراهم می کند
  // فقط در مرورگر
  onUploadProgress: function (progressEvent) {
    // با این رویداد پیشرفت بومی هر کاری که می خواهید انجام دهید
  },

  // `onDownloadProgress` امکان مدیریت رویدادهای پیشرفت دانلودها را فراهم می کند 
  // فقط در مرورگر
  onDownloadProgress: function (progressEvent) {
    // با این رویداد پیشرفت بومی هر کاری که می خواهید انجام دهید
  },

  // `maxContentLength` حداکثر اندازه محتوای پاسخ http را در بایت های مجاز در node.js تعریف می کند.
  maxContentLength: 2000,

  // `maxBodyLength` (گزینه ای فقط node.js) حداکثر اندازه محتوای درخواست http را در بایت های مجاز تعیین می کند.
  maxBodyLength: 2000,

  // `validateStatus` تعیین می کند که آیا پرامیس مربوط به کد وضعیت HTTP پاسخ داده شده را قبول یا رد کند.
  // اگر `validateStatus` مقدار `true` را برگرداند (یا روی `null` یا `undefined` تنظیم شده باشد)، پرامیس قبول خواهد شد. در غیر این صورت پرامیس رد می شود
  validateStatus: function (status) {
    return status >= 200 && status < 300; // پیش فرض
  },

  // `maxRedirects` حداکثر تعداد تغییر مسیرهایی را که باید در node.js دنبال شوند تعریف می کند.
  // اگر روی 0 تنظیم شود ، هیچ تغییرمسیری دنبال نمی شود.
  maxRedirects: 5, // پیش فرض

  // `socketPath` یک سوکت یونیکس را برای استفاده در node.js تعریف می کند.
  // به عنوان مثال، '/var/run/docker.sock' برای ارسال درخواست به داکر docker.
  // فقط `socketPath` یا `proxy` را می توان تعیین کرد.
  // اگر هر دو مشخص شده باشند، `socketPath` استفاده می شود.
  socketPath: null, // پیش فرض

  // `httpAgent` و` httpsAgent` یک عامل سفارشی را تعریف می کنند که هنگام انجام درخواستهای http و https به ترتیب در node.js، 
  // این اجازه را می دهد تا گزینه هایی مانند `keepAlive` اضافه شوند که به طور پیش فرض فعال نیستند.
  httpAgent: new http.Agent({ keepAlive: true }),
  httpsAgent: new https.Agent({ keepAlive: true }),

  // `proxy` نام میزبان، پورت و پروتکل سرور پراکسی را تعریف می کند.
  // همچنین می توانید پروکسی خود را با استفاده از متغیرهای محیط `http_proxy` و`https_proxy` تعریف کنید. 
  // اگر از متغیرهای محیطی برای پیکربندی پراکسی خود استفاده می‌کنید، می‌توانید متغیر محیطی `no_proxy` را به‌عنوان فهرستی از دامنه‌هایی که نباید پروکسی شوند با کاما تعریف کنید.
  // از `false` برای غیرفعال کردن پروکسی ها و نادیده گرفتن متغیرهای محیطی استفاده کنید.
  // `auth` نشان می دهد که اعتبارنامه پایه HTTP باید برای اتصال به پروکسی مورد استفاده قرار گیرد و اعتبارنامه ها را تأمین می کند.
  // با این کار یک هدر `Proxy-Authorization` تنظیم می شود و هدرهای سفارشی `Proxy-Authorization` که با استفاده از `headers` تنظیم کرده اید را بازنویسی می کند.
  // اگر سرور پروکسی از HTTPS استفاده می کند ، باید پروتکل را روی `https` تنظیم کنید. 
  proxy: {
    protocol: 'https',
    host: '127.0.0.1',
    port: 9000,
    auth: {
      username: 'mikeymike',
      password: 'rapunz3l'
    }
  },

  // `cancelToken` یک توکن لغو را مشخص می کند که می تواند برای لغو درخواست استفاده شود
  // (برای جزئیات بیشتر به بخش لغو درخواستها مراجعه کنید)
  cancelToken: new CancelToken(function (cancel) {
  }),

  // `decompress` نشان می دهد که آیا محتوای بدنه پاسخ باید به طور خودکار از حالت فشرده خارج شود یا خیر.
  // اگر روی `true` تنظیم شود، هدر 'content-encoding' را از شیء پاسخهای همه پاسخهای فشرده خارج می کند
  // - فقط در node.js (XHR نمی تواند فشرده سازی را غیرفعال کند)
  decompress: true // پیش فرض

}
```
