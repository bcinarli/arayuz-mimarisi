# İsimlendirme
Diğer bütün yazılım geliştirme sistemleri gibi, arayüz geliştirme için de isimlendirme kritik bir öneme sahiptir. Hem tanımların tekrar kullanılabilirliğini arttırabilme hem de geliştirmeyi kolaylaştırma ihtimalleri göz önüne alındığında üzerinde durulması gereken bir konu olarak öne çıkmaktadır.

> There are only two hard things in Computer Science: cache invalidation and naming things. <sup>1</sup>
>
> -- Phil Karlton

İsimlendirme, çoğunlukla zaman alan ve bazen angarya gibi görülen bir iş olmakla beraber, iyi kurgulandığında, üzerinde harcanan her dakika fazlasıyla geri kazanılacaktır. Bu kurguyu yaparken dikkat edilmesi gereken bir kaç nokta ön plana çıkmaktadır. Bunlara göz atacak olursak;

## 1.1 İsimler Tasarımdan Bağımsız Olmalı
Çoğu arayüz mimarisinin amacı, HTML yapısını bir defa kurguladığında, çok büyük bir altyapı değişikliği olmadığı sürece farklı tasarım revizelerinde aynı yapıyı kullanabilmeye devam etmektir. Bunun en önemli sebeplerinden biri, HTML ve birçok durumda HTML çıktısını üreten backend kodlarının revizesinin çok masraflı olmasıdır. Bazı durumlarda, CSS içerisinden bir seçici tanımını değiştirerek yapabildiğiniz güncellemeler, arka tarafta onlarca kod dosyası içerisinde yüzlerce seçici tanımını etkileyebilir. 

Web tabanlı sistemleri yaşayan, sürekli büyüyen bir organizma gibi düşündüğümüzde, görünümlerinin, tasarımlarının sürekli değişmesi, yenilenmesi, revize edilmesi işten bile değildir. Örneğin, ilk başlarda yeşil renk olarak tasarlanmış ve uygulanmış bir aksiyon butonunun rengi daha sonrasında mavi olarak değiştirildiğini varsayalım. Kod kurgusu ilk tasarıma göre `.green-button` şeklinde yapılmış ve bütün sistemdeki ana aksiyon butonlarına `green-button` classı eklenmiş olsun. Tasarım revizesinden sonra bu _class_ eklenmiş bütün öğeler mavi gözükmeye başlıyor. İsminde _yeşil_ geçen bir _class_ tanımını _mavi_ renk vermek için kullanmaya başlamış oluyoruz. Bir süre sonra bütün buton stillerinin değiştiğini düşündüğünüzde vardığınız nokta; yeşil aslında mavi, kırmızı aslında gri, pembe aslında kırmızı, mavi aslında sarı renk veren bir anlam karmaşası olacaktır. 

İlk tasarım geldiğinde yapılan seçici tanımları
``` css
.green-button {
	background-color: green;
}

.blue-button {
	background-color: blue;
}

.red-button {
	background-color: red;
}
```

Tasarımlarda renk revizesi olduktan sonraki seçici tanımları
``` css
.green-button {
	background-color: blue;
}

.blue-button {
	background-color: yellow;
}

.red-button {
	background-color: gray;
}
```

Seçici isimlerinin tasarımdan bağımsız olması, size yeni tasarım revizelerinde daha rahat hareket etmenizi ve kodlarınızın karışmadan daha rahat kontrol edilebilmesine imkan sağlayacaktır. Bunun için yapılabilecek en iyi kurgulardan biri, isimlendirmelerinizi öğelerin __nasıl göründüğü__ yerine __nasıl çalıştığına__ göre yapmaktır. Yine buton seçici tanımlarından yola çıkacak olursak; formları işlemi sonlandıran (submit) buton tasarım ne olursa olsun hep işlemi sonlandırma görevini yerine getirecektir. Dolayısıyla, bu tarz buton için `green-button` yerine `primary-button` ya da daha geniş kullanım amacı verebilecek `primary-action` gibi isim verilebilir. Seçici tanım setini genişleterek, `secondary-action`, `cancel-action`, `return-action` gibi geniş bir buton seçici tanım setine sahip olunabilir. Ayrıca bu şekilde hazırlanmış bir set, herhangibir anlam kayması olmadan, `button`, `a`, `input[type=submit]` gibi buton işlevi görebilecek farklı elemanlarda da kolayca kullanılabilir. 

> When people tell you something's wrong or doesn't work for them, they are almost always right. <sup>2</sup>
>
> -- Neil Gaiman

