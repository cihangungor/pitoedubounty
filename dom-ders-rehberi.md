# DOM Manipülasyonu — Ders Anlatım Rehberi

> **Ön koşul:** Değişkenler, veri türleri, koşullar, döngüler, fonksiyonlar, diziler ve nesneler konuları tamamlanmış olmalıdır.  
> **Hedef:** Öğrenciler bu rehberin sonunda bir web sayfasındaki elemanları seçebilir, içeriklerini ve stillerini değiştirebilir, yeni elemanlar oluşturabilir, olayları dinleyebilir ve form verileriyle işlem yapabilir hale gelecektir.  
> **Tahmini süre:** 6–8 ders saati.

---

## Bölüm 1 · DOM Nedir?

### Anlatım Notları

DOM (Document Object Model), tarayıcının HTML dosyasını okuyup bellekte oluşturduğu **ağaç yapısıdır**. HTML'deki her etiket bu ağaçta bir "düğüm" (node) olur. JavaScript bu ağaca erişerek sayfayı okuyabilir, değiştirebilir ve yeni parçalar ekleyebilir.

Öğrencilere şu benzetmeyi yapın: HTML dosyası bir **mimari plan**, DOM ise o plana göre inşa edilmiş **binadır**. JavaScript, binaya girip odaları boyayan, mobilya ekleyen ya da duvarları yıkan **ustadır**.

### Tahtada Gösterim

Aşağıdaki HTML için ağaç yapısını tahtaya çizin:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Sayfa Başlığı</title>
  </head>
  <body>
    <h1 id="baslik">Merhaba</h1>
    <p class="icerik">Bu bir paragraf.</p>
  </body>
</html>
```

Ağaç şöyle görünecektir:

```
document
 └── html
      ├── head
      │    └── title → "Sayfa Başlığı"
      └── body
           ├── h1#baslik → "Merhaba"
           └── p.icerik → "Bu bir paragraf."
```

**Vurgulayın:** `document` en tepedeki nesnedir. JavaScript'te DOM ile ilgili her şey `document` nesnesi üzerinden başlar.

---

## Bölüm 2 · Eleman Seçme

### Anlatım Notları

"Bir elemanı değiştirmek istiyorsak önce onu bulmamız gerekir" diye giriş yapın. CSS'te seçicileri (selector) öğrenmiştik; burada da aynı mantık var. JavaScript'e "şu elemanı bul ve bana ver" diyoruz.

### `getElementById` — ID ile Tek Eleman Seçme

```javascript
const baslik = document.getElementById("baslik");
console.log(baslik); // <h1 id="baslik">Merhaba</h1>
```

**Ne zaman kullanılır:** Sayfada benzersiz (unique) olan bir elemana ulaşmak istediğinizde. ID'ler sayfada tekdir; bu yüzden her zaman tek bir eleman döner.

**Dikkat:** Eleman bulunamazsa `null` döner. Bu yüzden sonraki satırlarda o değişkeni kullanmadan önce kontrol etmek iyi bir alışkanlıktır.

### `querySelector` — CSS Seçicisi ile Tek Eleman Seçme

```javascript
// ID ile seçme (getElementById ile aynı iş)
const baslik = document.querySelector("#baslik");

// Sınıf (class) ile seçme — ilk eşleşeni getirir
const ilkParagraf = document.querySelector(".icerik");

// Etiket adıyla seçme — ilk <p> elemanını getirir
const ilkP = document.querySelector("p");

// Karmaşık seçiciler de yazılabilir (CSS'teki gibi)
const ozelEleman = document.querySelector("div.kart > h3");
```

**Ne zaman kullanılır:** CSS'teki tüm seçici kurallarını kullanmak istediğinizde. `getElementById`'den daha esnektir. Yalnızca **ilk eşleşen** elemanı döndürür.

### `querySelectorAll` — Birden Fazla Eleman Seçme

```javascript
// Tüm paragrafları seç
const tumParagraflar = document.querySelectorAll("p");
console.log(tumParagraflar.length); // Kaç paragraf var?

// Tüm "kart" sınıfına sahip elemanları seç
const tumKartlar = document.querySelectorAll(".kart");
```

**Ne zaman kullanılır:** Aynı türdeki veya aynı sınıftaki birden fazla elemana ulaşmak istediğinizde. Sonuç bir **NodeList**'tir (diziye benzer ama tam dizi değildir). `forEach` ile döngüye alınabilir.

```javascript
tumParagraflar.forEach(function(paragraf) {
  console.log(paragraf.textContent);
});
```

### Hangisini Ne Zaman Kullanmalı?

| Yöntem                | Döndürdüğü    | Ne Zaman Kullanılır                                |
|-----------------------|---------------|-----------------------------------------------------|
| `getElementById`      | Tek eleman    | ID'si bilinen benzersiz bir eleman                  |
| `querySelector`       | Tek eleman    | CSS seçicisiyle ilk eşleşeni bulmak                 |
| `querySelectorAll`    | NodeList      | Birden fazla eleman seçmek, hepsiyle işlem yapmak   |

**Pratik tavsiye:** Modern projelerde `querySelector` ve `querySelectorAll` en çok kullanılan yöntemlerdir. CSS bilginizi doğrudan JavaScript'e aktarabilirsiniz.

### Sınıf İçi Uygulama

Bir Bootstrap kart yapısı verin, öğrencilerden konsolda şu işlemleri yapmalarını isteyin:

```html
<div class="container">
  <div class="card" id="kartBir">
    <h5 class="card-title">Başlık 1</h5>
    <p class="card-text">İçerik 1</p>
    <button class="btn btn-primary">Detay</button>
  </div>
  <div class="card" id="kartIki">
    <h5 class="card-title">Başlık 2</h5>
    <p class="card-text">İçerik 2</p>
    <button class="btn btn-primary">Detay</button>
  </div>
