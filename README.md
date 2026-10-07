# Droneport Destekli İHA Görevleri İçin Enerji Tahmini ve Karar Destek Sistemi

Bu projede, Droneport destekli insansız hava aracı görevlerinin enerji ihtiyacını tahmin eden ve batarya koşullarına göre görev uygunluğunu değerlendiren makine öğrenmesi tabanlı bir karar destek sistemi geliştirilmiştir.

Proje; sentetik veri üretimi, veri temizleme, keşifsel veri analizi, regresyon modellerinin karşılaştırılması, hiperparametre optimizasyonu, hata analizi, batarya uygunluk değerlendirmesi ve senaryo tabanlı iyileştirme aşamalarından oluşmaktadır.

> **Önemli not:** Bu çalışma sentetik verilerle geliştirilmiş bir kavram kanıtlama projesidir. Gerçek bir İHA veya Droneport sisteminin teknik özelliklerini ve performansını temsil etmemektedir.

---

## Projenin Amaçları

Projenin temel amaçları aşağıdaki gibidir:

- Görev ve çevre koşullarına göre enerji ihtiyacını tahmin etmek,
- Farklı regresyon modellerinin performanslarını karşılaştırmak,
- En başarılı modelin hiperparametrelerini optimize etmek,
- Batarya sağlığı ve başlangıç doluluk oranına göre görev uygunluğunu belirlemek,
- Model kapsamının dışındaki girdileri tespit etmek,
- Riskli veya uygun olmayan görevler için alternatif iyileştirme önerileri oluşturmak.

---

## Kullanılan Görev Değişkenleri

Makine öğrenmesi modelinde aşağıdaki giriş özellikleri kullanılmıştır:

- Toplam görev mesafesi
- Ortalama uçuş hızı
- Operasyon süresi
- Toplam görev süresi
- Faydalı yük ağırlığı
- Ortalama rüzgâr hızı
- Ortam sıcaklığı
- Batarya sıcaklığı

Modelin tahmin ettiği hedef değişken:

- Görev için gerekli enerji miktarı (`Wh`)

Başlangıç batarya seviyesi ve batarya sağlık oranı, enerji tahmininden sonra görev uygunluğunun değerlendirilmesinde kullanılmıştır.

---

## Proje Aşamaları

### 1. Sentetik Veri Üretimi ve Temizleme

Her satırın bir İHA görevini temsil ettiği 5.000 kayıtlık sentetik veri kümesi oluşturulmuştur.

Veri üretiminde aşağıdaki ilişkiler dikkate alınmıştır:

- Görev mesafesi, uçuş hızı ve görev süresi arasındaki ilişki,
- Rüzgâr hızının doğrusal olmayan etkisi,
- Rüzgâr ile faydalı yük arasındaki etkileşim,
- Ortam ve batarya sıcaklıklarının ideal değerlerden uzaklaşması,
- Şarj döngüsü ile batarya sağlığı arasındaki ilişki.

Veri ön işleme yöntemlerinin test edilmesi amacıyla kontrollü eksik ve geçersiz değerler oluşturulmuştur. Matematiksel olarak geri hesaplanabilen değerler yeniden oluşturulmuş, diğer eksik değerlerde uygun doldurma yöntemleri uygulanmıştır.

Temizleme sonunda 4.997 kayıtlık doğrulanmış veri kümesi elde edilmiştir.

### 2. Makine Öğrenmesi Modelleri

Aşağıdaki regresyon yöntemleri değerlendirilmiştir:

- Doğrusal Regresyon
- Karar Ağacı Regresyonu
- Rastgele Orman Regresyonu
- Gradyan Artırma Regresyonu

Modellerin değerlendirilmesinde aşağıdaki ölçütler kullanılmıştır:

- MAE
- RMSE
- R²

Beş katlı çapraz doğrulama sonucunda en başarılı yöntem gradyan artırma modeli olmuştur. Model üzerinde rastgele hiperparametre araması uygulanmış ve 60 farklı parametre birleşimi değerlendirilmiştir.

### 3. Karar Destek ve Optimizasyon Sistemi

Tahmin edilen enerji ihtiyacı; batarya kapasitesi, batarya sağlık oranı ve başlangıç doluluk seviyesiyle birlikte değerlendirilmiştir.

Görevler aşağıdaki durumlarla sınıflandırılmıştır:

- **Uygun:** Kalan batarya oranı %20 veya daha fazla
- **Riskli:** Kalan batarya oranı %10 ile %20 arasında
- **Uygun Değil:** Kalan batarya oranı %10'un altında veya mevcut enerji yetersiz
- **Manuel İnceleme Gerekli:** En az bir görev girdisi modelin eğitim kapsamı dışında

Riskli veya uygun olmayan görevler için aşağıdaki bağımsız seçenekler araştırılmıştır:

- Başlangıç batarya seviyesinin artırılması
- Faydalı yükün azaltılması
- Daha düşük rüzgâr hızının beklenmesi
- Görev mesafesinin azaltılması
- Daha sağlıklı batarya kullanılması

---

## Model Sonuçları

Optimize edilen gradyan artırma modeli, hiperparametre seçiminde kullanılmayan 1.000 görevlik test kümesinde aşağıdaki sonuçları vermiştir:

