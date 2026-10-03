# CLAUDE.md

Bu klasör Yağmur'un HTML oyunları. GitHub: https://github.com/yagmursendag/oyunlar, site: https://yagmursendag.github.io/oyunlar/ (GitHub Pages, `main` dalı, kök klasör). Her oyunun kendi klasöründe ayrıca CLAUDE.md / README.md var.

Yağmur kod yazmayı yeni öğreniyor: her şeyi sade Türkçe anlat, ne yaptığını kısaca öğret. Arayüz metinleri Türkçe, kod yorumları İngilizce.

## Geliştirme kuralları (Yağmur ile birlikte belirlendi)

Her yeni istek şu sırayla yapılır:

1. **Geliştir (bu bilgisayarda):** Yeni özellik = yeni sürüm dosyası `oyun/versions/v{N}-{kisa-turkce-ad}.html`. Eski sürümler asla silinmez ve değiştirilmez.
2. **Test et:** Claude test eder: sözdizimi kontrolü, headless Chrome ile ekran görüntüleri (masaüstü 1280x720 ve telefon yatay ~844x390, dokunmatik taklidi), oyun mantığı testleri. Görüntülerde görülen kusurlar düzeltilir. Yağmur "önce ben deneyeyim" derse yayından önce bilgisayarda açıp gösterilir.
3. **Yayınla:** Testler geçince ayrıca sormadan:
   - `index.html` yeni sürümün birebir kopyası yapılır,
   - oyunun README tablosuna Türkçe "ne eklendi" satırı eklenir,
   - yv-salon'da `sw.js` içindeki `CACHE` adı yeni sürüme çevrilir (telefonlar yeni sürümü alsın diye),
   - anlaşılır Türkçe bir commit mesajıyla `git commit` + `git push` yapılır,
   - birkaç dakika sonra canlı adresin yeni sürümü verdiği kontrol edilir.
4. **Haber ver:** Ne değiştiğini, test sonuçlarını ve telefonda neye bakması gerektiğini kısaca yaz; oyun linkini ver. Yağmur telefondan kontrol eder, sorun varsa sonraki sürümde düzeltilir.

Testler geçmezse ya da bir şey yarım kalırsa yayınlanmaz; durum açıkça söylenir.

## Dikkat

- Depo herkese açık: şifre, kişisel bilgi, e-posta asla dosyalara yazılmaz (admin şifresi koda girmez, sadece açık anahtar var).
- Git kimliği bu depoda: `yagmursendag` / `337370565+yagmursendag@users.noreply.github.com`.
- Yeni kurulan programlar için PowerShell'de PATH yenile: `$env:Path = [Environment]::GetEnvironmentVariable('Path','Machine') + ';' + [Environment]::GetEnvironmentVariable('Path','User')`.
- Depoyu silmek, geçmişi yeniden yazmak (force push), herkese açık/özel ayarını değiştirmek gibi geri alınamaz işler için her zaman önce sor.