</div>
```

Görevler:
1. `kartBir`'i `getElementById` ile seçin ve konsola yazdırın.
2. İlk `.card-title` elemanını `querySelector` ile seçin.
3. Tüm butonları `querySelectorAll` ile seçip `forEach` ile "Buton bulundu" yazdırın.

---

## Bölüm 3 · İçerik Okuma ve Değiştirme

### `textContent` — Düz Metin

```javascript
const baslik = document.querySelector("#baslik");

// Okuma
console.log(baslik.textContent); // "Merhaba"

// Yazma (içeriği tamamen değiştirir)
baslik.textContent = "Hoş Geldiniz!";
```

**Ne yapar:** Elemanın içindeki **sadece metni** okur veya yazar. HTML etiketlerini dikkate almaz; etiket yazarsanız düz metin olarak görünür.

```javascript
baslik.textContent = "<em>Vurgulu</em>";
// Ekranda görünen: <em>Vurgulu</em>  (etiket olarak yorumlanmaz)
```

### `innerHTML` — HTML İçerik

```javascript
const kutu = document.querySelector("#kutu");

// Okuma
console.log(kutu.innerHTML); // "<p>Eski içerik</p>"

// Yazma (HTML olarak yorumlanır)
kutu.innerHTML = "<strong>Kalın metin</strong> ve <em>italik metin</em>";
```

**Ne yapar:** Elemanın içindeki **HTML kodunu** okur veya yazar. Yazdığınız HTML etiketleri tarayıcı tarafından yorumlanır ve görsel olarak uygulanır.

**Uyarı:** Kullanıcıdan gelen veriyi doğrudan `innerHTML` ile sayfaya basmak güvenlik açığı oluşturabilir (XSS). Kullanıcı verisi için `textContent` tercih edin.

### `value` — Form Elemanlarının Değeri

```javascript
const isimInput = document.querySelector("#isimInput");

// Kullanıcının yazdığı değeri okuma
console.log(isimInput.value); // "Ahmet"

// Değeri programla değiştirme
isimInput.value = "Mehmet";
```

**Ne zaman kullanılır:** `<input>`, `<textarea>` ve `<select>` gibi form elemanlarının değerine erişmek için. Normal HTML elemanlarında (`<p>`, `<h1>` vb.) `value` yoktur; onlar için `textContent` kullanılır.

### Özellik (Attribute) Okuma ve Değiştirme

```javascript
const resim = document.querySelector("img");

// Okuma
console.log(resim.getAttribute("src"));  // "foto.jpg"
console.log(resim.getAttribute("alt"));  // "Fotoğraf"

// Değiştirme
resim.setAttribute("src", "yeni-foto.jpg");
resim.setAttribute("alt", "Yeni fotoğraf açıklaması");

// Kaldırma
resim.removeAttribute("title");
```

**Ne zaman kullanılır:** Elemanların `src`, `href`, `alt`, `title`, `disabled`, `placeholder` gibi HTML özelliklerine erişmek veya bunları değiştirmek için.

### Sınıf İçi Uygulama

```html
<h1 id="sayacBaslik">Sayaç: 0</h1>
<button id="artirBtn">+1 Artır</button>
```

```javascript
let sayac = 0;
const baslik = document.querySelector("#sayacBaslik");
const buton = document.querySelector("#artirBtn");

buton.addEventListener("click", function() {
  sayac++;
  baslik.textContent = "Sayaç: " + sayac;
});
```

Öğrencilere adım adım ne olduğunu açıklayın: butona her tıklamada `sayac` değişkeni 1 artar ve başlığın metni güncellenir.

---

## Bölüm 4 · Stil ve CSS Sınıfı Değiştirme

### `style` ile Satır İçi Stil Verme

```javascript
const kutu = document.querySelector("#kutu");

kutu.style.backgroundColor = "lightblue";    // arka plan rengi
kutu.style.color = "darkblue";               // yazı rengi
kutu.style.padding = "20px";                 // iç boşluk
kutu.style.border = "2px solid blue";        // kenarlık
kutu.style.borderRadius = "10px";            // köşe yuvarlama
kutu.style.fontSize = "18px";               // yazı boyutu
```

**Önemli kural:** CSS'te `background-color` şeklinde tire ile yazılan özellikler, JavaScript'te **camelCase** ile yazılır: `backgroundColor`. Bu kural tüm çok kelimeli CSS özellikleri için geçerlidir (`font-size` → `fontSize`, `border-radius` → `borderRadius`).

**Ne zaman kullanılır:** Tek bir elemana dinamik olarak (kullanıcı etkileşimine bağlı) hızlıca stil vermek için. Ancak çok sayıda stil değişikliği yapıyorsanız CSS sınıfı kullanmak daha temizdir.

### `classList` ile CSS Sınıfı Yönetimi

Önce CSS dosyanızda sınıfları tanımlayın, sonra JavaScript ile ekleyip çıkarın:

```css
/* style.css */
.gizli       { display: none; }
.vurgulu     { background-color: yellow; font-weight: bold; }
.hata        { color: red; border: 1px solid red; }
.basarili    { color: green; border: 1px solid green; }
```

```javascript
const mesajKutusu = document.querySelector("#mesaj");

// Sınıf ekleme
mesajKutusu.classList.add("vurgulu");

// Sınıf çıkarma
mesajKutusu.classList.remove("vurgulu");

