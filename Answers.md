# Java Core Concepts Q&A's

## 1 – Why do we need to use OOP? Some major OOP languages?



OOP'yi kullanmamızın temel nedeni, yazılım geliştirmeyi daha düzenli, esnek ve sürdürülebilir hale getirmektir.

**Avantajları:** 

- **Encapsulation (Kapsülleme):** Verileri korur ve gizler.
- **Inheritance (Kalıtım):** Kod tekrarını azaltır ve yeniden kullanım sağlar.
- **Polymorphism (Çok Biçimlilik):** Aynı metodu farklı şekillerde kullanma imkanı tanır.
- **Abstraction (Soyutlama):** Karmaşıklığı azaltır, gereksiz detayları gizler.

**Major OOP languages:**
- Java
- C++
- Python
- C#

## 2 – Interface vs Abstract class?



### Abstract Class
- Nesne oluşturulamaz.
- Hem soyut (abstract) hem de normal (concrete) metotlar içerebilir.
- Field (değişken) tanımlanabilir ve bunlara erişim belirleyicileri (public, private, protected) uygulanabilir.
- Constructor içerebilir.
- Bir sınıf yalnızca bir soyut sınıfı miras alabilir (extends).

### Interface

- Tüm metotlar varsayılan olarak soyuttur (abstract) ve public'tir.
- Değişkenler varsayılan olarak public, static ve final'dir.
- Constructor içeremez.
- Bir sınıf birden fazla interface'i implemente edebilir (implements).
- Java 8 ve sonrası: default ve static metotlar içerebilir.


## 3 – Why do we need `equals` and `hashCode`? When to override?



`equals()`: İki nesnenin aynı olup olmadığını belirlemek için kullanılır. Varsayılan implementasyon, bellek adreslerini karşılaştırır. (yani, iki nesne **aynı referansa** sahip mi diye bakar)

`hashCode()`: Nesnenin hash kodunu döndürür. Hash tabanlı koleksiyonlarda nesneleri daha hızlı bulmak için kullanılır.

Varsayılan `equals()` metodu referansları karşılaştırdığı için, **aynı içeriğe sahip iki nesne eşit kabul edilmez.**
Eğer iki nesnenin içeriğine göre eşit olup olmadığını kontrol etmek istiyorsak, equals() metodunu override etmeliyiz.


Eğer bir nesneyi `HashMap` anahtarı (key) veya HashSet elemanı olarak kullanacaksak, `hashCode()` metodunu da override etmeliyiz.

**_`hashCode()` neden önemli?_**

Hash tabanlı veri yapıları, nesneleri daha hızlı bulmak için `hashCode()` değerini kullanır.
Eğer `hashCode()` doğru şekilde override edilmezse, HashMap veya HashSet nesneleri yanlış gruplandırabilir.

## 4 – Diamond problem in Java? How to fix it?



* Çoklu kalıtım (multiple inheritance) nedeniyle oluşur.
* 
* Java'da bir sınıf yalnızca bir başka sınıftan türeyebilir (extends ile).
* Ancak, birden fazla interface implemente edilebilir (implements ile).
* Eğer iki farklı interface aynı metodu tanımlarsa ve bir sınıf her iki interface'i de implemente ederse, hangi metodun çağrılacağı belirsiz hale gelir.
Bu çakışma **_"Diamond Problem"_** olarak adlandırılır.
* 

**Java, çoklu sınıf kalıtımına izin vermez.**

Sadece tek bir sınıftan extends edilebilir. Bu yüzden "class-based" diamond problem oluşmaz.

Fakat Interface'lerde varsayılan metotlar (default methods) kullanılırsa problem oluşabilir.
Interface’lerde çakışma olursa, metodu override ederek veya belirli bir interface’i çağırarak çözülür.

## 5 – Why do we need a Garbage Collector? How does it run?



* Kullanılmayan nesneleri otomatik olarak temizleyerek bellek sızıntılarını önler.
* Ulaşılmayan nesneleri kaldırarak belleği optimize eder.
* Elle bellek yönetimiyle uğraşmak yerine, geliştiricinin işini kolaylaştırır.

