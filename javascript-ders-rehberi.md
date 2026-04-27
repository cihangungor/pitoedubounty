# Temel JavaScript — Ders Anlatım Rehberi

> **Ön koşul:** HTML, CSS ve Bootstrap konuları tamamlanmış olmalıdır.  
> **Hedef kitle:** Web geliştirmeye yeni başlayan öğrenciler.  
> **Tahmini süre:** 8–10 ders saati (her bölüm yaklaşık 1 ders saati olarak planlanmıştır).

---

## Bölüm 1 · JavaScript Nedir ve Neden Öğreniyoruz?

### Anlatım Notları

Öğrencilere şu ana kadar öğrendikleri üç katmanı hatırlatarak başlayın:

- **HTML** → yapı (iskelet)
- **CSS** → görünüm (kıyafet)
- **JavaScript** → davranış (hareket, etkileşim)

Bir web sayfasını canlı bir insana benzetin: HTML kemikleri, CSS dış görünüşü, JavaScript ise beyni ve kasları temsil eder. Kullanıcı bir butona tıkladığında ne olacağına, bir formun nasıl kontrol edileceğine, bir içeriğin nasıl dinamik olarak değişeceğine JavaScript karar verir.

### Gösterim Önerisi

Tarayıcıda basit bir HTML sayfası açın. Konsola (`F12 → Console`) `alert("Merhaba Dünya!")` yazarak JavaScript'in tarayıcıda doğrudan çalıştığını gösterin. Bu, öğrenciler için "sihirli an" olacaktır.

### Kod Yazma Yöntemleri

Üç farklı yöntemi gösterin ve hangisinin ne zaman tercih edileceğini açıklayın:

```html
<!-- 1. Inline: Doğrudan HTML etiketi içinde -->
<button onclick="alert('Tıkladın!')">Tıkla</button>

<!-- 2. Internal: <script> etiketi ile sayfa içinde -->
<script>
  console.log("Sayfa yüklendi.");
</script>

<!-- 3. External: Ayrı .js dosyasında (ÖNERİLEN) -->
<script src="script.js"></script>
```

**Vurgulayın:** Gerçek projelerde her zaman harici dosya kullanılır; bu, kodun düzenli ve bakımı kolay kalmasını sağlar.

---

## Bölüm 2 · Değişkenler ve Veri Türleri

### Anlatım Notları

Değişkeni bir "etiketli kutu" olarak tanımlayın: içine bir değer koyarsınız, etiket (isim) üzerinden o değere ulaşırsınız.

### `var`, `let` ve `const` Farkları

```javascript
var eski = "Artık pek kullanılmıyor";   // fonksiyon kapsamlı
let yas = 25;                            // blok kapsamlı, değiştirilebilir
const PI = 3.14;                         // blok kapsamlı, sabit
```

**Pratik kural:** Varsayılan olarak `const` kullanın. Değer değişecekse `let` kullanın. `var` kullanmaktan kaçının.

### Temel Veri Türleri

| Tür         | Örnek                        | Açıklama                     |
|-------------|------------------------------|------------------------------|
| `string`    | `"Merhaba"`, `'Dünya'`      | Metin                        |
| `number`    | `42`, `3.14`                 | Tam sayı ve ondalık          |
| `boolean`   | `true`, `false`              | Doğru / Yanlış               |
| `undefined` | `let x;`                     | Tanımlı ama değer atanmamış  |
| `null`      | `let y = null;`              | Bilinçli olarak "boş" değer  |
| `array`     | `[1, 2, 3]`                 | Sıralı liste                 |
| `object`    | `{ad: "Ali", yas: 20}`      | Anahtar-değer çifti          |

### Template Literal (Şablon Dizeler)

```javascript
const ad = "Ayşe";
const yas = 22;
console.log(`Benim adım ${ad}, yaşım ${yas}.`);
// Çıktı: Benim adım Ayşe, yaşım 22.
```

### Sınıf İçi Etkinlik

Öğrencilerden kendilerini tanıtan değişkenler oluşturmalarını isteyin:

```javascript
const ogrenciAd = "Mehmet";
const ogrenciYas = 21;
const ogrenciBolum = "Bilgisayar Mühendisliği";
console.log(`${ogrenciAd}, ${ogrenciYas} yaşında, ${ogrenciBolum} öğrencisi.`);
```

---

## Bölüm 3 · Operatörler

### Aritmetik Operatörler