// Sınıf var/yok değiştirme (toggle)
// Varsa çıkarır, yoksa ekler — açma/kapama düğmeleri için idealdir
mesajKutusu.classList.toggle("gizli");

// Sınıfın var olup olmadığını kontrol etme
if (mesajKutusu.classList.contains("hata")) {
  console.log("Bu eleman hata sınıfına sahip.");
}

// Birden fazla sınıfı aynı anda ekleme
mesajKutusu.classList.add("basarili", "vurgulu");
```

**Ne zaman kullanılır:** Önceden tanımlı CSS stillerini açıp kapatmak için. `style` özelliğine göre çok daha temiz ve bakımı kolaydır. Gerçek projelerde stil değişiklikleri neredeyse her zaman `classList` ile yapılır.

### `style` mı `classList` mı?

| Durum | Tercih | Neden |
|-------|--------|-------|
| Kullanıcı bir renk seçiyor (dinamik değer) | `style` | Değer önceden bilinemez |
| Butona tıklayınca kutu görünsün/gizlensin | `classList` | Stil önceden CSS'te tanımlı |
| Hata durumunda input kırmızı olsun | `classList` | `.hata` sınıfı CSS'te hazır |
| Sürükle-bırak ile konum değiştirme | `style` | `left` ve `top` değerleri dinamik |

### Sınıf İçi Uygulama — Karanlık/Aydınlık Mod

```css
body.karanlik {
  background-color: #1a1a2e;
  color: #e0e0e0;
}
```

```html
<button id="modBtn">🌙 Karanlık Mod</button>
```

```javascript
const modBtn = document.querySelector("#modBtn");

modBtn.addEventListener("click", function() {
  document.body.classList.toggle("karanlik");

  if (document.body.classList.contains("karanlik")) {
    modBtn.textContent = "☀️ Aydınlık Mod";
  } else {
    modBtn.textContent = "🌙 Karanlık Mod";
  }
});
```

---

## Bölüm 5 · Eleman Oluşturma ve Silme

### Anlatım Notları

Şu ana kadar var olan elemanları seçip değiştirdik. Şimdi JavaScript ile sıfırdan yeni elemanlar oluşturmayı ve sayfaya eklemeyi öğreneceğiz. Bu beceri, dinamik içerik üretmenin (liste oluşturma, kart ekleme, tablo satırı ekleme) temelidir.

### `createElement` ve `appendChild`

```javascript
// 1. Yeni bir eleman oluştur (henüz sayfada değil, bellekte)
const yeniLi = document.createElement("li");

// 2. İçeriğini belirle
yeniLi.textContent = "Yeni görev";

// 3. CSS sınıfı ekle
yeniLi.classList.add("list-group-item");

// 4. Sayfadaki bir elemanın içine ekle
const liste = document.querySelector("#gorevListesi");
liste.appendChild(yeniLi);
```

**Adımları vurgulayın:** `createElement` elemanı sadece bellekte oluşturur; sayfada görünmesi için mutlaka bir üst elemana `appendChild` ile eklenmelidir.

### Daha Karmaşık Eleman Oluşturma

```javascript
// Bir Bootstrap kartı oluşturalım
function kartOlustur(baslik, icerik) {
  const kart = document.createElement("div");
  kart.classList.add("card", "mb-3");

  const kartGovde = document.createElement("div");
  kartGovde.classList.add("card-body");

  const kartBaslik = document.createElement("h5");
  kartBaslik.classList.add("card-title");
  kartBaslik.textContent = baslik;

  const kartIcerik = document.createElement("p");
  kartIcerik.classList.add("card-text");
  kartIcerik.textContent = icerik;

  // İç içe yerleştirme
  kartGovde.appendChild(kartBaslik);
  kartGovde.appendChild(kartIcerik);
  kart.appendChild(kartGovde);

  return kart;
}

// Kullanımı
const container = document.querySelector("#kartAlani");
container.appendChild(kartOlustur("JavaScript", "DOM konusunu öğreniyoruz."));
container.appendChild(kartOlustur("Bootstrap", "Kart bileşenleri kullanıyoruz."));
```

### Eleman Silme

```javascript
// Yöntem 1: Elemanın kendisini silme
const silinecek = document.querySelector("#eskiEleman");
silinecek.remove();

// Yöntem 2: Üst elemandan çocuğu silme
const liste = document.querySelector("#gorevListesi");
const ilkEleman = liste.firstElementChild;
liste.removeChild(ilkEleman);
```

### Tüm İçeriği Temizleme

```javascript
const liste = document.querySelector("#gorevListesi");
liste.innerHTML = ""; // Tüm alt elemanları siler
```

**Dikkat:** `innerHTML = ""` hızlı ve pratiktir ama büyük listelerde performans sorunu yaratabilir. Birkaç düzine eleman için sorunsuz çalışır.

---

## Bölüm 6 · Olaylar (Events)

### Anlatım Notları

Olaylar, kullanıcının sayfayla yaptığı etkileşimlerdir: tıklama, yazma, fare hareketleri, form gönderme gibi. JavaScript bu olayları "dinler" ve gerçekleştiğinde belirlenen fonksiyonu çalıştırır.

### `addEventListener` Yapısı

```javascript
const buton = document.querySelector("#buton");

buton.addEventListener("click", function() {
  console.log("Butona tıklandı!");
});
```

**Yapıyı parçalayarak açıklayın:**
1. `buton` → Hangi eleman dinlenecek?
2. `"click"` → Hangi olay dinlenecek?
3. `function() { ... }` → Olay gerçekleşince ne yapılacak?

### Sık Kullanılan Olaylar ve Kullanım Alanları

```javascript
// ── FARE OLAYLARI ──

