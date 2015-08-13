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

CSS tanımlarında hem seçici sadeliğini sağlamak hem de kontrolü daha kolaylaştırmak için uygulama/proje içerisinde tanım setlerini belirli bir sıra ile hazırlamak gerekmektedir. Bu sıra temel olarak, genelden özele, küçük parçalardan büyük *component*lere ve değişebilen durumlardan final durum/görünüm şeklinde düşünülebilir. Bir liste halinde bunu düşünecek olacak olursak,

1. Ayarlar (*Settings*)
2. Araçlar (*Tools*)
3. Genel Tanımlar/Reset (*Generic*)
4. Elemanlar (*Elements*)
5. Sayfa Dizilimi (*Layout*)
6. Küçük Parçalar (*Particules*)
7. Bileşenler (*Components*)
8. Yardımcı/Final Durumlar (*Auxiliary*)

Bu liste içerisindeki ilk iki madde özellikle Sass, Less gibi CSS önişlemci (*preprocessor*) kullanılan projeler için geçerliyken, diğer maddeler bütün arayüz geliştirme ihtiyacı olan sistemlere uygulanabilir. Listeyi detaylıca inceleyecek olursak;

## 1. Ayarlar
Sass veya Less kullanılan sistemlerde CSS kodlarının tamamında kullanılacak değişkenleri en önce yüklemek gerekecektir. Bu değişkenlerin kapsamı, tasarımın renklerinden, responsive kodlama için *breakpoint* tanımlarına kadar her türlü parametrik kullanımı kapsayabilir.

*Örneğin:*

```scss
$base-spacing: 1em;
$base-input-spacing: 5px;
$base-gutter: 20px;
$base-grid-columns: 12;

$colors: (
	primary   : #c54846,
	secondary : #0072bc,
	text      : #222
);

$fonts: (
	size: 16px,
	"line-height": 1.5,
	text: '"OpenSans", Helvetica, Arial, sans-serif',
	title: '"Roboto Slab", Georgia, Times, serif'
);
```

## 2. Araçlar
Yine Sass ve Less kullanırken faydalı olabilecek, tekrar eden tanımları kolay yapmak için kullanabileceğimiz *mixin* ve *fonksiyon*ları, ayarlardan çağırarak, parametlerin bu *mixin* ve *fonksiyon*lar ile uyumlu çalışması sağlanacaktır.

```scss
@mixin dims($width, $height: null) {
    width: $width;
    @if $height == null {
        height: $width;
    }
    @else {
        height: $height;
    }
}
```

## 3. Genel Tanımlar/Reset
Genel tanımlar kısmına daha çok HTML elemanların tüm tarayıcılarda ortak gözükmesi ya da ortak davranması istenen durumlar ile alakalı kodlar bulunabilir. Yine `float` edilmiş elemanların *wrapper*larının düzgün gözükmesi için bolca kullanılan `clearfix` metodları, `html`, `body` etiketleri gibi tasarım ve uygulamadan çok sayfanın kendisiyle alakalı tanımlar burada bulunabilirler. Ayrıca yine bu aşamada sisteme yüklenecek *reset* kodlarının, herşeyi sıfırlayan bir reset tanımı yerine tarayıcılarda elemanların aynı gözükmesini sağlayan *normalize.css* gibi bir reset olması, sıfırlanan herşeyin tekrar tanımlanması ihtiyacını da ortadan kaldıracaktır.

*Örneğin:*

``` scss
/**
 * Grid ve benzeri kullanımlarda daha rahat genişlik hesaplamak için
 * bütün elemanların `box-sizing` tanımını `border-box` olarak ayarlayalım
 */
html {
    box-sizing: border-box;
}

* {
    &,
    &:after,
    &:before {
        box-sizing: inherit;
    }
}

/**
 * Clearfix
 * Daha anlamlı ve amacına uygun isimlendirmek için `group`
 * olarak tanımladık. Zaten burada yapılan iş de, elemanları
 * yan yan bir "grup" için de düzgün olarak göstermek :)
 */
.group {
    &:after {
        content: "";
        display: table;
        clear: both;
    }
}
```

## 4. Elemanlar
Genel tanımlardan sonra yapılacak tanımlar yine daha genel geçer olan ve geliştirme sırasında işleri kolaylaştıracak şekilde HTML.etiketlerine doğrudan yapılacak tanımlar gelmektedir. Burada elemanlar olarak bahsedilen grup doğrudan *tag*lerin kendisidir. Başlık etiketleri `h1` - `h6` dan, liste etiketleri `ul`, `ol`, `li`, `dl` gibi özellikle içeriklerin sınıflandırılması, pozisyonlandırılması için kolaylık sağlayan tanımlar yapılabilir. Dikkat edilmesi gereken noktalardan biri bu kısımlarda, __font__, __renk__ gibi görünüme dayalı tanımlardan çok, __margin__, __padding__ gibi pozisyon, *alignment* gibi tanımlar önceliklendirilmelidir. Daha görselliğe dayalı tanımlar için *component* içleri doğru tercih olacaktır. Bu grupta hala genel tanımlar yapıldığı için, *overwrite* yapma ihtimali olan tanımlardan kaçınıp, ortak özellikleri ön plana çıkartmak faydalı olacaktır. 

## 5. Sayfa Dizilimi

## 6. Küçük Parçalar

## 7. Bileşenler

## 8. Yardımcı/Final Durumlar
Kodlar içerisine en son eklenmesi beklenen tanımlar daha çok bir elemanın ya da *component*in ilgili seçici eklendiğinde gözükeceği son durumları belirler. Bir elemanın gizlenmesi, belirli bir pozisyonda kalması gibi durumları bu kategoride sayabiliriz.

*Örneğin*
``` scss
.is-hidden {
	display: none;
}

.mobile-hidden {
	@media screen and (max-width: 414px){
		display: none;
	}
}
```
