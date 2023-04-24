<h1 align="center">METU Datathon Proje Raporu</h1>
<hr>

<p align="center">
  <img src='./Images/METU-Stat-Logo.png' style="float: left;margin:5px 20px 5px 1px" height='170'>
</p>

## Akış Şeması
[Giriş](#giriş)  
[Veri Seti & Veri Ön İşleme](#veriseti--veri-ön-i̇şleme-hk)  
[Makine Öğrenmesi & Tahminleme](#makine-öğrenmesi--tahminleme)  
[Modellerin Değerlendirilmesi](#model-değerlendirmesi)  

<br>
<hr>

## Giriş
Veri setine göre bir churn rate çalışması yapılmak amaçlandı. Buna ilişkin, öncelikle "Churn" edecek kişilerin oluşturuldu.

Müşterinin son belirli bir süre içerisinde hiç alışveriş yapmamış olmasını churn etmiş olarak kabul edip buna göre churn değişkeni oluşturuldu. Müşterilerin alışveriş davranışı incelendi ve aşağıdaki bulgular yapıldı:

* Müşterilerin en son ne zaman alış-veriş yaptığının dağılımı sağa çarpık bir dağılım gösteriyor (Ortalama < Medyan).   
* Çoğu müşteri ortalama olarak sık alışveriş yapıyor. Bu nedenle mod ya da ortalama yerine medyan değeri ele alındı.  
* Ancak eğer medyan değeri referans alırsam verilerim çok fazla dengesiz olacak (ML-DL modellerinin tarafsızlığını yanılatacak). Bu nedenle birazcık daha tolere davranarak 3. çeyrekiği ele alındı. Bu sayede churn eden müşteri sayısı fazla olmamış olur ki diğer türlü churn eden ve etmeyen sayısının dengede olduğu durumda asıl bu dengesiz bir şirketi ortaya çıkarır.    
* Müşterilerin churned kabul edilmesi konusunda biraz daha insaflı davranan bir şirket olduğumuz varsayıldı.

Yukarıda belirtilen adımlar uygulandıktan sonra aşağıdaki çıkarımlar yapıldı:   

> Müşterilerin yaptığı her alış-verişi modele dahil edilmemeli. Müşteri üzerinde genel bir yargıya varılmalı. Müşterimiz herhangi bir şey aldığında model her seferinde onu değerlendirmemeli genel manada ele almalı.  

<hr>

## Veriseti & Veri Ön İşleme Hk.

Veriler "Sales", "Customer" ve "Product" adında üç veri seti şeklindedir. "Sales" veri seti her bir kullanıcının yaptığı alışverişe ilişkin verileri tutar. "Customer" veri seti her bir müşteriye ilişkin bilgileri (Yaş, Cinsiyet, İkamet Yeri) tutar. "Product" veri seti ise her bir ürüne ilişkin kodu ve ürünün ne olduğunun bilgilerini tutar.  

Bu üç veri seti kullanılarak müşterileri genel yargıda değerlendirebilecek bir veri seti oluşturuldu. Bu aşamada;  
* kayıp veriler incelendi.  
* Kategorik değişkenler OHEncode edilip müşterileri genel manada yargılayacak şekilde ayarlandı.  
* Numerik verilerin standartlaştırılması ise makine öğrenmesi kısmında modele göre değişecek şekilde ele alınacaktır.  

En nihayetinde tüm veri setleri kullanılarak gerekli analizler ile müşterilerin hakkında genel bilgi içerecek şekilde düzenlendi. Detaylı bilgilere `./Notebooks/2-preprocessing.ipynb` uzantısındaki notebook'tan ulaşılabilir.  

<hr>

## Makine Öğrenmesi & Tahminleme  

Makine öğrenmesi modeller hususunda veri seti incelendiğinde denenecek modeller aşağıdaki gibi seçildi:  

1. ↘️ Karar Ağaçları (Decision Trees):  
* Yüksek korelasyona sahip değişkenler mevcut ve bu durum lojistik regresyon modeli için sıkıntı yaratabilir. Karar ağaçları bu durumda daha iyi performans gösterebilir.  
* Karar ağaçları aynı zamanda değişkenler arasındaki ilişkileri anlamak için kullanışlıdır.  
* Karar ağaçları kolayca anlaşılabilen bir modeldir. Karar ağaçları modeli, kararlarımızı neden aldığımızı anlamak isteyen müşterilerimize de açıklayabiliriz. 

<br>

2. ↘️ Rastgele Ormanlar (Random Forest):  
* Rastgele ormanlar, yüksek boyutlu ve karmaşık veri setleri için genellikle iyi sonuçlar verir. Veri setimizde 38 değişken var ve rastgele ormanlar bu değişkenler arasındaki ilişkileri iyi bir şekilde ele alabilir.  
* Rastgele ormanlar aynı zamanda açıklayıcılık açısından da faydalıdır. Her bir karar ağacı için özellik önem derecelerini hesaplayarak, hangi özelliklerin modeldeki tahminler için en önemli olduğunu belirleyebiliriz.

<br>

3. ↘️ Gradient Boosting (Gradient Boosted Trees):  
* Gradient Boosting, yüksek boyutlu ve karmaşık veri setleri için de iyi sonuçlar verir.  
* Gradient Boosting aynı zamanda overfitting'i önlemek için de kullanılabilir. Bu durum, modelimizi daha genelleştirilebilir hale getirerek tahminlerin doğruluğunu artırabilir.  

Model testleri yapıldıktan sonra modeller aşağıdaki metrikler ile değerlendirildi:  
<hr>

## Model Değerlendirmesi

$$Accuracy(Doğruluk) = \frac{\text{Doğru sınıflandırılan örnek sayısı}}{\text{Toplam örnek sayısı}}$$

$$Precision(Hassasiyet) = \frac{\text{Gerçek pozitiflerin sayısı}}{\text{Toplam pozitif tahminlerin sayısı}}$$

$$Recall(Duyarlılık) = \frac{\text{Gerçek pozitiflerin sayısı}}{\text{Toplam gerçek pozitiflerin sayısı}}$$

$$F1\text{ }Score = 2 \cdot \frac{Precision \cdot Recall}{Precision + Recall}$$

<br>

En nihayetinde elde edilen sonuçlar ise aşağıdaki tablodaki gibi bulundu:  

<div align="center">

| Model | Accuracy | Precision | Recall | F1 Score | AUC |  
|:-----:|---------:|----------:|-------:|---------:|----:|
|Decision Tree|0.736|0.629|0.656|0.642|0.719|
|Random Forest|0.776|0.718|0.625|0.668|0.744|
|Gradient Boosting Trees|0.798|0.785|0.607|0.685|0.757|

</div>