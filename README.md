Bu projede, makine öğrenmesi ve görüntü işleme tekniklerini kullanarak yüz maskesi var/yok ayrımı yapan bir sınıflandırma modeli geliştirdik. Aşağıda proje adımları ve kullandığımız teknolojiler özetlenmiştir. Dataset: https://www.kaggle.com/datasets/omkargurav/face-mask-dataset
Proje Adımları
1.	Veri Bölme: Kaggle’dan indirilen "with_mask" ve "without_mask" klasörlerindeki görüntüler, %80 eğitim ve %20 doğrulama olarak ayrıldı.
2.	Görsel İyileştirme (CLAHE): OpenCV kullanarak her görüntüye Contrast Limited Adaptive Histogram Equalization (CLAHE) uyguladık. Böylece düşük kontrastlı görüntülerde maske ve yüz hatları daha net hale geldi.
3.	Veri Artırma (Augmentation): Keras ImageDataGenerator ile döndürme, zoom, kaydırma, yatay çevirme gibi teknikleri eğitim setine uyguladık; doğrulama seti yalnızca normalize edildi.
4.	Model Mimarisi: Transfer learning yaklaşımıyla ImageNet ön eğitimli ResNet50’yi temel aldık ve katmanlarını dondurarak (freeze) aşağıdaki özelleştirilmiş sınıflandırıcıyı ekledik:
o	GlobalAveragePooling2D
o	Dense(256, ReLU) + Dropout(0.5)
o	Dense(128, ReLU) + Dropout(0.3)
o	Dense(64, ReLU) + Dropout(0.3)
o	Dense(1, sigmoid)
5.	Model Eğitimi: Adam optimizer (lr=1e-4) ve binary_crossentropy loss kullanarak 20 epoch boyunca eğittik. EarlyStopping ve ReduceLROnPlateau callback’leriyle öğrenme oranı ve durdurma kontrolü sağladık.
6.	Değerlendirme: Eğitim ve doğrulama doğruluk ve loss eğrilerini görselleştirdik, shuffle=False ile confusion matrix ve classification report ürettik. Sonuç olarak %80 civarında doğruluk elde ettik.
Kullanılan Teknolojiler
•	Python 3.8+
•	TensorFlow & Keras: Model tanımlama, eğitim ve değerlendirme.
•	OpenCV: Görüntü okuma, CLAHE ile kontrast iyileştirme.
•	scikit-learn: Veri bölme (train_test_split), metrik hesaplama.
•	Matplotlib & Seaborn: Eğitim eğrilerinin ve confusion matrix’in görselleştirilmesi.
•	NumPy & OS: Dosya işlemleri ve veri manipülasyonu.

