# OOP-Rehber
Üniversite öğrencileri için OOP sınavları için internette kaliteli "Türkçe Kaynak" oluşturma amacıyla hazırlanmaya başlamıştır. Hataları af buyrunuz.
Öncelikle yararlı linkleri bırakmakla başlayalım: 
### OOP ve Java için daha önceden yazdığım kısım için ![buraya tıklayabilirsiniz](https://github.com/tonyukukkula/OOP-Rehber/blob/main/src/oop_java.pdf)
### 20bin satırlık Java Öğretici İçerik için: ![buradan asıl repoya ulaşabilirsiniz](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md)
### Extends nedir? ![buradan ulaşabilirsiniz](https://yazilimcity.net/java-extends-nedir-ne-ise-yarar-nasil-kullanilir/)
20bin satır gözünüzü korkutuyorsa seçtiğim yerlere bakabilirsiniz:
* Sorular
  * ![OOP Programlama Egzersizleri (Kolay)](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-04-programming-exercise-pe-oop-01)
  * ![Nesne kompozisyonu](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-06-understanding-object-composition)
  * ![Nesne Sınıfı ve Kalıtım, Çıktısını Tahmin edin](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-09--introducing-object-class)
  * ![Kalıtım ve Metot Üstelemesi(Overriding), Çıktısını Tahmin edin](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-10-inheritance-and-method-overriding)
    * ![Kalıtımlı Kod yazalım](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-11-classroom-exercise-ce-oop-01)
  
