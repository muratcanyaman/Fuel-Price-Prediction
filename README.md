# ⛽ Küresel Yakıt Fiyatları Tahmini (2020-2026)

Bu belge, dünyanın çeşitli pazar bölgelerindeki makro-ekonomik dinamikleri kullanarak pompa benzin fiyatlarını tahmin etmeyi amaçlayan uçtan uca bir makine öğrenmesi analizidir. Projemiz "Global Fuel Prices 2020-2026" Kaggle veri seti kullanılarak hazırlanmıştır.

## 🎯 "Veri Sızıntısı" (Data Leakage) Problemine Çözüm
Bildiğiniz üzere yakıt tahmini modellerinde sıklıkla yapılan büyük bir hata, aynı türden metadan (petrol türevleri) gelen "Dizel" veya "LPG" fiyatlarını hedef "Benzin" (Petrol) fiyatını tahmin etmede kullanmaktır. Ancak bu yaklaşım hileli bir başarı olan ve modeli ezbere yönlendiren "Veri Sızıntısına" (Data Leakage) sebep olur.  
Bu projede, modelimizi bu "kısa yoldan" uzak tutmak için, **dizel ve lpg sütunları notebook içerisinde bilinçli olarak düşürülmüştür.**

## 📊 Kullanılan Gerçek Değişkenler
Benzin fiyatlarını tamamen yapısal ve ekonomik dinamiklerle tespit etmek üzere sadece aşağıdaki sütunlar sisteme beslenmiştir:
- `brent_crude_usd`: Küresel Brent Ham Petrol Fiyatı (USD)
- `tax_percentage`: Fiyatlara Yansıyan Yüzdelik Vergi Oranı
- `subsidy_level`: Devlet Desteği (Sübvansiyon Seviyesi)
- `income_level`: O Ülkenin Ekonomik Gelir Grubu 

*(Not: Hedefi direk bölen `country` ve `region` özellikleriyle birlikte, `diesel` ve `lpg` verileri test sağlığı adına modelden dışlanmıştır.)*

## 🔍 Notebook Adımları ve Model Kurulumu
1. **Keşifçi Veri Analizi (EDA):** Notebook içerisinde "Histogramlar" ile yoğunluk analizi, "Keman Grafikleri (Violin Plots)" ile kategorik verilerin (gelir düzeyi vb.) benzin fiyatına etkileri görselleştirilmiş ve değişkenler arası "Korelasyon Haritası" (Heatmap) çıkarılmıştır.
2. **Kategorik Veri İşleme:** Gelir seviyesi (Low, Medium, High vb.) algoritmaya uygun şekilde sayısallaştırıldı (`OrdinalEncoder`).
3. **Standartlaştırma (Scaling):** Geniş rakam aralıklarına sahip ham petrol verisi gibi metrikler modelin sağlığı adına `StandardScaler` ile aynı düzleme çekildi.
4. **Modelleme Mimari:** Tüm doğrusal olmayan (non-linear) karmaşıklığı çok başarılı bir şekilde yakalayabilmesi sebebiyle güçlü bir gradient boosting aracı olan **XGBoost Regressor** (Hiperparametreler: `n_estimators=200`, `learning_rate=0.05`, `max_depth=6`) ile çalışıldı.

## 🚀 Model Performansı ve Temel Çıkarımlar
- **Hata ve İstatistik (Metrikler):** Kurulan XGBoost modeli, zorlu koşullarına rağmen **~%99.8 R² Skoru** ve sadece ortalama **0.04 cent (MAE)** sapma hatasıyla pürüzsüz bir tahmin gerçekleştirmiştir.
- **Dışa Vurum (Feature Importance):** Yapılan özellik çıkarımı (etki yüzdesi) analizine göre; ülke içi pompa fiyatlarını derinden değiştiren kuvvetlerin "küresel petrol varili maliyetlerinden ziyade", temelinde **yerel vergi politikaları** ve devletin sunduğu **sübvansiyon teşvikleri** olduğu kanıtlanmıştır.

## 🛠️ Nasıl Çalıştırılır?
1. Repo dosyalarını `git clone` ile ortamınıza indirin.
2. İlgili veri bilimi kütüphanelerini kurmalısınız (`pandas, numpy, matplotlib, seaborn, scikit-learn, xgboost`).
3. Her şeyi görmek ve test etmek için Jupyter arayüzünde `fuel_price_prediction.ipynb` notebook dosyasını açarak tüm satırları sırasıyla *(Run All)* çalıştırın.