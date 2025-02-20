
Veriyi Anlamak 

customerID: Müşteri kimliği (benzersiz bir tanımlayıcı).

gender: Müşterinin cinsiyeti (örneğin, 'Male' veya 'Female').

SeniorCitizen: Müşterinin yaşlı olup olmadığı (0 = Hayır, 1 = Evet).

Partner: Müşterinin bir partneri olup olmadığı (örneğin, 'Yes' veya 'No').

Dependents: Müşterinin bağımlı bireyleri olup olmadığı (örneğin, 'Yes' veya 'No').

tenure: Müşterinin hizmeti kullanma süresi (ay cinsinden).

PhoneService: Müşterinin telefon servisi kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

MultipleLines: Müşterinin birden fazla telefon hattı kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

InternetService: Müşterinin internet hizmetine sahip olup olmadığı (örneğin, 'DSL', 'Fiber optic', 'No' gibi seçenekler).

OnlineSecurity: Müşterinin online güvenlik hizmeti kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

OnlineBackup: Müşterinin online yedekleme hizmeti kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

DeviceProtection: Müşterinin cihaz koruma hizmeti kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

TechSupport: Müşterinin teknik destek hizmeti kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

StreamingTV: Müşterinin TV yayınlarını akış hizmeti ile izleyip izlemediği (örneğin, 'Yes' veya 'No').

StreamingMovies: Müşterinin film yayınlarını akış hizmeti ile izleyip izlemediği (örneğin, 'Yes' veya 'No').

Contract: Müşterinin sözleşme tipi (örneğin, 'Month-to-month', 'One year', 'Two year').

PaperlessBilling: Müşterinin kağıtsız faturalama kullanıp kullanmadığı (örneğin, 'Yes' veya 'No').

PaymentMethod: Müşterinin ödeme yöntemi (örneğin, 'Electronic check', 'Mailed check', 'Bank transfer' gibi).

MonthlyCharges: Müşterinin aylık ödeme tutarı (float tipi, para birimi).

TotalCharges: Müşterinin toplam ödeme tutarı (string olarak saklanıyor, fakat sayısal olmalı).

