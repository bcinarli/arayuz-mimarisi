# Sıralama
CSS içerisinde yapılan tanımların tarayıcılar tarafından işlenmesi yukarıdan aşağıya doğru yapıldığı için, bir seçici için yapılan son tanım geçerli olur. Bazı istisnalar dışında bu durum her zaman geçerlidir. Bu istisnaların temel kaynağı seçici spesifikliğidir. CSS içerisinde daha yukarılarda tanımlanmış öncelikli seçiciler (id seçici gibi) daha aşağıda tanımlanmış daha az öncelikli seçicilerdeki (class seçici gibi) tanımları ezebilir.

``` css
/** üst satırlarda ID seçici **/
#header {
	color: blue;
}

... // başka CSS tanımları

/** alt satırlada Class seçici **/
.header {
	color: red;
}
```

``` html
<div id="header" class="header">
	Başlık
</div>
```

Buradaki örnekte, "Başlık" metinin rengi _kırmızı_ değil _mavi_ olacaktır. Çünkü daha yukarıda olmasına rağmen daha öncelikli olan ID seçicinin tanımı tarayıcılar tarafından geçerli kabul edilecektir. Çoğu zaman bu istenmeyen durumun önüne geçmek için `!important` metodu kullanılarak, son tanımın geçerli olması sağlanmaya çalışılmaktadır.

``` css
.header {
	color: red !important;
}

// artık son tanım olan `kırmızı` renk görünür olacaktır.
```

`!important` kullanımı ilk başlarda avantaj sağlıyor gibi gözükse de, ilerleyen zamanlarda daha çok `!important` kullanımına sebep olmaktadır. Bu durum da sistemler üzerinde gereksiz kod yüküne ve kod bakımının giderek zorlaşmasına yol açacaktır.

CSS tanımlarında heç seçici sadeliğini sağlamak hem de kontrolü daha kolaylaştırmak için uygulama/proje içerisinde tanım setlerini belirli bir sıra ile hazırlamak gerekmektedir. Bu sıra temel olarak, genelden özele, küçük parçalardan büyük *component*lere ve değişebilen durumlardan final durum/görünüm şeklinde düşünülebilir. Bir liste halinde bunu düşünecek olacak olursak,

1. Ayarlar (*Settings*)
2. Araçlar (*Tools*)
3. Genel Tanımlar/Reset (*Generic*)
4. Elemanlar (*Elements*)
5. Sayfa Dizilimi (*Layout*)
6. Küçük Parçalar (*Particules*)
7. Bileşenler (*Components*)
8. Yardımcı/Final Durumlar (*Auxiliary*)

Bu liste içerisindeki ilk iki madde özellikle Sass, Less gibi CSS önişlemci (*preprocessor*) kullanılan projeler için geçerliyken, diğer maddeler bütün arayüz geliştirme ihtiyacı olan sistemlere uygulanabilir.