// click: Eleman tıklandığında
buton.addEventListener("click", function() {
  console.log("Tek tıklama");
});

// dblclick: Çift tıklama
kutu.addEventListener("dblclick", function() {
  console.log("Çift tıklama algılandı");
});

// mouseover: Fare elemanın üzerine geldiğinde
kart.addEventListener("mouseover", function() {
  kart.style.boxShadow = "0 4px 8px rgba(0,0,0,0.2)";
});

// mouseout: Fare elemandan ayrıldığında
kart.addEventListener("mouseout", function() {
  kart.style.boxShadow = "none";
});


// ── KLAVYe OLAYLARI ──

// keydown: Bir tuşa basıldığı an
document.addEventListener("keydown", function(e) {
  console.log("Basılan tuş:", e.key);
});

// keyup: Tuş bırakıldığında
input.addEventListener("keyup", function(e) {
  console.log("Güncel değer:", e.target.value);
});


// ── FORM / INPUT OLAYLARI ──

// input: Input değeri her değiştiğinde (anlık)
arama.addEventListener("input", function(e) {
  console.log("Yazılan:", e.target.value);
});

// change: Değer değişip eleman odağı kaybettiğinde
// Select kutularında seçim değiştiğinde anında tetiklenir
sehirSelect.addEventListener("change", function(e) {
  console.log("Seçilen şehir:", e.target.value);
});

// submit: Form gönderildiğinde
form.addEventListener("submit", function(e) {
  e.preventDefault(); // Sayfanın yenilenmesini engelle!
  console.log("Form gönderildi");
});


// ── SAYFA OLAYLARI ──

// DOMContentLoaded: HTML tamamen okunduğunda
document.addEventListener("DOMContentLoaded", function() {
  console.log("Sayfa hazır, elemanlar erişilebilir.");
});
```

### Event Nesnesi (`e` veya `event`)

Olay dinleyicisine verilen fonksiyon otomatik olarak bir **event nesnesi** alır. Bu nesne olaya dair detaylı bilgi taşır.

```javascript
buton.addEventListener("click", function(e) {
  console.log(e.target);    // Tıklanan eleman
  console.log(e.type);      // Olay türü: "click"
});

input.addEventListener("keydown", function(e) {
  console.log(e.key);       // Basılan tuşun adı: "Enter", "a", "Escape"
  console.log(e.code);      // Tuşun fiziksel kodu: "KeyA", "Enter"

  if (e.key === "Enter") {
    console.log("Enter tuşuna basıldı!");
  }
});
```

### `e.preventDefault()` — Varsayılan Davranışı Engelleme

```javascript
// Form gönderiminde sayfanın yenilenmesini engelleme
form.addEventListener("submit", function(e) {
  e.preventDefault();
  // Artık sayfa yenilenmez, verilerle JavaScript'te işlem yapabiliriz
});

// Bir linkin sayfayı yönlendirmesini engelleme
link.addEventListener("click", function(e) {
  e.preventDefault();
  console.log("Link tıklandı ama sayfa değişmedi.");
});
```

**Ne zaman kullanılır:** Tarayıcının doğal davranışını durdurmak istediğinizde. Formlarla çalışırken neredeyse her zaman kullanacaksınız, çünkü form gönderildiğinde sayfa varsayılan olarak yenilenir ve JavaScript verileriniz kaybolur.

---

## Bölüm 7 · Form Elemanlarıyla Çalışma

### Anlatım Notları

Bu bölüm tüm önceki konuları birleştirir: eleman seçme, değer okuma, DOM'a ekleme ve olay dinleme. Öğrencilere "artık gerçek bir uygulama yapıyoruz" deyin. HTML-CSS-Bootstrap ile tasarladıkları formları JavaScript ile işlevsel hale getirecekler.

### Proje Yapısı

Tüm bu bölüm boyunca kullanılacak ortak HTML şablonu:

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Form İşlemleri</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
        rel="stylesheet">
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="container mt-5">
    <!-- İçerikler buraya gelecek -->
  </div>
  <script src="script.js"></script>
</body>
</html>
```

---

### 7.1 · Input ve Textarea'dan Değer Okuma

```html
<div class="mb-3">
  <label for="adInput" class="form-label">Adınız</label>
  <input type="text" id="adInput" class="form-control" placeholder="Adınızı yazın">
</div>
<div class="mb-3">
  <label for="mesajArea" class="form-label">Mesajınız</label>
  <textarea id="mesajArea" class="form-control" rows="3"
            placeholder="Mesajınızı yazın"></textarea>
</div>
<button id="gosterBtn" class="btn btn-primary">Göster</button>
<div id="sonucAlani" class="alert alert-info mt-3" style="display:none;"></div>
```

```javascript
const adInput = document.querySelector("#adInput");
const mesajArea = document.querySelector("#mesajArea");
const gosterBtn = document.querySelector("#gosterBtn");
const sonucAlani = document.querySelector("#sonucAlani");

gosterBtn.addEventListener("click", function() {
  const ad = adInput.value.trim();         // .trim() baş ve sondaki boşlukları siler
  const mesaj = mesajArea.value.trim();

  if (ad === "" || mesaj === "") {
    sonucAlani.textContent = "Lütfen tüm alanları doldurun!";
    sonucAlani.className = "alert alert-danger mt-3";  // Kırmızı uyarı
  } else {
    sonucAlani.textContent = `${ad} diyor ki: "${mesaj}"`;
    sonucAlani.className = "alert alert-success mt-3"; // Yeşil onay
  }

  sonucAlani.style.display = "block"; // Sonuç alanını görünür yap
});
```