* Neden
  * ![Neden Kalıtım?](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-07-the-need-for-inheritance)
  * ![Super cidden super mi?](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-12-constructors-and-calling-super)
  * ![Soyut Sınıflar](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-14-introducing-abstract-classes)
    * ![Soyut Sınıflar Neden var?](https://github.com/in28minutes/java-tutorial-for-beginners/blob/master/README.md#step-15-abstract-classes---design-aspects)

Bunlardan sonra zaman buldukça güncelleyeceğim kısımlara geçelim.
  
## Polymorphism ve Trace (Çok Biçimlilik ve Çıktı Tahmin)
```java 
class insan {
	public String isim;
	public int no;

	public insan() {
		this(1907, "fener");
	}

	public insan(int no, String isim) {
		this.isim = isim;
		this.no = no;
	}

	public void yaz() {
		System.out.println(this.isim + " " + this.no);
	}
}
```
Yukarıdaki kod örneğinde basit ve get&set metodları olmayan bir *insan* sınıfı oluşturduk. şimdi insan sınıfının daha özelleşmiş bir biçimi olan *calisan* sınıfını oluşturalım.
```java
class calisan extends insan {
	public int isno;
	public String isim_is;

	public calisan() {
		super();
	}

	public calisan(int a, String b) {
		super(a, b);
	}

	public calisan(int a, String b, int c, String d) {
		super(a, b);
		this.isno = c;
		this.isim_is = d;
	}

	public String yazdir() {
		return "calisan [isno=" + isno + ", isim_is=" + isim_is + ", isim=" + isim + ", no=" + no + "]";
	}
}
```

**Dikkat ettiğiniz gibi bu sınıflar _toString()_ adında olmayan ama aynı işleve sahip _yaz()_ ve _yazdir()_ fonksiyonlarına sahiptir. Bu fonksiyonlardan birisi void olarak diğeri String tanımlanmış. Çıktı Tahmin sorularında bunlar gibi nüanslara dikkat buyurunuz.**

Yukarıdaki kodu özetlememiz gerekirse öncelikle bir _insan_ sınıfı oluşturduk sonrasında buradan _calisan_ sınıfını genişlettik. Kalıtımın neden gerekli olduğunu anlatmak bu örnekten pek mümkün gözükmüyor o yüzden hayal etmenizi isteyeceğim: eğer _insan_ özelliklerini ve metotlarını taşıyan başka bir sınıf yazmamız gerekseydi ve ortada bir _insan_ sınıfı olmasaydı nolurdu? Cevap basit sadece biraz daha kod yazardık, e neden yazmıyoruz? Çünkü mühendis insan _bir miktar_ tembeldir.

Konudan bir miktar saptıktan sonra gene çok biçimliliğe dönelim. Burdaki kod parçacıkları birbiriyle ilişkilidir ve aralarında hiyerarşi vardır.

* Minik görev: Buradaki kod parçacığının UML diyagramını çiziniz.

Trace Sorumuza gelelim:
```java
	public static void main(String[] args) {
		insan Alperen = new calisan(51, "Mühendislik", 1634, "ALPEREN");
		System.out.println(Alperen.yazdir()); 
	}
```
<details><summary>Cevap İçin Tıklayınız</summary>
<p>
Hata verir çünkü Alperen insandan halt oldu, typecast yapmadan göremez.
Typecast yani "(calisan) Alperen"; typecast'in Türkçe'si tür dönüşümüdür.
Polymorhpism'in doğası budur.</br>
<b>Doğrusu:</b>
  
```java
	public static void main(String[] args) {
		insan Alperen = new calisan(51, "Mühendislik", 1634, "ALPEREN");
		System.out.println(((calisan) Alperen).yazdir()); 
	}
```
*Çıktı: calisan [isno=1634, isim_is=ALPEREN, isim=Mühendislik, no=51]*
</p>
</details>

Bu basit soruyu şöyle genişletelim:
```java
	public static void main(String[] args) {
    insan adana = new insan();
		adana.yaz();
		calisan gariban = new calisan();
		insan Alperen = new calisan(51, "Mühendislik", 1634, "ALPEREN");
		System.out.println(((calisan) Alperen).yazdir()); 
		gariban.yaz();
		System.out.println(gariban.yazdir());
	}
```
<details><summary>Cevap İçin Tıklayınız</summary>
<p>
fener 1907</br>
calisan [isno=1634, isim_is=ALPEREN, isim=Mühendislik, no=51]</br>
fener 1907</br>
calisan [isno=0, isim_is=null, isim=fener, no=1907]
</p>
</details>

*Çok önemli bilgi-1: interface görürseniz, interface'den kalıtılmış sınıflarda interface içinde soyutlanmış tüm fonksiyonları boş olsa bile yazmayı unutmayınız yoksa derleyici hatası alırsınız.*

## Hata İdaresi (Exception Handling)
Genellikle hataların derlenme zamanında(compile time) görülmesi istenir çünkü düzgün derlendiği için çalıştığı zannedilen bir ürün(yazılım) iş üstündeyken hata vermesi feci sonuçlar doğurabilir. Gerçi bu istek sınav zamanlarında geçerli değildir :). Her türlü hata için kullanabileceğiniz örnek bir kod kırpıntısını(code snippet) aşağıda görebilirsiniz. Bu kırpıntıda yalnızca 0'a bölüm hatası(divide by zero error) örnek gösterilmiştir ama herhangi bir hata için de bu kırpıntıyı kullanabilirsiniz
```java
import java.lang.Throwable; //tüm hataların en üst kümesi(super class)*1
public class Main
{
  public static void main (String[]args)
  {

    try//hata olursa bunun içinde olsun dediğimiz blok
    {
        System.out.println ("Hello World");
        int a = 666;
        int b = 0;
        System.out.println (a / b);//0'a bolme hatası
    } catch (Throwable e)//ne hatası olursa olsun throw edebilmek için, çünkü Throwable tüm hataların en üst kümesidir
    {
      System.out.println ("Dostum burada bir sikinti var!\n" + e);//hata olduğunda ne olduğunu(neyin throw edildiğini) e ile çıktı veririz.
    }
  }
}
```
![*1](https://www.javainterviewpoint.com/exception-handling-in-java-a-complete-guide-to-java-exceptions/)