## 1.2 İsimler Herkes Tarafından Anlaşılabilir Olmalı
Tasarımdan bağımsızlık kadar, seçici isimlerinin anlaşılabilir ve hatta tahmin edilebilir olması geliştirmelerin kolay yapılması ve birden fazla geliştiricinin çalıştığı projelerde kod tekrarlarının önüne geçilmesi açısından önemlidir. İsimlendirme her zaman subjektif bir konu olmuştur. Bir çok geliştirici, kendi işini kolaylaştıracağını düşündüğünü _class_ tanımlarını sisteme eklerken kendisinin rahat anlayacağı bir ismi verir. Ama bu isim çoğunlukla başkası tarafından anlamsız bulunabilir. Örneğin CSS'in projelerde yoğun kullanılmaya başlandığı zamanlarda bazı geliştiriciler `m10`, `mb5`, `ml20` gibi *class*ları veriyorlardı. İlk bakıldığında bu class isimleri şuan hiçbir anlam ifade etmiyor olabilir. *Class*ların detayları ise şu şekilde;

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

Yani aslında ilk başta hiç bir anlam ifade etmeyen, ama elemanlar arasına _margin_ tanımı vermek için hazırlanmış *class*lardır. Bunlar Bir dokümantasyon olmadığı durumlarda geliştirmelere devam edecek bir sonraki geliştirici için oldukça sıkıntıya sebep olabilecek bir sonuç doğuracaktır. 

Başka bir hata da, bir sete anlam ifade etmeyecek şekilde isimler vermektir. `buttonStyle1`, `buttonStyle2`, `buttonStyle3` gibi hem işlevsellik hem de neye benzedikleri açısından hiçbir anlam ifade etmeyen isimlendirmelerdir. Bu grup da, _margin_ tanımları gibi koda ilk defa bakan birisine anlam ifade etmeyecektir. 

Bu tarz sadece kişiye özel anlam ifade edecek isimlerin önüne yine yaptığı işe göre isimlendirme yöntemini uygulayarak geçilebilir. `m10` class ismi yerine, elemanlar arasında tasarımsal boşluğun (gutter) `10px` olarak yapan class ismini `separate-by-gutter` gibi bir isimle daha anlamlı olarak hem de yine tasarımdan bağımsız hale getirerek yapılabilir. Bu sayede, tasarım değiştiğinde, yeni _gutter_ tanımınız `20px` olsa bile, HTML tarafında değişiklik yapma ihtiyacı olmadan CSS üzerinden revize yapma imkanı olacaktır. Ayrıca kodu ilk defa okuyan bir kişi de `separate-by-gutter` tanımını görünce, elemanın diğer elemanlar ile arasına _gutter_ kadar mesafeye sahip olacağını kolayca anlayabilecektir. 

> When you think of a concrete object, you think wordlessly, and then, if you want to describe the thing you have been visualising you probably hunt about until you find the exact words that seem to fit it. <sup>3</sup>
>
> -- George Orwel

## 1.3 İçgüdüsel ve Tahmin Edilebilir Olmalı
Özellikle ekip olarak çalışılan projelerde, isimlendirmenin kolay takip edilebilir olması ve akla uygun olması çok önemlidir. Tahmin edilebilir isimler kullanımı kolaylaştırdığı gibi, seçici tanımının var olup olmadığının bilinmemesi yüzünden kod tekrarınını azaltacaktır. _Widget_ olarak kullanılan kısımlar için `.widget` isimli bir _component_ seçici tanımının yapılmış olması, başlık alanları için `.title` isimli seçicilerin oluşturulması ya da anasayfa _spot_ alanı olarak kullanılacak içeriğe `.spot` şeklinde isim verilmesi tamamen çalışma içerisinde tahmin edilebilir bir isimlendirme standardına sahip olunacaktır.

Bu tarz isimlendirmeler genelde içgüdüsel olarak yapılmaktadır ve elemanın ya da *component*'e görünümünden çok yaptığı işe ve/veya içeriğine göre şekillenmektedir. Nasıl _menu_ olarak kullanılan bir elemana `list` şeklinde bir class tanımı yapmak anlamsız geliyorsa, içgüdüsel olmayan isimlendirmeler de tanımları kullananlar için anlamsız/ters gelecektir.

Kolay bulunabilen, gündelik kodlama dili içerisinde sık kullanılan ve metaforlardan uzak olan isimleri tercih etmek seçicileri tanımlamayı kolaylaştıracak ve aylar sonra yeni bir ekleme yapma ihtiyacı hissetiğinizde "buna neden böyle demişiz" sorularından sizi kurtaracaktır. Ekip olarak çalışılan, büyük projelerde ise, hem kod bakımını hem de geliştirmeyi hızlandıracak bir temel atılmış olacaktır.

---

1. <span name="fn-1" id="fn-1">Programcılıkta iki zor şey vardır: Ön belleği silme ve tanımları isimlendirme.</span>
2. Birileri ne zaman bir şeyin yanlış olduğunu veya kendileri için anlamlı olmadığını söylüyorsa, hemem hemen her zaman haklıdırlar.
3. Bir nesneyi düşündüğünüzde, ilk başta kelimeler olmadan düşünürsünüz. Ne zaman bu düşündüğünüz nesneyi anlatmak isterseniz, bu nesneye birebir anlatan kelimeleri bulana kadar _ava çıkarsınız._ 
