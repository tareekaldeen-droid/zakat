const CACHE_NAME = 'zakat-calculator-v3';
const assetsToCache = [
  './',
  'index.html',
  'manifest.json',
  'https://cdn.jsdelivr.net/npm/jsbarcode@3.11.5/dist/JsBarcode.all.min.js',
  'https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js',
  'https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js',
  'https://fonts.googleapis.com/css2?family=Amiri&family=Cairo:wght@400;600;700&family=Lateef&display=swap'
];

// تثبيت الـ Service Worker وحفظ الملفات
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME).then(cache => {
      console.log('🕌 جاري حفظ ملفات التطبيق للعمل بدون اتصال...');
      return cache.addAll(assetsToCache);
    })
  );
});

// تفعيل النسخة الجديدة وحذف القديمة
self.addEventListener('activate', event => {
  event.waitUntil(
    caches.keys().then(keys => {
      return Promise.all(
        keys.filter(key => key !== CACHE_NAME).map(key => caches.delete(key))
      );
    })
  );
});

// استراتيجية الاستجابة (Cache First)
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request).then(cachedResponse => {
      return cachedResponse || fetch(event.request);
    })
  );
});
