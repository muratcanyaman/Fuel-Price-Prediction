# Küresel Yakıt Fiyatları Tahmini (2020-2026)

Bu proje, "veri sızıntısı" (data leakage) tuzağına düşmeden, tamamen makro-ekonomik ve politik değişkenler üzerinden küresel benzin fiyatlarını tahmin etmeyi amaçlayan uçtan uca bir Veri Bilimi çalışmasıdır.

**Veri Seti Kaynağı:** Bu projede analiz edilen veri seti (Global Fuel Prices 2020-2026), Kaggle açık veri platformundan [bu bağlantı üzerinden](https://www.kaggle.com/datasets/belbino/global-fuel-prices-20202026/data) temin edilmiştir.

## Proje Özeti
Pek çok model, benzin fiyatını tahmin etmek için modeline diğer yakıtların (örneğin Dizel veya LPG) fiyatını vererek %100'e yakın başarı elde ettiğini düşünür. Ancak tüm bu yakıtlar aynı hammaddeden üretildikleri ve yine o ülkenin aynı vergi dilimlerine tabi oldukları için, birini kullanarak diğerini basit bir "oran-orantı" problemi gibi tahmin etmek aslında yapay ve yapısal olarak hileli bir model (Veri Sızıntısı - Data Leakage) anlamına gelir.

Bu projede, modelden "kolay yoldan kopya çekmesini" engellemek için diğer yakıt türlerini **kasıtlı olarak sildik**. Yalnızca aşağıdaki gerçek makro-ekonomik öznitelikleri kullanarak pompa fiyatlarını tahmin eden donanımlı bir **XGBoost Regresyon** modeli inşa ettik:
- **Gelir Seviyesi (Income Level)**
- **Devlet Desteği / Sübvansiyon (Subsidy Level)**
- **Brent Ham Petrol Fiyatı USD** (Küresel pazar değeri)
- **Vergi Yüzdesi (Tax Percentage)**

## Keşifçi Veri Analizi (EDA)
Modele geçmeden önce verinin karakteristiğini görsel olarak ortaya koymak için detaylı analizler (EDA) yaptık:
*   **Fiyat Dağılım Grafikleri:** Küresel benzin fiyatlarının dünyadaki genel frekans dağılımını (histogramlar) gözlemledik.
*   **Keman Grafikleri (Violin Plots):** Klasik bir doğrusal algoritmadan ziyade verilerdeki kırılımların asıl sebebi olan devlet destekleri veya gelir düzeyi gruplarının pompa fiyatarını nasıl ayırdığını görselleştirdik.
*   **Korelasyon Haritası (Heatmap):** Ham petrol, vergi oranları ve diğer yakıtların matematiksel ilişkilerini çıkarttık.

## Model Performansı ve Temel Çıkarımlar
Karar Ağacı (Decision Tree) tabanlı bir algoritma olan **XGBoost'u** özellikle seçtik. Çünkü gelir seviyesi veya belirli vergi kademeleri gibi mantıksal ayrımların sebep olduğu doğrusal olmayan (non-linear) karar ayrımlarını yakalama konusunda XGBoost rakipsizdir.

*   **Tutarlılık ve Başarı:** En zorlu şartlarla sınanmasına rağmen model **~%99.8'lik mükemmel bir R² Skoru** ile tahmini tutturmuş ve hata payı (MAE) ortalama 0.04 cent gibi çok küçük sınırların altında kalmıştır.
*   **Veriden Elde Edilen İçgörüler:** Değişken önemi (Feature Importance) analizi, nihai benzin fiyatlarını belirleyen mutlak sürücülerin **Devlet Destekleri (Subsidy Level)** ve **Ülkelerin Gelir Seviyesi** (Income Level) olduğunu gözler önüne sermiştir. Sürpriz bir şekilde, varil başı küresel ham petrol pazar maliyetinin pompa etiketindeki pazar rolü, yerel hükümetlerin karar düzenlemelerinin çok gerisinde kalmıştır.

## Başlarken
1. Bu depo adresini (repository) ortamınıza kopyalayın (clone).
2. Ortamınızda `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, ve `xgboost` paketlerinin kurulu olduğundan emin olun.
3. Çalışmadaki görsel çıktıları ve modelin başarımını kendiniz görmek için `fuel_price_prediction.ipynb` notebook dosyasını baştan uca tek seferde çalıştırın (Run All).