Önce hangi nesnelerin hala erişilebilir olduğunu belirler (Mark), sonra kullanılmayanları siler (Sweep) ve bellek parçalanmasını önlemek için kalan nesneleri düzenler (Compact). 
Java, nesneleri "Young" ve "Old" jenerasyonlarına ayırarak sık kullanılan nesneleri koruyup kısa ömürlüleri hızlıca temizler.
JVM tarafından gerektiğinde otomatik çalıştırılır, ancak `System.gc()`; ile öneri olarak çağrılabilir.



## 6 – Java `static` keyword usage?



`static`, bir sınıfa ait olan ve nesne oluşturulmadan erişilebilen üyeleri tanımlamak için kullanılır.

### Static Variables (Statik Değişkenler)

* Sınıfa aittir, tüm nesneler tarafından paylaşılır.
* Bellekte sadece bir kez oluşturulur.
* Sabit değerler veya tüm nesneler için ortak değişkenler için kullanılır.

### Static Methods (Statik Metotlar)

* Nesne oluşturmadan çağrılabilir.
* Sadece diğer static üyelerle çalışabilir (instance değişkenlerine erişemez).
* Matematiksel işlemler, yardımcı (utility) metotlar için uygundur.

###  Static Blocks (Statik Bloklar)

* Sınıf yüklendiğinde bir kez çalışır.
* Statik değişkenlerin başlatılması için kullanılır.

### Static Nested Classes (Statik İç Sınıflar)

* Bağlı olduğu sınıfın nesnesi olmadan kullanılabilir.
* Bağımsız yardımcı sınıflar oluşturmak için uygundur.



## 7 – Immutability means? Where, How, and Why to use it?



Immutability, bir nesnenin oluşturulduktan sonra durumunun değiştirilememesi anlamına gelir. Immutable nesneler, herhangi bir değişiklik gerektiğinde mevcut nesneyi değiştirmek yerine yeni bir nesne oluşturarak çalışır.

### Nerede?

* **Java Standart Kütüphanesi**: String, Integer, Double, LocalDate gibi sınıflar immutable’dır.
* **Çok İş Parçacıklı (Multithreading) Ortamlar**: Veri tutarlılığını ve thread-safety sağlamak için tercih edilir.
* **Cache Mekanizmaları**: Immutable nesneler değiştirilemediği için tekrar kullanılabilir ve performans optimizasyonu sağlar.
* **Fonksiyonel Programlama**: Yan etkisiz (side-effect-free) kod yazmayı teşvik eder, bu da hata ayıklamayı kolaylaştırır.

### Nasıl?

* Tüm alanlar private ve final olmalıdır.
* Setter metotları tanımlanmamalıdır.
* Değer atamaları sadece constructor içinde yapılmalıdır.
* Eğer sınıf mutable nesne referansları içeriyorsa, bunların defensive copy (savunmacı kopya) yöntemiyle korunması gerekir.

### Neden?

* Thread-safety sağlar, böylece eşzamanlı erişimlerde veri bütünlüğü korunur.
* Yan etkisiz (side-effect-free) programlama imkanı sunar, hata ayıklamayı kolaylaştırır.
* Cache ve performans optimizasyonu sağlar, gereksiz bellek tahsislerini azaltır.
* Güvenliği artırır, çünkü immutable nesneler dışarıdan değiştirilemez ve veri manipülasyonu riskini azaltır.

## 8 – Composition and Aggregation means and differences?



### Composition

Bir nesnenin başka bir nesnenin ayrılmaz bir parçası olduğu güçlü bir ilişkiyi ifade eder.
**Alt nesne, üst nesne olmadan bağımsız olarak var olamaz ve üst nesne yok edildiğinde, alt nesne de otomatik olarak yok edilir.** Bu tür bir ilişki, _**bileşenlerin sıkı bir şekilde bağlandığı ve tek bir varlık olarak ele alındığı**_ durumlarda kullanılır.

### Aggregation

Aggregation, bir nesnenin başka bir nesne ile zayıf bir bağımlılık ilişkisi içinde olduğu durumu ifade eder. **Alt nesne, üst nesneden bağımsız olarak var olabilir ve üst nesne yok edilse bile alt nesne varlığını sürdürebilir.** Bu tür bir ilişki, n***esneler arasında esnek bir bağlılık*** gerektiğinde kullanılır.


