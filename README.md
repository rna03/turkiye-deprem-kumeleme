# Türkiye Deprem Kayıtlarının K-Means ile Kümelenmesi

Türkiye ve çevresindeki 50.000 geçmiş deprem kaydının konum, derinlik ve büyüklük özellikleri kullanılarak gruplandırıldığı denetimsiz öğrenme projesidir.

**Çalışma geçmiş kayıtları özetler; gelecekteki depremleri tahmin etmez ve deprem risk haritası üretmez.**

[Notebook'u incele](Turkiye_Deprem_Kumeleme.ipynb) · [Google Colab'de aç](https://colab.research.google.com/github/rna03/turkiye-deprem-kumeleme/blob/main/Turkiye_Deprem_Kumeleme.ipynb)

## Amaç

Benzer özelliklere sahip deprem kayıtlarını gruplamak, kümelerin profillerini açıklamak ve sonuçların veri hazırlama kararlarına karşı duyarlılığını incelemek.

## Veri Seti

- **Kaynak:** [Turkey Earthquake Data — Kaggle](https://www.kaggle.com/datasets/ozgecinko/turkey-earthquake-data-1914-2023)
- **Dosya:** `veriler.csv`
- **Boyut:** 50.000 kayıt, 15 sütun
- **Dosyadan doğrulanan tarih aralığı:** 11.09.1994–31.07.2023
- **Model girdileri:** Enlem, boylam, derinlik ve dosyadaki xM büyüklük değeri

1994 ve 2023 kısmi yıllardır. Veride hazır gerçek küme etiketleri bulunmamaktadır. Ham veri depoya eklenmemiştir; kaynak bağlantısından indirilmelidir.

## Kullanılan Teknolojiler

Python, pandas, NumPy, Matplotlib, scikit-learn ve Google Colab.

## Analiz Süreci

1. Eksik değer, tarih, tekrarlı kayıt ve derinlik kontrolleri yapıldı.
2. Konum, büyüklük, derinlik ve yıllık kayıt dağılımları görselleştirildi.
3. Derinliğe `log1p` dönüşümü uygulandı.
4. Dört özellik `StandardScaler` ile standartlaştırıldı.
5. K-Means ile 2–10 arasındaki küme sayıları karşılaştırıldı.
6. Silhouette, Davies–Bouldin ve Elbow grafiği değerlendirilerek K=5 seçildi.
7. Küme profilleri ve konumsal dağılımları incelendi.
8. Sıfır derinlikli kayıtları dışlamanın etkisi ve farklı başlangıçlara karşı kararlılık değerlendirildi.
9. Küme bazında Silhouette değerleriyle zayıf ayrışan gruplar incelendi.

K-Means için `init="k-means++"`, `n_init=20` ve ana deneyde `random_state=42` kullanıldı.

Modeller tüm kayıtlarla oluşturuldu. Silhouette, her küme sayısı için aynı 3.000 kayıtlık örneklemde hesaplandı. Amaç mevcut kataloğu betimlemek olduğundan ayrı bir tahmin test kümesi kullanılmadı.

## Temel Sonuçlar

| Ölçüt | Sonuç |
|---|---:|
| Seçilen küme sayısı | 5 |
| Silhouette — 3.000 kayıtlık örneklem | 0,2874 |
| Davies–Bouldin — tüm kayıtlar | 1,1002 |
| Örneklemde negatif Silhouette oranı | %5,00 |
| Sıfır derinlikler dahil/hariç ARI — ortak kayıtlar | 0,6959 |
| Tüm veride farklı başlangıçların referansla ARI aralığı | 0,9999–1,0000 |

**Bu değerler doğruluk yüzdesi değildir.** Model denenen başlangıçlarda kararlı sonuçlar üretmiştir; ancak kümeler kısmen örtüşmektedir. Sıfır derinlikli kayıtların çıkarılması, başlangıç değişikliklerine kıyasla daha belirgin bir değişim oluşturmuştur.

## Küme Profilleri

| Küme | Kayıt sayısı | Öne çıkan profil |
|---|---:|---|
| 1 | 3.485 | Çok düşük kayıtlı derinlik; üyelerinin yaklaşık yarısı sıfır derinlikli |
| 2 | 15.250 | Kuzeybatı ağırlıklı, en kalabalık grup |
| 3 | 4.590 | Ortalama büyüklüğü en yüksek, farklı konumlara yayılmış grup |
| 4 | 12.371 | Güneybatı ağırlıklı, ortalama ve medyan derinliği en yüksek grup |
| 5 | 14.304 | Doğu ağırlıklı grup |

Bu tanımlar göreli kayıt profilleridir. Küme numaraları tehlike veya risk sıralaması değildir.

## Çalıştırma

1. Üstteki **Google Colab'de aç** bağlantısına tıklayın.
2. Kaggle kaynağından içinde `veriler.csv` bulunan ZIP dosyasını indirin.
3. Notebook hücrelerini baştan sona çalıştırın.
4. İlk hücrede dosya seçimi istendiğinde ZIP dosyasını yükleyin.

Notebook, Google Colab'in dosya yükleme arayüzünü kullanır. ZIP içindeki CSV dosyasının adı `veriler.csv` olmalıdır.

## Sınırlılıklar

- Sıfır derinlik değerlerinin ve Ke/Sm kayıt türlerinin anlamları doğrulanmamıştır.
- xM alanının tek ve homojen bir Mw ölçeği olduğu varsayılmamıştır.
- Standartlaştırılmış enlem-boylam uzaklıkları kilometre cinsinden değildir.
- K-Means, uzamış ve düzensiz grupları temsil etmekte sınırlı kalabilir.
- Silhouette sonuçları 3.000 kayıtlık örnekleme dayanmaktadır.
- Gerçek küme etiketleri ve bağımsız uzman doğrulaması yoktur.
- Artçı deprem dizilerinin etkisi ayrıca incelenmemiştir.
- Sonuçlar kullanılan katalog kesiti ve ön işleme tercihleriyle sınırlıdır.

## Hazırlayan

Rana Yılmaz
