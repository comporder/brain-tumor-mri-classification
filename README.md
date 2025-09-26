# Brain Tumor MRI Classification

## Projenin Amacı
Bu projenin amacı, MRI görüntülerinden **beyin tümörlerini otomatik olarak sınıflandırmak** için derin öğrenme tabanlı yöntemler geliştirmektir. Amaç, farklı tümör tiplerini (glioma, meningioma, pituitary) ve tümör bulunmayan durumları ayırt ederek tanısal süreçlerde yapay zekâ desteği sağlamaktır. Böylece doktorların iş yükünü azaltmak ve erken teşhisi kolaylaştırmak hedeflenmiştir.

## Veri Seti
- Kullanılan veri seti: **Brain Tumor MRI Dataset**  
- Sınıflar: **Glioma, Meningioma, Pituitary, No Tumor**  
- Toplam görüntü sayısı: ~7000  
- Görseller `train`, `validation` ve `test` olarak ayrılmıştır.  
- Tüm görüntüler **224x224 piksel** boyutuna getirilmiş ve **1/255 normalizasyon** uygulanmıştır.  
- Modelin genelleme kabiliyetini artırmak için **data augmentation** (döndürme, yakınlaştırma, yatay/dikey çevirme) uygulanmıştır.

## Kullanılan Yöntemler
Projede üç farklı yaklaşım denenmiş ve karşılaştırılmıştır:

### 1. Custom CNN
- **Amaç:** Sıfırdan oluşturulan bir CNN ile temel bir karşılaştırma modeli elde etmek.  
- **Yapı:** 3 konvolüsyon katmanı, max-pooling, dropout ve fully connected katmanlar.  
- **Neden seçildi?**  
  - Modelin karmaşıklığını doğrudan kontrol edebilmek için.  
  - Transfer learning öncesinde, verinin kendi başına ne kadar ayırt edilebilir olduğunu görmek için baseline (temel) performans oluşturmak.

### 2. EfficientNetB0 (Transfer Learning)
- **Amaç:** Daha derin ve optimize edilmiş bir mimari ile daha yüksek doğruluk elde etmek.  
- **Yapı:** Imagenet ile önceden eğitilmiş EfficientNetB0 tabanı, üzerine eklenen dense katmanlarla yeniden eğitildi.  
- **Neden seçildi?**  
  - EfficientNet mimarisi, model boyutu ve doğruluk arasında **dengeli bir ölçekleme** sunar.  
  - Daha küçük parametre sayısı ile yüksek performans elde etmesi, özellikle sınırlı veri setlerinde genelleme açısından avantaj sağlar.  

### 3. MobileNetV2 (Transfer Learning)
- **Amaç:** Daha hafif bir model kullanarak hızlı eğitim ve test sağlamak.  
- **Yapı:** Imagenet tabanlı MobileNetV2 mimarisi, sınıflandırma katmanı yeniden eğitildi.  
- **Neden seçildi?**  
  - Mobil cihazlar veya düşük donanım kapasiteli ortamlarda kullanılabilecek kadar **hafif ve hızlı** bir model olması.  
  - Transfer learning sayesinde daha kısa sürede yüksek doğruluk değerlerine ulaşabilmesi.  

**Ortak teknikler:**
- **Transfer Learning:** Daha önce geniş ölçekli veri setlerinde eğitilmiş modellerin ağırlıkları kullanılarak, küçük bir tıbbi veri setinde genelleme başarısı artırıldı.  
- **EarlyStopping:** Overfitting’i engellemek için doğrulama kaybı takip edilerek eğitim doğru noktada sonlandırıldı.  
- **Metrikler:** Accuracy yanında **precision, recall, f1-score** ve **confusion matrix** kullanılarak sınıflar bazında detaylı performans ölçümü yapıldı.  

## Sonuçlar
Modellerin test sonuçları aşağıda özetlenmiştir:

| Model         | Test Accuracy | Güçlü Sınıflar        | Zayıf Sınıf   | Açıklama |
|---------------|---------------|-----------------------|---------------|----------|
| Custom CNN    | ~%86          | No Tumor, Pituitary   | Meningioma    | Basit mimari için yüksek başarı, baseline performans sağladı |
| EfficientNetB0| ~%85          | No Tumor, Pituitary   | Meningioma    | Daha dengeli sonuçlar, fakat validation accuracy dalgalanmaları görüldü |
| MobileNetV2   | ~%85          | No Tumor, Pituitary   | Meningioma    | Daha hızlı eğitime rağmen benzer doğruluk, deployment için uygun |

**Öne çıkan gözlemler:**
- **No Tumor** ve **Pituitary** sınıflarında tüm modeller yüksek başarı sağlamıştır.  
- **Meningioma sınıfında recall düşük** kalmıştır, bu sınıf en çok karıştırılan tümör tipidir.  
- Genel doğruluk tüm modellerde **%85–86 aralığında** olmuştur.  

## Kaggle Notebook
Projeye ait tüm kod ve çıktıların yer aldığı Kaggle notebook bağlantısı:  
[Brain Tumor MRI Classification - Kaggle](https://www.kaggle.com/code/emirzalp/brain-tumor-mri-classification)