```javascript
let a = 10, b = 3;
console.log(a + b);   // 13  → Toplama
console.log(a - b);   // 7   → Çıkarma
console.log(a * b);   // 30  → Çarpma
console.log(a / b);   // 3.33 → Bölme
console.log(a % b);   // 1   → Mod (kalan)
console.log(a ** b);  // 1000 → Üs alma
```

### Karşılaştırma Operatörleri

```javascript
console.log(5 == "5");    // true  → sadece değer karşılaştırır
console.log(5 === "5");   // false → değer + tür karşılaştırır (ÖNERİLEN)
console.log(5 !== "5");   // true
console.log(10 > 3);      // true
console.log(10 <= 10);    // true
```

**Vurgulayın:** Her zaman `===` ve `!==` kullanılmalıdır. `==` beklenmedik sonuçlar verebilir.

### Mantıksal Operatörler

```javascript
let yetiskin = true;
let ehliyetVar = false;

console.log(yetiskin && ehliyetVar);  // false → VE (ikisi de true olmalı)
console.log(yetiskin || ehliyetVar);  // true  → VEYA (biri yeterli)
console.log(!yetiskin);               // false → DEĞİL (tersini alır)
```

---

## Bölüm 4 · Koşul Yapıları

### `if`, `else if`, `else`

```javascript
const not = 75;

if (not >= 90) {
  console.log("AA — Pekiyi");
} else if (not >= 80) {
  console.log("BA — İyi");
} else if (not >= 70) {
  console.log("BB — Orta-İyi");
} else if (not >= 60) {
  console.log("CC — Orta");
} else {
  console.log("FF — Kaldı");
}
```

### Ternary (Üçlü) Operatör

```javascript
const yas = 17;
const durum = yas >= 18 ? "Yetişkin" : "Reşit değil";
console.log(durum); // "Reşit değil"
```

### `switch` Yapısı

```javascript
const gun = "Pazartesi";

switch (gun) {
  case "Pazartesi":
  case "Salı":
  case "Çarşamba":
  case "Perşembe":
  case "Cuma":
    console.log("İş günü");
    break;
  case "Cumartesi":
  case "Pazar":
    console.log("Hafta sonu");
    break;
  default:
    console.log("Geçersiz gün");
}
```

### Sınıf İçi Etkinlik

Öğrencilerden bir "Vücut Kitle İndeksi (VKİ) hesaplayıcı" yapmalarını isteyin: kullanıcıdan boy ve kilo alıp sonucu kategorize eden bir program.

---

## Bölüm 5 · Döngüler

### Anlatım Notları

Döngüyü "tekrar eden iş" olarak tanımlayın. Bir öğretmenin 30 öğrenciye tek tek not vermesi gibi; aynı işi farklı verilerle tekrarlıyoruz.

### `for` Döngüsü

```javascript
// 1'den 10'a kadar sayıları yazdır
for (let i = 1; i <= 10; i++) {
  console.log(i);
}

// Bir dizideki elemanları döndür
const meyveler = ["Elma", "Armut", "Portakal"];
for (let i = 0; i < meyveler.length; i++) {
  console.log(meyveler[i]);
}
```

### `while` Döngüsü

```javascript
let sayac = 5;
while (sayac > 0) {
  console.log(`Geri sayım: ${sayac}`);
  sayac--;
}
console.log("Başla!");
```

### `for...of` Döngüsü

```javascript
const renkler = ["kırmızı", "mavi", "yeşil"];
for (const renk of renkler) {
  console.log(renk);
}
```

### `break` ve `continue`

```javascript
// break: Döngüyü tamamen durdurur
for (let i = 1; i <= 10; i++) {
  if (i === 5) break;
  console.log(i); // 1, 2, 3, 4
}

// continue: O adımı atlar, döngü devam eder
for (let i = 1; i <= 5; i++) {
  if (i === 3) continue;
  console.log(i); // 1, 2, 4, 5
}
```

### Sınıf İçi Etkinlik

Çarpım tablosu yazdıran iç içe döngü yazımı:

```javascript
for (let i = 1; i <= 10; i++) {
  let satir = "";
  for (let j = 1; j <= 10; j++) {
    satir += `${i * j}\t`;
  }
  console.log(satir);
}
```

---

## Bölüm 6 · Fonksiyonlar

### Anlatım Notları

Fonksiyonu bir "makine" olarak tanımlayın: malzeme girer (parametre), ürün çıkar (return). Bir kez tasarlarsınız, istediğiniz kadar kullanırsınız.