**Satır satır açıklayın:**
- `.value` → Input veya textarea'nın içindeki metni alır.
- `.trim()` → Kullanıcı başa veya sona boşluk koymuşsa temizler.
- `.className` → Elemanın tüm sınıflarını sıfırlayıp yeniden atar. `classList.add/remove`'dan farkı: tüm sınıfları komple değiştirir.

---

### 7.2 · Select (Açılır Liste) ile Çalışma

```html
<div class="mb-3">
  <label for="sehirSelect" class="form-label">Şehir Seçin</label>
  <select id="sehirSelect" class="form-select">
    <option value="">-- Seçiniz --</option>
    <option value="istanbul">İstanbul</option>
    <option value="ankara">Ankara</option>
    <option value="izmir">İzmir</option>
    <option value="bursa">Bursa</option>
  </select>
</div>
<p id="sehirSonuc"></p>
```

```javascript
const sehirSelect = document.querySelector("#sehirSelect");
const sehirSonuc = document.querySelector("#sehirSonuc");

sehirSelect.addEventListener("change", function() {
  const secilen = sehirSelect.value;

  if (secilen === "") {
    sehirSonuc.textContent = "";
    return; // Fonksiyondan çık, aşağısı çalışmaz
  }

  // Seçili option'ın görünen metnini alma
  const secilenMetin = sehirSelect.options[sehirSelect.selectedIndex].text;
  sehirSonuc.textContent = `Seçtiğiniz şehir: ${secilenMetin}`;
});
```

**Açıklayın:**
- `sehirSelect.value` → Seçili option'ın `value` özelliğini verir (`"istanbul"`, `"ankara"` vb.).
- `sehirSelect.options` → Tüm option elemanlarının listesi.
- `sehirSelect.selectedIndex` → Seçili option'ın sıra numarası (0'dan başlar).
- `.text` → Option elemanının görünen metnini verir.

---

### 7.3 · Checkbox ve Radio Butonlarla Çalışma

```html
<h5>Hobiler (Checkbox — birden fazla seçilebilir)</h5>
<div class="form-check">
  <input class="form-check-input hobi" type="checkbox" value="muzik" id="muzik">
  <label class="form-check-label" for="muzik">Müzik</label>
</div>
<div class="form-check">
  <input class="form-check-input hobi" type="checkbox" value="spor" id="spor">
  <label class="form-check-label" for="spor">Spor</label>
</div>
<div class="form-check">
  <input class="form-check-input hobi" type="checkbox" value="okuma" id="okuma">
  <label class="form-check-label" for="okuma">Kitap Okuma</label>
</div>

<h5 class="mt-3">Cinsiyet (Radio — yalnızca biri seçilebilir)</h5>
<div class="form-check">
  <input class="form-check-input" type="radio" name="cinsiyet" value="erkek" id="erkek">
  <label class="form-check-label" for="erkek">Erkek</label>
</div>
<div class="form-check">
  <input class="form-check-input" type="radio" name="cinsiyet" value="kadin" id="kadin">
  <label class="form-check-label" for="kadin">Kadın</label>
</div>

<button id="hobiBtn" class="btn btn-primary mt-3">Seçimleri Göster</button>
<p id="hobiSonuc" class="mt-2"></p>
```

```javascript
const hobiBtn = document.querySelector("#hobiBtn");
const hobiSonuc = document.querySelector("#hobiSonuc");

hobiBtn.addEventListener("click", function() {

  // ── CHECKBOX: Seçili olanları toplama ──
  const checkboxlar = document.querySelectorAll(".hobi:checked");
  // ":checked" → yalnızca işaretli olanları seçer (CSS pseudo-class)

  const seciliHobiler = [];
  checkboxlar.forEach(function(cb) {
    seciliHobiler.push(cb.value);
  });

  // ── RADIO: Seçili olanı bulma ──
  const secilenCinsiyet = document.querySelector('input[name="cinsiyet"]:checked');
  // name="cinsiyet" olan radio'lar arasında işaretli olanı bul

  let sonucMetni = "";

  if (seciliHobiler.length > 0) {
    sonucMetni += "Hobiler: " + seciliHobiler.join(", ");
  } else {
    sonucMetni += "Hobi seçilmedi";
  }

  if (secilenCinsiyet) {
    sonucMetni += " | Cinsiyet: " + secilenCinsiyet.value;
  } else {
    sonucMetni += " | Cinsiyet seçilmedi";
  }

  hobiSonuc.textContent = sonucMetni;
});
```

**Vurgulayın:**
- `.checked` özelliği `true` veya `false` döner. Tek bir checkbox kontrol ederken kullanılır: `if (checkbox.checked) { ... }`
- `":checked"` CSS seçicisidir; `querySelectorAll` ile kullanarak sadece işaretli olanları seçebilirsiniz.
- Radio butonlarda aynı `name` özelliğine sahip olanlar bir grup oluşturur; gruptan yalnızca biri seçilebilir.

---

### 7.4 · Tam Form Gönderimi

```html
<form id="kayitFormu">
  <div class="mb-3">
    <label for="adSoyad" class="form-label">Ad Soyad</label>
    <input type="text" id="adSoyad" class="form-control" required>
  </div>
  <div class="mb-3">
    <label for="email" class="form-label">E-posta</label>
    <input type="email" id="email" class="form-control" required>
  </div>
  <div class="mb-3">
    <label for="sifre" class="form-label">Şifre</label>
    <input type="password" id="sifre" class="form-control" required>
  </div>
  <div class="mb-3">
    <label for="yas" class="form-label">Yaş</label>
    <input type="number" id="yas" class="form-control" min="1" max="120">
  </div>
  <button type="submit" class="btn btn-success">Kayıt Ol</button>
</form>

<div id="kayitSonuc" class="mt-3"></div>
```

