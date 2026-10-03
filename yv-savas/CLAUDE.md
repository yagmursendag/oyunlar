# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Proje

**YV Su Savaşı** — iki takımlı (Mavi / Turuncu) çocuk su savaşı oyunu. Tek dosyalık HTML + vanilla JS; build, paket yöneticisi, test ya da git yok. Hedef oyuncu bir çocuk (varsayılan oyuncu adları "Yağmur" ve "Baba"), bu yüzden tüm arayüz metni **Türkçe** ve sade; kod yorumları İngilizce.

Çalıştırmak: `index.html` dosyasını tarayıcıda aç (`start "" index.html`). Doğrulama da böyle yapılır — otomatik test yok.

Dış bağımlılıklar yalnızca CDN'den: three.js **r128** (`cdnjs`) ve Google Fonts "Baloo 2". İnternet yoksa 3D otomatik olarak 2D'ye düşer (`D3.ok` / `use3D()`); her yeni özellik bu 2D yolunda da çalışmalı.

## Sürüm akışı (README'deki kural — uy)

- `index.html` her zaman **en son sürümün birebir kopyası**dır (şu an `versions/v12-...html` ile aynı).
- Her yeni özellik = **yeni sürüm dosyası**: `versions/v{N}-{kisa-turkce-slug}.html`. Eski sürümler **asla silinmez/değiştirilmez**.
- Yeni sürüm eklerken: `versions/` altına dosyayı yaz, `index.html`'i onunla aynı yap, `README.md` tablosuna Türkçe bir "Ne eklendi" satırı ekle.

## Mimari (`index.html` tek `<script>` bloğu)

Kod `// ---------- bölüm ----------` başlıklarıyla ayrılmış; sıra: setup → character designer → sound → input → state → Sünger Bob → actions → update → drawing helpers → character (2D) → pets → menu previews → 3D character designer → maps → 3D game view → flow.

- **Koordinat sistemi:** Oyun mantığı tamamen 2D mantıksal uzayda (`W=1280, H=720`, `MID` saha ortası, `TOP`/`BOT` oyun alanı sınırları) çalışır. Takım 0 solda, takım 1 sağda (`bounds(t)`); havuzlar `POOLS`.
- **İki render yolu, tek simülasyon:** `#c` 2D canvas ve üstünde `#g3` WebGL canvas. `draw()` → `use3D()` ise `draw3D()` yoksa `draw2D()`. 3D görünüm aynı `chars/pets/drops/balloons` durumunu `proj()` ile 3D sahneye yansıtır (`updCharModel`, `updPetModel`); ayrı fizik yok. Görsel bir değişiklik genelde **hem 2D hem 3D** tarafında yapılmalı.
- **Karakter görünümü (`look` objesi):** `OPTS` dizisi tasarım ekranı UI'ını veriden üretir (`buildDesigner`). Yeni bir özelleştirme seçeneği eklemek şu yerlere dokunur: `OPTS`, `DEFAULT_LOOKS`, `randomLook()`, 2D çizim (`drawChar` ve yardımcıları: `drawHairBack/Front`, `drawEars`, `drawEyes2D`...), 3D model (`buildCharModel`).
- **Evcil hayvan eklemek:** `PETS` (konuşma + ses), `OPTS` içindeki `pet` öğeleri, `drawPet` (2D), `buildPet3D` (3D), `petUpdate` davranışı, `#drand` rastgele listesi ve dosya sonundaki emoji→kind ses eşlemesi.
- **Haritalar:** `THEMES` (zemin), `ITEMS` (yerleştirilebilir nesneler; engel/havuz/fıskiye davranışı `update` ve `collide` içinde), `PRESET_MAPS` + kullanıcı `customMaps`. Seçim id'leri `p{i}` (hazır) / `c{i}` (özel). Editör: `openMapper`/`drawMapper`/`placeAt` (simetrik yerleştirme).
- **Kalıcılık:** Tek `localStorage` anahtarı `yv-savas` (`looks`, isimler, görünüm, `maps`, `mapSel`); `saveSettings()` ile yazılır. Eski sürümlerin kayıtları hâlâ okunuyor — `normLook()` (v2/v3 `style` → saç parçaları) ve `hat:'bunny'` → `ears:'bunny'` göçü gibi. Bir alanı yeniden adlandırır/kaldırırsan aynı yere göç kodu ekle; yeni alanlar `DEFAULT_LOOKS` ile birleştirilerek varsayılan alır.
- **Kontroller:** `KEYS` — Oyuncu 1 WASD + F/Space (su) + G/E (balon); Oyuncu 2 oklar + Enter/Numpad0 + Sağ Shift/Ctrl. `P` duraklat, `Esc` menü. Ses Web Audio ile sentezleniyor (`tone`, `noise`), dosya yok.
- **Akış:** `state` = `menu` | `play` | `over`; `begin(mode)` / `startGame` / `showMenu` / `showOver`; tek `requestAnimationFrame(frame)` döngüsü menü önizlemelerini, 3D tasarım ekranını ve harita editörünü de çizer.