### Fonksiyon Tanımlama Yolları

```javascript
// 1. Klasik fonksiyon bildirimi
function selamla(isim) {
  return `Merhaba, ${isim}!`;
}

// 2. Fonksiyon ifadesi (expression)
const topla = function(a, b) {
  return a + b;
};

// 3. Arrow function (ok fonksiyonu) — modern ve kısa
const carp = (a, b) => a * b;

// Tek parametrede parantez opsiyonel
const karesi = x => x * x;
```

### Varsayılan Parametre

```javascript
function selamVer(isim = "Misafir") {
  console.log(`Hoş geldin, ${isim}!`);
}

selamVer();        // Hoş geldin, Misafir!
selamVer("Zeynep"); // Hoş geldin, Zeynep!
```

### Fonksiyonla Pratik Örnek

```javascript
function notHesapla(vize, final) {
  const ortalama = vize * 0.4 + final * 0.6;

  if (ortalama >= 50) {
    return { ortalama, durum: "Geçti" };
  } else {
    return { ortalama, durum: "Kaldı" };
  }
}

const sonuc = notHesapla(60, 80);
console.log(`Ortalama: ${sonuc.ortalama}, Durum: ${sonuc.durum}`);
// Ortalama: 72, Durum: Geçti
```

### Sınıf İçi Etkinlik

Öğrencilerden şu fonksiyonları yazmalarını isteyin:
1. Bir sayının çift mi tek mi olduğunu döndüren fonksiyon
2. Bir dizideki en büyük sayıyı bulan fonksiyon
3. Celsius'u Fahrenheit'a çeviren fonksiyon

---

## Bölüm 7 · Diziler (Arrays) ve Temel Metotlar

### Dizi Oluşturma

```javascript
const notlar = [85, 90, 78, 92, 88];
const isimler = ["Ali", "Veli", "Ayşe"];
const karisik = [1, "iki", true, null]; // farklı türler olabilir
```

### Sık Kullanılan Dizi Metotları

```javascript
const meyveler = ["Elma", "Armut"];

// Eleman ekleme / çıkarma
meyveler.push("Portakal");       // sona ekle  → ["Elma","Armut","Portakal"]
meyveler.pop();                  // sondan çıkar → ["Elma","Armut"]
meyveler.unshift("Çilek");      // başa ekle   → ["Çilek","Elma","Armut"]
meyveler.shift();                // baştan çıkar → ["Elma","Armut"]

// Arama
meyveler.includes("Elma");      // true
meyveler.indexOf("Armut");      // 1

// Birleştirme ve dilimleme
meyveler.join(", ");             // "Elma, Armut"
meyveler.slice(0, 1);           // ["Elma"] → orijinali değiştirmez
meyveler.splice(1, 1, "Muz");  // index 1'den 1 eleman sil, "Muz" ekle
```

### Döngüsel Dizi Metotları

```javascript
const sayilar = [1, 2, 3, 4, 5];

// forEach: Her eleman için bir işlem yap
sayilar.forEach(sayi => console.log(sayi * 2));

// map: Dönüştürülmüş yeni dizi oluştur
const kareler = sayilar.map(s => s * s);
// [1, 4, 9, 16, 25]

// filter: Koşula uyan elemanları süz
const buyukler = sayilar.filter(s => s > 3);
// [4, 5]

// find: Koşula uyan ilk elemanı bul
const ilkCift = sayilar.find(s => s % 2 === 0);
// 2

// reduce: Tüm elemanları tek değere indirge
const toplam = sayilar.reduce((acc, s) => acc + s, 0);
// 15
```

### Sınıf İçi Etkinlik

Bir öğrenci listesi dizisi oluşturup şu işlemleri yaptırın:
- `map` ile tüm isimleri büyük harfe çevirin
- `filter` ile notu 50'nin üstünde olanları süzün
- `reduce` ile not ortalamasını hesaplayın

---

## Bölüm 8 · Nesneler (Objects)

### Anlatım Notları

Nesneyi bir "kimlik kartı" olarak tanımlayın: üzerinde isim, yaş, meslek gibi etiketli bilgiler (özellikler) bulunur.

### Nesne Oluşturma ve Erişim

