<h1 align="center">METU Datathon Raporlaması</h1>
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