| Ölçüt | Sonuç |
|---|---:|
| MAE | 8,9065 Wh |
| RMSE | 12,3851 Wh |
| R² | 0,9758 |

Yalnızca toplam görev süresini kullanan temel modele göre:

- MAE değeri yaklaşık %59,50 azaltılmıştır.
- RMSE değeri yaklaşık %59,17 azaltılmıştır.
- Açıklanan değişim oranı %85,51'den %97,58'e yükseltilmiştir.

Test görevlerinin:

- %66'sında mutlak hata 10 Wh veya daha az,
- %92,1'inde mutlak hata 20 Wh veya daha az,
- %98,2'sinde mutlak hata 30 Wh veya daha az olarak hesaplanmıştır.

Modelin en büyük hatası, veri kümesindeki tek 600 Wh üzeri görevde görülmüştür. Bu nedenle modelin eğitim aralığı dışındaki görevlerde otomatik onay verilmemektedir.

---

## Özellik Önemi Sonuçları

Permütasyon önemine göre enerji tahmininde en etkili değişkenler:

1. Toplam görev süresi
2. Rüzgâr hızı
3. Faydalı yük ağırlığı
4. Ortalama uçuş hızı
5. Ortam sıcaklığı

Özellik önemleri doğrudan fiziksel nedensellik olarak değil, modelin tahmin sürecindeki göreli kullanım düzeyi olarak değerlendirilmiştir.

---

## Proje Yapısı

```text
droneport-enerji-optimizasyonu/
│
├── data/
│   ├── droneport_temel_gorev_verileri.csv
│   ├── droneport_ham_gorev_verileri.csv
│   ├── droneport_temiz_gorev_verileri.csv
│   └── README.md
│
├── models/
│   ├── optimize_gradyan_artirma_modeli.joblib
│   ├── model_metadata.json
│   └── README.md
│
├── notebooks/
│   ├── Veri_Uretimi_ve_Temizleme.ipynb
│   ├── Makine_Ogrenmesi_ve_Model_Degerlendirme.ipynb
│   ├── Gorev_Karar_Destek_ve_Optimizasyon.ipynb
│   └── README.md
│
├── results/
│   ├── capraz_dogrulama_sonuclari.csv
│   ├── hiperparametre_arama_sonuclari.csv
│   ├── iyilestirme_onerileri.csv
│   ├── karar_destek_senaryo_sonuclari.csv
│   ├── karar_destek_sistemi_ayarlari.json
│   ├── nihai_model_karsilastirmasi.csv
│   ├── ozellik_onemleri.csv
│   ├── test_kumesi_tahminleri.csv
│   ├── veri_temizleme_ozeti.json
│   └── README.md
│
├── requirements.txt
├── .gitignore
└── README.md
```

---

## Notebook Çalıştırma Sırası

Notebook dosyalarının aşağıdaki sırayla çalıştırılması önerilmektedir:

1. [`Veri_Uretimi_ve_Temizleme.ipynb`](notebooks/Veri_Uretimi_ve_Temizleme.ipynb)
2. [`Makine_Ogrenmesi_ve_Model_Degerlendirme.ipynb`](notebooks/Makine_Ogrenmesi_ve_Model_Degerlendirme.ipynb)
3. [`Gorev_Karar_Destek_ve_Optimizasyon.ipynb`](notebooks/Gorev_Karar_Destek_ve_Optimizasyon.ipynb)

Notebook dosyaları Google Colab ortamı için hazırlanmıştır. İlk Notebook, proje dosyalarını Google Drive içerisinde aşağıdaki klasörde oluşturmaktadır:

```text
Droneport_Enerji_Optimizasyonu
```

İkinci ve üçüncü Notebook, önceki aşamalarda bu klasöre kaydedilen dosyaları kullanmaktadır.

---

## Kurulum

Projeyi yerel ortama indirmek için:

```bash
git clone https://github.com/AmirAzimi-15-58/droneport-enerji-optimizasyonu.git
cd droneport-enerji-optimizasyonu
```

Gerekli Python kütüphanelerini yüklemek için:

```bash
pip install -r requirements.txt
```

> Google Colab üzerinde çalıştırıldığında temel kütüphanelerin büyük bölümü hazır olarak bulunmaktadır.

---

## Kullanılan Teknolojiler

- Python
- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- Joblib
- Google Colab
- Google Drive
- GitHub

---

## Sınırlamalar

- Kullanılan veriler tamamen sentetiktir.
- Batarya kapasitesi ve güvenlik eşikleri simülasyon varsayımlarıdır.
- Model gerçek cihaz verileriyle doğrulanmamıştır.
- Uç koşullarda tahmin hatası artabilmektedir.
- Sistem gerçek uçuş güvenliği onayı vermemektedir.
- Gerçek kullanım öncesinde üretici sınırları, gerçek sensör verileri ve uçuş güvenliği kuralları uygulanmalıdır.

---

## Lisans ve Kullanım Notu

Bu proje eğitim ve kavram kanıtlama amacıyla hazırlanmıştır. Gerçek uçuş operasyonlarında doğrudan kullanılmamalıdır.

