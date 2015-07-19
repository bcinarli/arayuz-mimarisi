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

```
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