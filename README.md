# VERİ MADENCİLİĞİ DERSİ DÖNEM ÖDEVİ
**Konu:** Telco Müşteri Kaybı (Churn) Analizi ve Tahminleme  
**Araç:** Orange Data Mining Tool  

Bu repo/rapor, bir telekomünikasyon şirketine ait müşteri verilerinin işlenmesi, görselleştirilmesi ve makine öğrenmesi algoritmaları kullanılarak müşterilerin şirketi terk etme (churn) durumlarının tahminlenmesi aşamalarını içermektedir.

---

## 📌 KISIM 1: Veriyi Tanıma ve Hazırlama (Preprocessing & EDA)

### İş Akışı (Orange Workflow)
Verinin yüklenmesi, eksik değerlerin doldurulması (Impute), gereksiz kolonların çıkarılması ve makine öğrenmesi modellerine aktarılması sürecini gösteren iş akışımız aşağıdadır:

<img width="1368" height="702" alt="Ekran görüntüsü 2026-05-13 201407" src="https://github.com/user-attachments/assets/e989af0e-3953-4f06-9df6-3de0605cb036" />

### Temizlenmiş Veri (Data Table)
Ham veri üzerinde yapılan ön işleme adımlarında; `TotalCharges` sütununda boş bırakılan (NaN) değerler sütun ortalaması ile doldurulmuş ve analizde hedef değişkene (Churn) etkisi olmayan/anlamsız `customerID` kolonu veri setinden çıkarılmıştır. Temizlenmiş verinin son hali aşağıdaki gibidir:

<img width="1247" height="1145" alt="Ekran görüntüsü 2026-05-13 201512" src="https://github.com/user-attachments/assets/742dcb51-d601-4d20-8f1d-1a37c4d7f0c6" />
<img width="1901" height="1137" alt="Ekran görüntüsü 2026-05-13 201450" src="https://github.com/user-attachments/assets/0ecdfa1d-b70e-41c9-aa6a-15f60f19749e" />

### Görsel Analiz (EDA) Yorumu
Distributions ve Scatter Plot widget'ları kullanılarak yapılan veri keşfi (EDA) sonucunda elde edilen teknik bulgu:

> *"Distributions grafiğine göre, Aydan Aya (Month-to-month) sözleşme türünü seçen müşterilerin platformu terk etme eğilimi çok daha yüksektir. 1 veya 2 yıllık taahhütlü uzun süreli sözleşmelerde ise müşteri kaybının (churn) belirgin şekilde azaldığı görülmektedir."*

---

## 📌 KISIM 2: Tahminleme ve Model Değerlendirme (Modeling & Evaluation)

Hazırlanan temiz veri, "Data Sampler" (Eğitim/Test Ayırımı) aracı ile Stratify (Katmanlı) yöntemi kullanılarak **%80 Eğitim (Training)** ve **%20 Test (Testing)** olarak ikiye ayrılmıştır. Veri; Lojistik Regresyon, Karar Ağacı (Decision Tree) ve Rastgele Orman (Random Forest) algoritmalarıyla eğitilmiştir.

### Model Başarı Karşılaştırması
Modellerin Test verisi üzerindeki performans (AUC ve Sınıflandırma Doğruluğu - CA) sonuçları aşağıdadır:

<img width="476" height="316" alt="Ekran görüntüsü 2026-05-13 202625" src="https://github.com/user-attachments/assets/ce459b93-2716-42f1-8bd9-76e6b876c284" />

*(Not: En yüksek AUC ve CA değerine sahip olan model Logistic Regression olarak tespit edilmiştir.)*

### Hata Analizi (False Positive)
En iyi performansı gösteren Lojistik Regresyon modelinin "Hata Matrisi" (Confusion Matrix) incelendiğinde:

> *Modelin yanlışlıkla 'Ayrılacak' (Churn=Yes) öngörüsünde bulunduğu ancak gerçekte ayrılmayıp 'Kalan' (Churn=No) müşteri sayısı **375** olarak gözlemlenmiştir. Bu durum modelin Tip 1 Hata (False Positive) oranıdır.*

### Riskli Müşteri Profili
Karar Ağacı (Tree Viewer) görsel çıktılarına göre dallanmalar incelendiğinde elde edilen profil:

> **Şirket için en riskli müşteri profili:** Sözleşmesi aylık olarak yenilenen (Month-to-month), internet altyapısı olarak fiber optik kullanan ve şirketteki abonelik süresi (tenure) henüz çok kısa olan (yeni) müşterilerdir.