| Özellik                 | Composition                                | Aggregation                         |
|-------------------------|--------------------------------------------|----------------------------------|
| **Bağımlılık**          | Güçlü (Strong Association)                 | Zayıf (Weak Association)         |
| **Yaşam Döngüsü**       | Üst nesne silinirse, alt nesne de silinir. | Üst nesne silinse bile, alt nesne varlığını sürdürebilir. |
| **Alt Nesne**           | Üst nesnenin ayrılmaz bir parçasıdır.      | Üst nesneyle ilişkili olabilir ama bağımsızdır. |
| **Bağlantı Türü**       | "Has-A" (Sahiplik) - Güçlü Bağ             | "Has-A" (Sahiplik) - Zayıf Bağ |
| **Gerçek Hayat Örneği** | Otomobil ve Motor                          | Öğretmen ve Öğrenciler |


## 9 – Cohesion and Coupling means and differences?


Cohesion (Bağlılık) :

* Cohesion, bir sınıf veya modülün **ne kadar iyi tanımlanmış ve odaklanmış** olduğunu gösterir.
* Yüksek cohesion, **bir sınıfın sadece tek bir sorumluluğa sahip olması** ve belirli bir amacı yerine getirmesi anlamına gelir.
* İyi tasarlanmış sistemlerde **yüksek cohesion tercih edilir**, çünkü bakımı daha kolaydır ve kodun yeniden kullanılabilirliğini artırır.

Coupling (Bağımlılık) :
* Coupling, iki veya daha fazla modülün **birbirine ne kadar bağımlı olduğunu** ifade eder.
* Düşük coupling, sistem bileşenlerinin birbirinden bağımsız çalışmasını sağlar.
* Yüksek coupling ise, bileşenlerin sıkı bağımlı olduğu ve değişiklik yapmanın zorlaştığı durumlardır.
* İyi yazılım tasarımında **düşük coupling tercih edilir**, çünkü sistem daha esnek ve sürdürülebilir olur.


| Özellik                       | Cohesion (Bağlılık)                | Coupling (Bağımlılık)              |
|-------------------------------|--------------------------------|--------------------------------|
| **Tanım**                     | Bir sınıfın tek bir sorumluluğa odaklanma derecesi. | Bir sınıfın başka sınıflara bağımlılık derecesi. |
| **Tercih Edilen**             | **Yüksek Cohesion** (Yüksek bağlılık) | **Düşük Coupling** (Düşük bağımlılık) |
| **Bakım Kolaylığı**           | Daha kolay, çünkü her sınıf tek bir iş yapar. | Daha zor, çünkü bir değişiklik diğer bileşenleri etkileyebilir. |
| **Yeniden Kullanılabilirlik** | Yüksek, modüller bağımsız çalışabilir. | Düşük, modüller birbirine bağımlıdır. |
| **Gerçek Hayat Örneği**       | Dosya işlemlerinin `DosyaOku` ve `DosyaYaz` olarak ayrılması. | Veritabanı işlemlerinin doğrudan bir sınıfa bağlanması yerine, bağımsız bir `DatabaseService` kullanılması. |

## 10 – Heap and Stack means and differences?



Heap Memory (Yığın Bellek) :
* Heap, Java uygulamasında **nesnelerin ve dizilerin** dinamik olarak tahsis edildiği bir bellek alanıdır.
* Heap, **Garbage Collector (GC) tarafından yönetilir** ve nesneler bellekte gerektiği sürece varlığını sürdürür.
* Büyük veri yapıları ve uzun süreli nesneler heap üzerinde saklanır.


Stack Memory (Yığın Bellek) Tanımı:
* Stack, **metod çağrıları, yerel değişkenler (primitive türler ve referanslar) ve yürütme sırası** için kullanılan bir bellek alanıdır.
* Stack, **Last In, First Out (LIFO)** prensibi ile çalışır, yani en son eklenen veriler ilk önce kaldırılır.
* Bir metod çağrıldığında, stack üzerine bir çerçeve (frame) eklenir ve metod tamamlandığında kaldırılır.