```javascript
const kayitFormu = document.querySelector("#kayitFormu");
const kayitSonuc = document.querySelector("#kayitSonuc");

kayitFormu.addEventListener("submit", function(e) {
  e.preventDefault(); // Sayfa yenilenmesin!

  // Form elemanlarından değerleri toplama
  const adSoyad = document.querySelector("#adSoyad").value.trim();
  const email = document.querySelector("#email").value.trim();
  const sifre = document.querySelector("#sifre").value;
  const yas = document.querySelector("#yas").value;

  // ── Basit doğrulama (validation) ──
  const hatalar = [];

  if (adSoyad.length < 3) {
    hatalar.push("Ad soyad en az 3 karakter olmalıdır.");
  }

  if (!email.includes("@")) {
    hatalar.push("Geçerli bir e-posta adresi giriniz.");
  }

  if (sifre.length < 6) {
    hatalar.push("Şifre en az 6 karakter olmalıdır.");
  }

  if (yas !== "" && (Number(yas) < 1 || Number(yas) > 120)) {
    hatalar.push("Yaş 1 ile 120 arasında olmalıdır.");
  }

  // ── Sonucu gösterme ──
  if (hatalar.length > 0) {
    kayitSonuc.innerHTML = "";
    hatalar.forEach(function(hata) {
      const p = document.createElement("p");
      p.textContent = "⚠ " + hata;
      p.style.color = "red";
      kayitSonuc.appendChild(p);
    });
  } else {
    kayitSonuc.innerHTML = "";

    const basariMesaji = document.createElement("div");
    basariMesaji.classList.add("alert", "alert-success");
    basariMesaji.textContent = `Hoş geldiniz, ${adSoyad}! Kayıt başarılı.`;
    kayitSonuc.appendChild(basariMesaji);

    kayitFormu.reset(); // Formu temizle (tüm alanları sıfırla)
  }
});
```

**Satır satır açıklayın:**
- `e.preventDefault()` → Formun normal gönderimini (sayfa yenileme) engeller.
- `.trim()` → Boşluk kontrolü öncesi temizlik.
- `hatalar` dizisi → Tüm hataları toplar, sonra tek seferde gösterir.
- `kayitFormu.reset()` → Tüm form elemanlarını başlangıç durumuna döndürür.

---

## Bölüm 8 · Birleştirici Proje — Öğrenci Kayıt Sistemi

### Anlatım Notları

Bu proje rehberdeki tüm konuları tek bir uygulamada birleştirir. Adım adım birlikte kodlayın. Her adımda hangi konunun kullanıldığını belirtin.