```javascript
const ogrenci = {
  ad: "Elif",
  soyad: "Yılmaz",
  yas: 21,
  bolum: "Yazılım Mühendisliği",
  notlar: [85, 90, 78],
  tamAd: function() {
    return `${this.ad} ${this.soyad}`;
  }
};

// Erişim yolları
console.log(ogrenci.ad);          // "Elif"      → nokta notasyonu
console.log(ogrenci["bolum"]);    // "Yazılım…"  → köşeli parantez
console.log(ogrenci.tamAd());     // "Elif Yılmaz"

// Yeni özellik ekleme
ogrenci.email = "elif@mail.com";

// Özellik silme
delete ogrenci.yas;
```

### Nesne Döngüleri

```javascript
// for...in ile anahtarları döndür
for (const anahtar in ogrenci) {
  console.log(`${anahtar}: ${ogrenci[anahtar]}`);
}

// Object.keys, Object.values, Object.entries
console.log(Object.keys(ogrenci));    // ["ad", "soyad", ...]
console.log(Object.values(ogrenci));  // ["Elif", "Yılmaz", ...]
```

### Destructuring (Yapı Bozma)

```javascript
const { ad, soyad, bolum } = ogrenci;
console.log(ad);    // "Elif"
console.log(bolum); // "Yazılım Mühendisliği"

// Dizilerde destructuring
const [ilk, ikinci, ...geriKalan] = [10, 20, 30, 40, 50];
console.log(ilk);        // 10
console.log(geriKalan);  // [30, 40, 50]
```

---

## Bölüm 9 · DOM Manipülasyonu

### Anlatım Notları

Bu bölüm HTML-CSS bilgisini JavaScript ile birleştirir. Öğrencilere "artık sayfayı canlı hale getiriyoruz" deyin. DOM'u bir ağaç yapısı olarak tahtaya çizin.

### Eleman Seçme

```javascript
// Tek eleman seçme
const baslik = document.getElementById("baslik");
const ilkKutu = document.querySelector(".kutu");

// Çoklu eleman seçme
const tumButonlar = document.querySelectorAll("button");
const tumKutular = document.querySelectorAll(".kutu");
```

### İçerik ve Stil Değiştirme

```javascript
// Metin değiştirme
baslik.textContent = "Yeni Başlık";
baslik.innerHTML = "<em>Vurgulu Başlık</em>";

// Stil değiştirme
baslik.style.color = "red";
baslik.style.fontSize = "24px";

// CSS sınıfı ekleme / çıkarma
baslik.classList.add("aktif");
baslik.classList.remove("gizli");
baslik.classList.toggle("vurgulu");
```

### Yeni Eleman Oluşturma

```javascript
const yeniParagraf = document.createElement("p");
yeniParagraf.textContent = "Bu paragraf JavaScript ile oluşturuldu.";
yeniParagraf.classList.add("bilgi");
document.body.appendChild(yeniParagraf);
```

### Sınıf İçi Etkinlik

Bootstrap ile hazırlanmış bir kart bileşeni üzerinde çalışın:
- Butona tıklayınca kart renginin değişmesi
- Bir input alanına yazılan metnin kart başlığında anlık görünmesi

---

## Bölüm 10 · Olaylar (Events)

### Temel Olay Dinleyicileri

```javascript
const buton = document.querySelector("#gonderBtn");

buton.addEventListener("click", function() {
  alert("Butona tıklandı!");
});

// Arrow function ile
buton.addEventListener("click", () => {
  console.log("Tıklama algılandı.");
});
```

### Sık Kullanılan Olaylar

| Olay          | Tetiklenme Anı                     |
|---------------|-------------------------------------|
| `click`       | Eleman tıklandığında               |
| `dblclick`    | Çift tıklama                       |
| `mouseover`   | Fare elemanın üzerine geldiğinde   |
| `mouseout`    | Fare elemandan ayrıldığında        |
| `keydown`     | Klavye tuşuna basıldığında         |
| `keyup`       | Klavye tuşu bırakıldığında        |
| `input`       | Input değeri değiştiğinde          |
| `submit`      | Form gönderildiğinde              |
| `change`      | Select/checkbox değiştiğinde       |
| `load`        | Sayfa tamamen yüklendiğinde       |

### Event Nesnesi

```javascript
document.addEventListener("keydown", (e) => {
  console.log(`Basılan tuş: ${e.key}`);
  console.log(`Tuş kodu: ${e.code}`);
});

// Form submit'te sayfanın yenilenmesini engelleme
const form = document.querySelector("form");
form.addEventListener("submit", (e) => {
  e.preventDefault(); // Sayfanın yenilenmesini engelle
  console.log("Form gönderildi (sayfa yenilenmeden).");
});
```