| Özellik           | Heap Memory (Yığın Bellek)           | Stack Memory (Yığın Bellek)         |
|------------------|----------------------------------|----------------------------------|
| **Ne Saklanır?**  | Nesneler (`new` ile oluşturulanlar)  | Yerel değişkenler, metod çağrıları |
| **Yönetim**      | Garbage Collector (GC) yönetir  | Otomatik olarak yönetilir (LIFO) |
| **Hız**         | Daha yavaş, çünkü GC çalışır  | Daha hızlı, doğrudan yönetilir  |
| **Yaşam Süresi** | Program çalıştığı sürece sürebilir  | Metod tamamlandığında temizlenir |
| **Yer Tahsisi**  | Dinamik (İhtiyaca göre genişler) | Statik ve metod bazlı tahsis edilir |
| **Bellek Boyutu** | Daha büyük, genişletilebilir  | Daha küçük, sınırlı |
| **Hata Riski**   | `OutOfMemoryError` (Aşırı kullanımda) | `StackOverflowError` (Derin çağrılarda) |


## 11 – Exception means? Types of Exceptions?



* Exception, programın normal akışını bozan **çalışma zamanı hatalarıdır**.
* Java'da hataları yönetmek için `try-catch-finally` ve `throws` yapıları kullanılır.


**Checked Exception (Denetlenen İstisnalar)**
* Derleme zamanında kontrol edilir.
* `IOException`, `SQLException` gibi istisnalar bu gruba girer.

 **Unchecked Exception (Denetlenmeyen İstisnalar)**
* Çalışma zamanında oluşur, derleyici tarafından kontrol edilmez.
* `NullPointerException`, `ArrayIndexOutOfBoundsException` gibi hatalar içerir.

**Error (Hata)**
* JVM tarafından üretilen, genellikle kurtarılamayan hatalardır.
* `OutOfMemoryError`, `StackOverflowError` gibi hatalar örnektir.



## 12 – How to summarize ‘clean code’ as short as possible?

**Readable, maintainable, and well-structured code** that follows best practices like **SOLID, DRY, and KISS**.



## 13 – What is method hiding in Java?



* "Kolay okunabilir, bakımı ve genişletilmesi basit, gereksiz karmaşıklıktan uzak kod."

*  **Anlamlı İsimler:** Değişken, metod ve sınıf adları açık ve açıklayıcı olmalı.
*  **Tek Sorumluluk İlkesi (SRP):** Her sınıf ve metod yalnızca bir amaca hizmet etmeli.
*  **Kısa ve Anlamlı Metodlar:** Metodlar tek bir iş yapmalı, fazla uzun olmamalı.
*  **Kod Tekrarından Kaçınma (DRY):** Aynı kod farklı yerlerde tekrar edilmemeli.
*  **Yorum Satırlarını Minimumda Tutma:** Kod kendini açıklayacak kadar net olmalı.
*  **Basitlik ve Okunabilirlik:** Gereksiz karmaşıklıklardan kaçınılmalı.


## 14 – What is the difference between abstraction and polymorphism in Java?

**Abstraction (Soyutlama)**
* Gereksiz detayları gizleyerek **yalnızca gerekli bilgiyi sunma** prensibidir.
* `abstract` sınıflar veya `interface` kullanılarak uygulanır.

**Polymorphism (Çok Biçimlilik)**
* Aynı metodun farklı biçimlerde çalışabilmesini sağlar.
* **Metod Overriding** ve **Metod Overloading** şeklinde uygulanır.

| Özellik          | Abstraction (Soyutlama)                | Polymorphism (Çok Biçimlilik)         |
|-----------------|--------------------------------------|--------------------------------------|
| **Ne İşe Yarar?** | Gereksiz detayları gizler.         | Aynı işlemi farklı şekillerde yapar. |
| **Nasıl Kullanılır?** | `abstract class`, `interface`   | `Overriding`, `Overloading`         |
| **Amaç**         | Esnek ve bağımsız kod yazmak.       | Kod tekrarını azaltmak.             |
| **Örnek**        | Araba sürerken motor detaylarını bilmeyiz. | Farklı hayvanlar farklı ses çıkarır. |