### HTML

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Öğrenci Kayıt Sistemi</title>
  <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css"
        rel="stylesheet">
  <style>
    .arama-vurgulu { background-color: #fff3cd; }
  </style>
</head>
<body>
  <div class="container mt-5">
    <h1 class="mb-4">Öğrenci Kayıt Sistemi</h1>

    <!-- Kayıt Formu -->
    <div class="card mb-4">
      <div class="card-header">
        <h5 class="mb-0">Yeni Öğrenci Ekle</h5>
      </div>
      <div class="card-body">
        <form id="ogrenciFormu">
          <div class="row">
            <div class="col-md-4 mb-3">
              <label for="ogrenciAd" class="form-label">Ad Soyad</label>
              <input type="text" id="ogrenciAd" class="form-control" required>
            </div>
            <div class="col-md-4 mb-3">
              <label for="ogrenciBolum" class="form-label">Bölüm</label>
              <select id="ogrenciBolum" class="form-select" required>
                <option value="">Seçiniz</option>
                <option value="Bilgisayar Mühendisliği">Bilgisayar Mühendisliği</option>
                <option value="Elektrik Mühendisliği">Elektrik Mühendisliği</option>
                <option value="Makine Mühendisliği">Makine Mühendisliği</option>
                <option value="İşletme">İşletme</option>
              </select>
            </div>
            <div class="col-md-4 mb-3">
              <label for="ogrenciNot" class="form-label">Not Ortalaması</label>
              <input type="number" id="ogrenciNot" class="form-control"
                     min="0" max="100" required>
            </div>
          </div>
          <button type="submit" class="btn btn-primary">Öğrenci Ekle</button>
        </form>
      </div>
    </div>

    <!-- Arama ve Filtre -->
    <div class="row mb-3">
      <div class="col-md-6">
        <input type="text" id="aramaInput" class="form-control"
               placeholder="İsme göre ara...">
      </div>
      <div class="col-md-3">
        <select id="bolumFiltre" class="form-select">
          <option value="hepsi">Tüm Bölümler</option>
          <option value="Bilgisayar Mühendisliği">Bilgisayar Müh.</option>
          <option value="Elektrik Mühendisliği">Elektrik Müh.</option>
          <option value="Makine Mühendisliği">Makine Müh.</option>
          <option value="İşletme">İşletme</option>
        </select>
      </div>
      <div class="col-md-3">
        <button id="istatistikBtn" class="btn btn-info w-100">İstatistikleri Göster</button>
      </div>
    </div>

    <!-- İstatistik Alanı -->
    <div id="istatistikAlani" class="alert alert-secondary mb-3" style="display:none;"></div>

    <!-- Öğrenci Tablosu -->
    <table class="table table-striped table-hover">
      <thead class="table-dark">
        <tr>
          <th>#</th>
          <th>Ad Soyad</th>
          <th>Bölüm</th>
          <th>Not Ortalaması</th>
          <th>Durum</th>
          <th>İşlem</th>
        </tr>
      </thead>
      <tbody id="ogrenciTablosu">
        <!-- JavaScript ile doldurulacak -->
      </tbody>
    </table>

    <p id="kayitSayisi" class="text-muted"></p>
  </div>

  <script src="script.js"></script>
</body>
</html>
```

### JavaScript

```javascript
// ============================================================
//  VERİ — Öğrencileri bir dizide tutuyoruz
// ============================================================
const ogrenciler = [];
let siradakiId = 1;


// ============================================================
//  ELEMAN SEÇİMLERİ  (Bölüm 2)
// ============================================================
const ogrenciFormu   = document.querySelector("#ogrenciFormu");
const ogrenciAd      = document.querySelector("#ogrenciAd");
const ogrenciBolum   = document.querySelector("#ogrenciBolum");
const ogrenciNot     = document.querySelector("#ogrenciNot");
const ogrenciTablosu = document.querySelector("#ogrenciTablosu");
const kayitSayisi    = document.querySelector("#kayitSayisi");
const aramaInput     = document.querySelector("#aramaInput");
const bolumFiltre    = document.querySelector("#bolumFiltre");
const istatistikBtn  = document.querySelector("#istatistikBtn");
const istatistikAlani = document.querySelector("#istatistikAlani");


// ============================================================
//  YARDIMCI FONKSİYONLAR
// ============================================================

// Not ortalamasına göre harf notu ve durum belirle
function durumBelirle(not) {
  if (not >= 90) return { harf: "AA", renk: "success" };
  if (not >= 80) return { harf: "BA", renk: "success" };
  if (not >= 70) return { harf: "BB", renk: "primary" };
  if (not >= 60) return { harf: "CC", renk: "warning" };
  if (not >= 50) return { harf: "DC", renk: "warning" };
  return { harf: "FF", renk: "danger" };
}


// ============================================================
//  TABLOYU GÜNCELLEME  (Bölüm 3, 4, 5 — içerik, stil, oluşturma)
// ============================================================

function tabloGuncelle(listelenecekler) {
  // Tabloyu temizle
  ogrenciTablosu.innerHTML = "";

  if (listelenecekler.length === 0) {
    const tr = document.createElement("tr");
    const td = document.createElement("td");
    td.setAttribute("colspan", "6");
    td.classList.add("text-center", "text-muted");
    td.textContent = "Kayıt bulunamadı.";
    tr.appendChild(td);
    ogrenciTablosu.appendChild(tr);
    kayitSayisi.textContent = "";
    return;
  }

  listelenecekler.forEach(function(ogr, index) {
    const durum = durumBelirle(ogr.not);

    // Tablo satırı oluştur
    const tr = document.createElement("tr");

    // Sıra numarası
    const tdSira = document.createElement("td");
    tdSira.textContent = index + 1;
    tr.appendChild(tdSira);

    // Ad Soyad
    const tdAd = document.createElement("td");
    tdAd.textContent = ogr.ad;
    tr.appendChild(tdAd);

    // Bölüm
    const tdBolum = document.createElement("td");
    tdBolum.textContent = ogr.bolum;
    tr.appendChild(tdBolum);

    // Not
    const tdNot = document.createElement("td");
    tdNot.textContent = ogr.not;
    tr.appendChild(tdNot);

    // Durum (harf notu rozeti)
    const tdDurum = document.createElement("td");
    const span = document.createElement("span");
    span.classList.add("badge", "bg-" + durum.renk);
    span.textContent = durum.harf;
    tdDurum.appendChild(span);
    tr.appendChild(tdDurum);

    // Sil butonu
    const tdIslem = document.createElement("td");
    const silBtn = document.createElement("button");
    silBtn.textContent = "Sil";
    silBtn.classList.add("btn", "btn-danger", "btn-sm");

    silBtn.addEventListener("click", function() {
      // Diziden kaldır
      const silinecekIndex = ogrenciler.findIndex(function(o) {
        return o.id === ogr.id;
      });
      if (silinecekIndex !== -1) {
        ogrenciler.splice(silinecekIndex, 1);
      }
      filtreUygula();
    });

    tdIslem.appendChild(silBtn);
    tr.appendChild(tdIslem);

    ogrenciTablosu.appendChild(tr);
  });

  kayitSayisi.textContent = `Toplam ${listelenecekler.length} kayıt gösteriliyor.`;
}


// ============================================================
//  FİLTRELEME ve ARAMA  (Bölüm 6, 7 — olaylar ve form elemanları)
// ============================================================

function filtreUygula() {
  const aramaMetni = aramaInput.value.trim().toLowerCase();
  const secilenBolum = bolumFiltre.value;

  const filtrelenmis = ogrenciler.filter(function(ogr) {
    // İsim araması
    const isimUyuyor = ogr.ad.toLowerCase().includes(aramaMetni);

    // Bölüm filtresi
    const bolumUyuyor = (secilenBolum === "hepsi") || (ogr.bolum === secilenBolum);

    return isimUyuyor && bolumUyuyor;
  });

  tabloGuncelle(filtrelenmis);
}


// ============================================================
//  OLAY DİNLEYİCİLER  (Bölüm 6)
// ============================================================

// Form gönderimi
ogrenciFormu.addEventListener("submit", function(e) {
  e.preventDefault();

  const ad    = ogrenciAd.value.trim();
  const bolum = ogrenciBolum.value;
  const not   = Number(ogrenciNot.value);

  // Doğrulama
  if (ad === "" || bolum === "" || isNaN(not)) {
    alert("Lütfen tüm alanları doğru şekilde doldurun.");
    return;
  }

  if (not < 0 || not > 100) {
    alert("Not 0 ile 100 arasında olmalıdır.");
    return;
  }

  // Yeni öğrenci nesnesini diziye ekle
  ogrenciler.push({
    id: siradakiId++,
    ad: ad,
    bolum: bolum,
    not: not
  });

  // Formu temizle
  ogrenciFormu.reset();
  ogrenciAd.focus(); // İmleci tekrar isim alanına getir

  // Tabloyu güncelle
  filtreUygula();
});

// Anlık arama (her tuş vuruşunda)
aramaInput.addEventListener("input", function() {
  filtreUygula();
});

// Bölüm filtresi değiştiğinde
bolumFiltre.addEventListener("change", function() {
  filtreUygula();
});

// İstatistikler
istatistikBtn.addEventListener("click", function() {
  if (ogrenciler.length === 0) {
    istatistikAlani.textContent = "Henüz kayıtlı öğrenci yok.";
    istatistikAlani.style.display = "block";
    return;
  }

  const toplamOgrenci = ogrenciler.length;

  const notToplam = ogrenciler.reduce(function(toplam, ogr) {
    return toplam + ogr.not;
  }, 0);
  const notOrtalama = (notToplam / toplamOgrenci).toFixed(1);

  const enYuksek = ogrenciler.reduce(function(max, ogr) {
    return ogr.not > max.not ? ogr : max;
  });

  const enDusuk = ogrenciler.reduce(function(min, ogr) {
    return ogr.not < min.not ? ogr : min;
  });

  const gecenler = ogrenciler.filter(function(ogr) {
    return ogr.not >= 50;
  }).length;

  const basariOrani = ((gecenler / toplamOgrenci) * 100).toFixed(0);

  istatistikAlani.innerHTML =
    `<strong>Toplam:</strong> ${toplamOgrenci} öğrenci | ` +
    `<strong>Ortalama:</strong> ${notOrtalama} | ` +
    `<strong>En yüksek:</strong> ${enYuksek.ad} (${enYuksek.not}) | ` +
    `<strong>En düşük:</strong> ${enDusuk.ad} (${enDusuk.not}) | ` +
    `<strong>Başarı oranı:</strong> %${basariOrani}`;
  istatistikAlani.style.display = "block";
});


// ============================================================
//  BAŞLANGIÇ — Sayfa yüklendiğinde boş tabloyu göster
// ============================================================
tabloGuncelle(ogrenciler);
```

---

## Bölüm 9 · Ders Akış Önerisi

| Ders | Konu | Yapılacak Etkinlik |
|------|------|--------------------|
| 1 | DOM nedir, eleman seçme yöntemleri | Konsolda `querySelector` denemeleri |
| 2 | İçerik okuma/değiştirme, `textContent` vs `innerHTML` vs `value` | Sayaç uygulaması |
| 3 | Stil ve sınıf değiştirme (`style`, `classList`) | Karanlık/Aydınlık mod geçişi |
| 4 | Eleman oluşturma ve silme | Dinamik kart listesi oluşturma |
| 5 | Olaylar ve event nesnesi | Klavye/fare olaylarını dinleme |
| 6 | Form elemanları: input, textarea, select | Değer okuma ve gösterme uygulamaları |
| 7 | Checkbox, radio ve form doğrulama | Kayıt formu ile doğrulama |
| 8 | Birleştirici proje: Öğrenci Kayıt Sistemi | Tüm konuların birlikte kullanımı |

---

## Ek A · Sık Yapılan Hatalar ve Çözümleri

| Hata | Neden | Çözüm |
|------|-------|-------|
| `Cannot read properties of null` | Eleman bulunamadı | ID veya seçici yazımını kontrol edin; `<script>` etiketinin `</body>` öncesinde olduğundan emin olun |
| Form gönderince sayfa yenileniyor | `e.preventDefault()` unutulmuş | `submit` olay dinleyicisine `e.preventDefault()` ekleyin |
| Input değeri her zaman boş geliyor | `value` yerine `textContent` kullanılmış | Form elemanları için her zaman `.value` kullanın |
| Stil değişikliği çalışmıyor | CSS özellik adı yanlış yazılmış | `background-color` değil `backgroundColor` (camelCase) |
| `querySelectorAll` sonucu üzerinde `addEventListener` çalışmıyor | NodeList tek eleman değil | `forEach` ile her elemana ayrı ayrı `addEventListener` ekleyin |
| Checkbox/radio durumu alınamıyor | `value` yerine `checked` gerekli | İşaretli mi diye `.checked` kullanın, seçili olanları bulmak için `":checked"` seçicisi |

---

## Ek B · Ödev Fikirleri

1. **Not Defteri** — Kullanıcı not yazar, "Ekle" der, notlar kart olarak alt alta sıralanır; her kartta "Sil" butonu bulunur.
2. **Alışveriş Listesi** — Ürün adı ve fiyat girilerek listeye eklenir; toplam tutar otomatik hesaplanır; ürünler silinebilir.
3. **Quiz Uygulaması** — Çoktan seçmeli sorular radio butonlarla sunulur; "Bitir" butonuyla doğru/yanlış sayısı hesaplanıp sonuç gösterilir.
4. **Hava Durumu Kartı** — Şehir seçimi (select) ve sıcaklık girişi (input) ile sahte bir hava durumu kartı oluşturulur; sıcaklığa göre kart rengi değişir.
5. **Basit Hesap Makinesi** — İki sayı girişi (input), işlem seçimi (select: toplama, çıkarma, çarpma, bölme) ve "Hesapla" butonu ile sonuç gösterilir.
