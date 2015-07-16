# 1. Temel Kavramlar

## 1.1 İsimlendirme
Diğer bütün yazılım geliştirme sistemleri gibi, arayüz geliştirme için de isimlendirme kritik bir öneme sahiptir. Hem tanımların tekrar kullanılabilirliğini arttırabilme hem de geliştirmeyi kolaylaştırma ihtimalleri göz önüne alındığında üzerinde durulması gereken bir konu olarak öne çıkmaktadır.

İsimlendirme, çoğunlukla zaman alan ve bazen angarya gibi görülen bir iş olmakla beraber, iyi kurgulandığında, üzerinde harcanan her dakika fazlasıyla geri kazanılacaktır. Bu kurguyu yaparken dikkat edilmesi gereken bir kaç nokta ön plana çıkmaktadır. Bunlara göz atacak olursak;

### 1.1.1 İsimler Tasarımdan Bağımsız Olmalı
Çoğu arayüz mimarisinin amacı, HTML yapısını bir defa kurguladığında, çok büyük bir altyapı değişikliği olmadığı sürece farklı tasarım revizelerinde aynı yapıyı kullanabilmeye devam etmektir. Bunun en önemli sebeplerinden biri, HTML ve birçok durumda HTML çıktısını üreten backend kodlarının revizesinin çok masraflı olmasıdır. Bazı durumlarda, CSS içerisinden bir tanımı değiştirerek yapabildiğiniz güncellemeler, arka tarafta onlarca kod dosyası içerisinde yüzlerce tanımı etkileyebilir. 

Web tabanlı sistemleri yaşayan, sürekli büyüyen bir organizma gibi düşündüğümüzde, görünümlerinin, tasarımlarının sürekli değişmesi, yenilenmesi, revize edilmesi işten bile değildir. Örneğin, ilk başlarda yeşil renk olarak tasarlanmış ve uygulanmış bir aksiyon butonunun rengi daha sonrasında mavi olarak değiştirildiğini varsayalım. Kod kurgusu ilk tasarıma göre `.green-button` şeklinde yapılmış ve bütün sistemdeki ana aksiyon butonlarına `green-button` classı eklenmiş olsun. Tasarım revizesinden sonra bu class tanımlanmış bütün öğeler mavi gözükmeye başlıyor. İsminde _yeşil_ geçen bir class tanımını _mavi_ renk vermek için kullanmaya başlamış oluyoruz. Bir süre sonra bütün buton stillerinin değiştiğini düşündüğünüzde vardığınız nokta; yeşil aslında mavi, kırmızı aslında gri, pembe aslında kırmızı, mavi aslında sarı renk veren bir anlam karmaşası olacaktır. 

İsimlerin tasarımdan bağımsız olması, size yeni tasarım revizelerinde daha rahat hareket etmenizi ve kodlarınızın karışmadan daha rahat kontrol edilebilmesine imkan sağlayacaktır. Bunun için yapılabilecek en iyi kurgulardan biri, isimlendirmelerinizi öğelerin __nasıl göründüğü__ yerine __nasıl çalıştığına__ göre yapmaktır. Yine buton tanımlarından yola çıkacak olursak; formları işlemi sonlandıran (submit) buton tasarım ne olursa olsun hep işlemi sonlandırma görevini yerine getirecektir. Dolayısıyla, bu tarz buton için `green-button` yerine `primary-button` ya da daha geniş Kullanım amacı verebilecek `primary-action` gibi isim verilebilir. Tanım setini genişleterek, `secondary-action`, `cancel-action`, `return-action` gibi geniş bir buton tanım setine sahip olunabilir. Ayrıca bu şekilde hazırlanmış bir set, herhangibir anlam kayması olmadan, `button`, `a`, `input[type=submit]` gibi buton işlevi görebilecek farklı elemanlarda da kolayca kullanılabilir. 

### 1.1.2 İsimler Herkes Tarafından Anlaşılabilir Olmalı
Tasarımdan bağımsızlık kadar, isimlerin anlaşılabilir ve hatta tahmin edilebilir olması geliştirmelerin kolay yapılması ve birden fazla geliştiricinin çalıştığı projelerde kod tekrarlarının önüne geçilmesi açısından önemlidir. Isimlendirme her zaman subjektif bir konu olmuştur. Bir çok geliştirici, kendi işini kolaylaştıracağını düşündüğünü tanımları sisteme eklerken kendisinin rahat anlayacağı bir ismi verir. Ama bu isim çoğunlukla başkası Tarafından anlamsız bulunabilir. Örneğin CSS'in projelerde yoğun kullanılmaya başlandığı zamanlarda bazı geliştiriciler `m10`, `mb5`, `ml20` gibi classları veriyorlardı. İlk bakıldığında bu class isimleri şuan hiçbir anlam ifade etmiyor olabilir. Tanımların detayları ise şu şekilde;

```css
.m10 { 
  margin: 10px;
}

.mb5 {
  margin-bottom: 5px;
}

.ml20 {
  margin-left: 20px;
}
```

Yani aslında ilk başta hiç bir anlam ifade etmeyen, ama elemanlar arasına _margin_ tanımı vermek için hazırlanmış classlardır. Bunlar Bir dokümantasyon olmadığı durumlarda geliştirmelere devam edecek bir sonraki geliştirici için oldukça sıkıntıya sebep olabilecek bir sonuç doğuracaktır. 

Başka bir hata da, bir sete anlam ifade etmeyecek şekilde isimler vermektir. `buttonStyle1`, `buttonStyle2`, `buttonStyle3` gibi hem işlevsellik hem de neye benzedikleri açısından hiçbir anlam ifade etmeyen isimlendirmelerdir. Bu grup da, _margin_ tanımları gibi koda ilk defa bakan birisine anlam ifade etmeyecektir. 

Bu tarz sadece kişiye özel anlam ifade edecek isimlerin önüne yine yaptığı işe göre isimlendirme yöntemini uygulayarak geçilebilir. `m10` class ismi yerine, elemanlar arasında tasarımsal boşluğun (gutter) `10px` olarak yapan class ismini `separate-by-gutter` gibi bir isimle daha anlamlı olarak hem de yine tasarımdan bağımsız hale getirerek yapılabilir. Bu sayede, tasarım değiştiğinde, yeni _gutter_ tanımınız `20px` olsa bile, HTML tarafında değişiklik yapma ihtiyacı olmadan CSS üzerinden revize yapma imkanı olacaktır. Ayrıca kodu ilk defa okuyan bir kişi de `separate-by-gutter` tanımını görünce, elemanın diğer elemanlar ile arasına _gutter_ kadar mesafeye sahip olacağını kolayca anlayabilecektir. 