### Sınıf İçi Etkinlik — Yapılacaklar Listesi (To-Do App)

Bu etkinlik önceki tüm konuları birleştirir:

```html
<!-- HTML yapısı -->
<div class="container mt-5">
  <h2>Yapılacaklar Listesi</h2>
  <div class="input-group mb-3">
    <input type="text" id="gorevInput" class="form-control"
           placeholder="Yeni görev ekle...">
    <button id="ekleBtn" class="btn btn-primary">Ekle</button>
  </div>
  <ul id="gorevListesi" class="list-group"></ul>
</div>
```

```javascript
// JavaScript
const gorevInput = document.querySelector("#gorevInput");
const ekleBtn = document.querySelector("#ekleBtn");
const gorevListesi = document.querySelector("#gorevListesi");

function gorevEkle() {
  const gorevMetni = gorevInput.value.trim();
  if (gorevMetni === "") return;

  const li = document.createElement("li");
  li.className = "list-group-item d-flex justify-content-between align-items-center";
  li.textContent = gorevMetni;

  const silBtn = document.createElement("button");
  silBtn.textContent = "Sil";
  silBtn.className = "btn btn-danger btn-sm";
  silBtn.addEventListener("click", () => li.remove());

  li.addEventListener("click", () => {
    li.classList.toggle("list-group-item-success");
  });

  li.appendChild(silBtn);
  gorevListesi.appendChild(li);
  gorevInput.value = "";
  gorevInput.focus();
}

ekleBtn.addEventListener("click", gorevEkle);
gorevInput.addEventListener("keydown", (e) => {
  if (e.key === "Enter") gorevEkle();
});
```

---

## Ek A · Hata Ayıklama (Debugging) İpuçları

Öğrencilere ilk dersten itibaren gösterin:

```javascript
// 1. console.log — en temel araç
console.log("Buraya geldi mi?", degisken);

// 2. console.table — diziler ve nesneler için
console.table([{ad: "Ali", not: 85}, {ad: "Veli", not: 90}]);

// 3. typeof — veri türünü kontrol et
console.log(typeof 42);        // "number"
console.log(typeof "merhaba");  // "string"
console.log(typeof undefined);  // "undefined"

// 4. try...catch — hata yakalama
try {
  const veri = JSON.parse("geçersiz json");
} catch (hata) {
  console.error("Hata oluştu:", hata.message);
}
```

---

## Ek B · Ödev Fikirleri (Kolaydan Zora)

1. **Hesap Makinesi** — Dört işlem yapan, sonucu ekranda gösteren basit hesap makinesi.
2. **Renk Değiştirici** — Butona her tıklamada sayfanın arka plan rengini rastgele değiştiren uygulama.
3. **Sayı Tahmin Oyunu** — Bilgisayarın rastgele ürettiği sayıyı kullanıcının tahmin ettiği oyun; "büyük/küçük" ipuçları verilir.
4. **Not Defteri** — Kullanıcının not ekleyip, düzenleyip, silebileceği basit bir not uygulaması (DOM manipülasyonu ağırlıklı).
5. **Quiz Uygulaması** — Çoktan seçmeli sorular soran, doğru/yanlış sayısı tutan, sonunda skoru gösteren uygulama.

---

## Ek C · Ders Akış Önerisi

| Ders | Konu                         | Önerilen Etkinlik              |
|------|------------------------------|--------------------------------|
| 1    | JS nedir, console, değişkenler | Kendini tanıtan değişkenler  |
| 2    | Operatörler ve koşul yapıları  | VKİ hesaplayıcı              |
| 3    | Döngüler                       | Çarpım tablosu               |
| 4    | Fonksiyonlar                   | Not hesaplama fonksiyonu     |
| 5    | Diziler ve metotları            | Öğrenci listesi işlemleri    |
| 6    | Nesneler ve destructuring       | Öğrenci bilgi kartı          |
| 7    | DOM manipülasyonu               | Bootstrap kart etkileşimi    |
| 8    | Olaylar + genel tekrar          | Yapılacaklar Listesi (To-Do) |

> **Pedagojik not:** Her dersin başında önceki konunun 5 dakikalık tekrarını yapın. Her dersin sonunda öğrencilerden "bugün öğrendiğim en önemli şey" yazmasını isteyin — bu hem sizin hem onların ilerlemeyi takip etmesine yardımcı olur.