Churn: Müşterinin hizmeti sonlandırıp sonlandırmadığı (örneğin, 'Yes' = sonlandırmış, 'No'




1. Business Understanding (İş Anlayışı)
Telco Customer Churn analizi, bir telekomünikasyon şirketinin müşteri kaybını (churn) tahmin etmeye yöneliktir. Bu bağlamda, müşterilerin aboneliklerini iptal etme olasılıklarını analiz etmek, şirketin müşteri sadakati stratejileri geliştirmesine yardımcı olabilir. Modelin temel amacı, hangi özelliklerin churn ile ilişkili olduğunu anlamak ve şirketin müşteri kaybını önlemeye yönelik kararlar almasını sağlamaktır.

2. Data Understanding (Veri Anlayışı)
Veri seti, 7043 müşteri kaydını ve 21 değişkeni içermektedir. Temel özellikler arasında müşteri kimliği, cinsiyet, yaşlılık durumu, abonelik süresi (tenure), internet servisi türü ve aylık ücret gibi değişkenler bulunmaktadır. Çıkardığınız önemli bulgular şunlardır:

Eksik Veri: TotalCharges değişkeninde 11 eksik veri bulunmuş, fakat bu eksiklik veri kaybına neden olmayacağı için bu veriler kaldırılmış.
Veri Türü Dönüşümleri: TotalCharges, nesne türünde (string) kaydedilmiş, ancak sayısal değer olarak saklanması gerekiyor. Benzer şekilde, SeniorCitizen değişkeni de integer türünde olmalıydı.
Dağılım Özellikleri: Verilerin çoğu normal dağılıma uymamaktadır (örneğin, TotalCharges, MonthlyCharges ve tenure değişkenleri için Shapiro-Wilk testi sonuçları p<0.05).
Ayrıca:

Churn ile ilişkili değişkenler: Chi-Square, Cramér's V ve Pearson Korelasyon gibi testler ile churn ile ilişkili olan önemli değişkenler belirlenmiştir.

3. Data Preparation (Veri Hazırlığı)
Eksik Veri: Eksik veriler TotalCharges'da bulundu ve bu eksik veriler başarıyla kaldırıldı.
Tekrar Edilen Veriler : Sayıları cok az olan bu veriler de başrıyla kaldırıldı
Veri Dönüşümü:
TotalCharges değişkeni float64 türüne dönüştürüldü.
SeniorCitizen değişkeni, 0 ve 1 değerleri için uygun şekilde integer türüne dönüştürüldü.
Öznitelik Seçimi: Gereksiz olan customerID gibi değişkenler veri setinden çıkarıldı.
Normalizasyon ve Skalalandırma: Verilerdeki yayılma (spread) ve dağılım (skewness) yüksek olduğu için, modelin daha iyi performans göstermesi adına uygun ön işleme tekniklerine (örneğin, log dönüşümü) ihtiyaç duyulabilir.

4. Hypothesis Testing (Hipotez Testleri)
Churn ile ilişkili değişkenler için hipotez testleri yapılmıştır. Aşağıda her bir testin sonuçları yer almaktadır:

Chi-Square Testi:
gender: p-değeri: 0.49 → Anlamlı bir ilişki yok.
SeniorCitizen: p-değeri: 2.48e-36 → Anlamlı bir ilişki var.
Partner: p-değeri: 3.97e-36 → Anlamlı bir ilişki var.
Dependents: p-değeri: 2.02e-42 → Anlamlı bir ilişki var.
PhoneService: p-değeri: 0.35 → Anlamlı bir ilişki yok.
MultipleLines: p-değeri: 0.0035 → Anlamlı bir ilişki var.
InternetService: p-değeri: 5.83e-159 → Anlamlı bir ilişki var.
OnlineSecurity: p-değeri: 1.40e-184 → Anlamlı bir ilişki var.
OnlineBackup: p-değeri: 7.78e-131 → Anlamlı bir ilişki var.
DeviceProtection: p-değeri: 1.96e-121 → Anlamlı bir ilişki var.
TechSupport: p-değeri: 7.41e-180 → Anlamlı bir ilişki var.
StreamingTV: p-değeri: 1.32e-81 → Anlamlı bir ilişki var.
StreamingMovies: p-değeri: 5.35e-82 → Anlamlı bir ilişki var.
Contract: p-değeri: 7.33e-257 → Anlamlı bir ilişki var.
PaperlessBilling: p-değeri: 8.23e-58 → Anlamlı bir ilişki var.
PaymentMethod: p-değeri: 1.43e-139 → Anlamlı bir ilişki var.
Sonuç: Çoğu değişkenin Churn ile anlamlı bir ilişkisi olduğu gözlemlenmiştir. Bu, şirketin müşteri kaybını tahmin etmek için bu değişkenlerin önemli olduğunu gösterir.

Cramér's V ve Phi Coefficient:

MultipleLines ve Churn: 0.04 → Çok zayıf ilişki.
InternetService ve Churn: 0.32 → Orta düzeyde ilişki.
Contract ve Churn: 0.41 → Orta düzeyde ilişki.
PaymentMethod ve Churn: 0.30 → Orta düzeyde ilişki.
Sonuç: InternetService, Contract, ve PaymentMethod gibi değişkenler, Churn ile daha belirgin ilişkiler göstermektedir.

Kruskal-Wallis H Testi (Sürekli Değişkenler):

tenure: p-değeri = 0.0 → Anlamlı fark var.
MonthlyCharges: p-değeri = 0.0 → Anlamlı fark var.
TotalCharges: p-değeri = 0.0 → Anlamlı fark var.
Sonuç: tenure, MonthlyCharges, ve TotalCharges değişkenleri ile Churn arasında istatistiksel olarak anlamlı farklar bulunmaktadır. Bu, müşterinin abonelik süresi, aylık ücret ve toplam ücretinin abonelik iptali üzerinde etkili olduğunu gösterir.

5. Modeling (Modelleme)
Modelleme amacı: Bağımsız değişkenlerin hedef değişken olan Churn ile olan ilişkisini ölçmektir. Mutual Information ve regresyon analizleri kullanılarak bağımsız değişkenlerin Churn üzerindeki etkileri incelenmiştir.

Mutual Information:
En güçlü ilişkiler Contract, tenure, OnlineSecurity, TechSupport, ve InternetService gibi değişkenlerle gözlemlenmiştir.
En düşük ilişkiyi gösteren değişkenler ise gender, MultipleLines ve PhoneService olmuştur.
Sonuç: Mutual Information analizine göre, Contract ve tenure değişkenleri Churn tahmini için oldukça önemli faktörlerdir. Bu değişkenler, churn (abone iptali) olasılığını etkileyen başlıca faktörlerdir.

6. Evaluation (Değerlendirme)
Modelin değerlendirilmesi sonucunda, aşağıdaki bulgular elde edilmiştir:

Korelasyonlar:

MonthlyCharges ve TotalCharges arasında orta düzeyde bir korelasyon (0.6511) bulunmaktadır.
Tenure ve TotalCharges arasında güçlü bir korelasyon (0.8259) vardır.
Tenure ve MonthlyCharges arasında zayıf bir korelasyon (0.2469) bulunmaktadır.
Model Başarısı:

Accuracy: Model doğruluğu %78.5 civarındadır.
Confusion Matrix ve Precision-Recall: Model churn tahminlerinde orta seviyede doğru sonuçlar elde etmektedir.
Cross-Validation: 5-fold çapraz doğrulama sonuçları, modelin genel doğruluğunun %80 civarında olduğunu göstermektedir. Bu, modelin genelleme kabiliyetinin güçlü olduğunu gösterir.







Müşteri Segmentasyonu ve CRM Stratejileri - Özet Rapor

1. Tenure (Müşteri Süresi) Analizi
0-3 Ay Arası Müşteriler (Kelebek Müşteri Grubu)
Bu grup, kampanyalar veya indirimler nedeniyle geçici olarak abone olmuş ve churn oranı yüksek olabilir.
Sadık müşterilere dönüşüm sağlamak için deneyim ve ilişki yönetimi geliştirilmelidir.
70+ Ay Arası Müşteriler (Kemik Kitle)
Bu grup, uzun vadeli sadık müşterilerdir.
Özel ödüller, öncelikli hizmet ve kişiselleştirilmiş deneyimler ile korunmalıdır.
3. TotalCharges ve MonthlyCharges
Düşük Ödeme Tutarları (~20 TL)
Genellikle fiyat duyarlı ve kampanya takibi yapan müşteriler.
Bu gruptan sadık müşteri dönüşümü için yükseltme, çapraz satış ve özel teklifler gereklidir.
Yüksek MonthlyCharges ve TotalCharges
Bu grup genellikle yüksek maliyetli hizmetler alıyor ancak memnuniyetsizlik nedeniyle churn oranı yüksek olabilir.
Değer odaklı stratejiler, müşteri memnuniyeti ve hizmet özelleştirmeleri bu gruptaki kullanıcıları elde tutmada kritik rol oynar.
4. Ödeme Yöntemleri
Electronic Check
Elektronik ödeme yöntemine olan ilgi arttıkça, bu ödeme seçeneği üzerinden kampanyalar ve özel teklifler yapılabilir.
5. İnternet Servisi
Fiber Optic
Yüksek fiyatla birlikte gelen sadakat ve memnuniyet sorunları olabilir.
Bu grup için fiyat-performans stratejileri ve müşteri desteği önemlidir.
DSL
Daha stabil ve düşük churn oranına sahip.
Bu grup için düşük maliyetli hizmetler ve sürekli değer sağlama odaklı stratejiler önerilir.
6. Churn (Ayrılma) Analizi
Churn = No (Sadık Müşteriler)
Bu grup uzun vadeli ilişkiler için önemlidir.
Müşterilere sadakat ödülleri, özel teklifler ve kişiselleştirilmiş hizmetler sunulabilir.
Churn = Yes (Kaybedilen Müşteriler)
Yüksek MonthlyCharges’a sahip, memnuniyetsiz müşteriler.
Bu gruptaki kullanıcıları elde tutmak için fiyat-performans oranı iyileştirilmeli ve daha fazla kişiselleştirilmiş hizmet sunulmalıdır.
7. Cinsiyet ve Internet Servisi İlişkisi
Cinsiyet Dağılımı
Erkek ve kadın kullanıcılar arasında eşit bir dağılım gözlemlenmiştir.
Churn oranları açısından anlamlı bir farklılık bulunmamaktadır.
Fiber Optic ve DSL Kullanımı
Fiber optic müşterileri yüksek churn oranına sahipken, DSL kullanıcıları daha düşük churn oranlarına sahip.
Bu gruplara yönelik ayrı stratejiler geliştirilmelidir.
8. Yaş ve Teknoloji Kullanımı
Yaşlı Müşteriler (SeniorCitizen = 1)
Daha düşük internet ve telefon hizmeti kullanımı, özellikle Fiber Optic hizmetine olan düşük talep.
Bu grup için daha basit kurulumlar ve eğitimli destek hizmetleri sunulabilir.
Sonuç ve Öneriler:
Kelebek Müşteriler (0-3 Ay): Yüksek churn oranına sahip bu grup için deneyim ve ilişki yönetimi geliştirilmelidir. Kampanya sonrası sadakat sağlanabilir.
Kemik Kitle (70+ Ay): Bu grup sadık müşterilerdir. Onlara özel ödüller ve hizmetler sunulmalı, uzun vadeli ilişkiler pekiştirilmelidir.
Yüksek Ödeme Müşterileri: Değer odaklı stratejilerle bu gruptaki müşterilerin memnuniyeti artırılabilir.
Churn Stratejisi: Churn oranı yüksek müşteriler için kişiselleştirilmiş teklifler ve müşteri deneyimi iyileştirmeleri önemlidir.
Bu öneriler, CRM stratejileri için temel aksiyonları belirlerken, müşteri memnuniyetini artırma ve sadakati pekiştirme amacını taşımaktadır.
















