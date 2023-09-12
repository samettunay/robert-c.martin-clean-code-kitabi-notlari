# Clean Code Kitabı Notları

Bu GitHub projesi, Robert C. Martin'in "Clean Code" kitabının özet ve notlarını içermektedir. Bu notlar, kitaptaki önemli konuları ve prensipleri özetlemek ve temiz kod yazma konusunda rehberlik etmek amacıyla oluşturulmuştur.

**İçerik:**
- Kitabın Ana Konuları
- Örnek Kod Parçacıkları ve Açıklamalar
- Temiz Kod Prensipleri


## Clean Code - Chapter 1


![image](https://github.com/samettunay/robert-c.martin-clean-code-kitabi-notlari/assets/79511355/5d48e083-d516-49d6-87ef-2fa59c0c76b8)

Kodun bir gün ortadan kaybolacağını düşünenler, formal olmayan bir matematik bulmayı uman matematikçilere benzerler. Bir gün istediğimiz şeyleri yapabilen makineler oluşturmanın yolunu umuyorlar, ancak bu makineler bizi o kadar iyi anlamalı ki, belirsiz belirtilmiş gereksinimleri mükemmel bir şekilde gerçekleştiren ve tam olarak bu gereksinimlere uyan programlara çevirebilmelidirler. Bu asla gerçekleşmeyecektir. Hatta insanlar bile, tüm sezgi ve yaratıcılıklarına rağmen, müşterilerinin belirsiz duygularından başarılı sistemler oluşturamamışlardır. Gerçekten de, gereksinimlerin iyi bir şekilde belirtilmesi, bize her zaman kod kadar formal ve kodun yürütülebilir testleri olarak hareket edebileceğini göstermiştir. Unutmayın ki kod, sonunda gereksinimleri ifade ettiğimiz dilin kendisidir. Gereksinimlere daha yakın diller oluşturabiliriz. Bu gereksinimleri formal yapılarına çevirmemize yardımcı olan araçlar oluşturabiliriz. Ancak gerekli hassasiyeti asla ortadan kaldıramayız, bu yüzden her zaman kod olacaktır.

## Meaningful Names - Chapter 2

![image](https://github.com/samettunay/robert-c.martin-clean-code-kitabi-notlari/assets/79511355/46cb452f-fd4e-4bc0-b2c1-066ba59edc83)

Bir değişkenin, fonksiyonun veya sınıfın adı tüm büyük sorulara cevap vermelidir. Size neden var olduğunu, ne yaptığını ve nasıl kullanıldığını anlatmalıdır. Bir ad yorum gerektiriyorsa, ad amacını açıklamaz.

```java
int d; // elapsed time in days
```

d ismi hiçbir şeyi açığa çıkarmaz. Geçen zaman ya da günler hissini uyandırmaz. Neyin ölçüldüğünü ve o ölçümün birimini belirten bir isim seçmeliyiz:

```java
int elapsedTimeInDays;
int daysSinceCreation;
int daysSinceModification;
int fileAgeInDays;
```

Amacı ortaya koyan adları seçmek, kodu anlamayı ve değiştirmeyi çok daha kolay hale getirebilir. 

Bu kodun amacı nedir?

```java
 public List<int[]> getThem() {
    List<int[]> list1 = new ArrayList<int[]>();
    for (int[] x : theList)
        if (x[0] == 4)
            list1.add(x);
    return list1;
 }
```

- Listede ne tür şeyler var?
- Listedeki bir öğenin sıfırıncı alt simgesinin önemi nedir? x[0]
- 4 değerinin önemi nedir?
- Döndürülen listeyi nasıl kullanırım?


Bu soruların yanıtları kod örneğinde mevcut değil ancak olabilir. Bir mayın temizleme oyununda çalıştığımızı varsayalım. Tahtanın **theList** adı verilen hücrelerin bir listesi olduğunu görüyoruz. Bunu **gameBoard** olarak yeniden adlandıralım.

Tahtadaki her hücre basit bir diziyle temsil edilir. Ayrıca sıfırıncı alt simgenin bir durum değerinin yeri olduğunu ve 4 durum değerinin **"flagged"** anlamına geldiğini de bulduk. Sadece bu kavramlara isim vererek kodu önemli ölçüde geliştirebiliriz:

```java
public List<int[]> getFlaggedCells() {
    List<int[]> flaggedCells = new ArrayList<int[]>();
        for (int[] cell : gameBoard)
            if (cell[STATUS_VALUE] == FLAGGED)
                flaggedCells.add(cell);
        return flaggedCells;
 }
 ```

Kodun basitliğinin değişmediğine dikkat edin. Hala tam olarak aynı sayıda operatör ve sabite ve tam olarak aynı sayıda yuvalama düzeyine sahiptir. Ancak kod çok daha açık hale geldi.

Daha da ileri gidebilir ve bir dizi int kullanmak yerine hücreler için basit bir sınıf yazabiliriz. Sihirli sayıları gizlemek için niyeti ortaya çıkaran bir işlev içerebilir (buna isFlagged adını verin). İşlevin yeni bir sürümüyle sonuçlanır:

```java
public List<Cell> getFlaggedCells() {
    List<Cell> flaggedCells = new ArrayList<Cell>();
    for (Cell cell : gameBoard)
        if (cell.isFlagged())
            flaggedCells.add(cell);
    return flaggedCells;
}
 ```

Bu basit isim değişiklikleriyle olup biteni anlamak hiç de zor değil. Bu, iyi isimler seçmenin gücüdür.

### Dezenformasyondan Kaçının

Programcılar kodun anlamını gizleyecek yanlış ipuçları bırakmaktan kaçınmalıdır. Bir hipotenüs kodluyor olsanız ve hp iyi bir kısaltma gibi görünse bile, bu yanıltıcı olabilir.


Aslında bir Liste olmadığı sürece, bir hesap grubuna **accountList** olarak başvurmayın. **List** programcılara özel bir şey ifade eder. Hesapların bulunduğu kapsayıcı aslında bir Liste değilse, yanlış sonuçlara varılabilir. Yani **accountGroup** veya **bundleOfAccounts** veya sadece düz hesaplar daha iyi olur.

Küçük şekillerde farklılık gösteren adları kullanmaktan kaçının. Tek bir modüldeki **XYZControllerForEfficientHandlingOfStrings** ile biraz daha uzak bir yerde bulunan **XYZControllerForEfficientStorageOfStrings** arasındaki ince farkı tespit etmek ne kadar sürer? Kelimelerin korkunç derecede benzer şekilleri var.

Geliştirici muhtemelen sizin bol yorumlarınızı ve hatta o sınıf tarafından sağlanan yöntemlerin listesini görmeden bir nesneyi ismine göre seçecektir.

Dezenformatif adlara gerçekten berbat bir örnek, değişken adları olarak küçük harf **L** veya büyük harf **O**'nun özellikle kombinasyon halinde kullanılması olabilir. Elbette ki sorun, bunların neredeyse tamamen sırasıyla bir ve sıfır sabitlerine benzemesidir.

```java
int a = l;
if ( O == l )
    a = O1;
else
    l = 01;
 ```

 ### Anlamlı Ayrımlar Yapın

 Sayı serisi adlandırma **(a1, a2, .. aN)**, kasıtlı adlandırmanın tersidir. Bu tür isimler bilgi bozucu değildir; bilgi verici değildir; yazarın niyetine dair hiçbir ipucu vermezler.

 ```java
public static void copyChars(char a1[], char a2[]) {
    for (int i = 0; i < a1.length; i++) {
        a2[i] = a1[i];
    }      
}
 ```

 Bu işlev, bağımsız değişken adları için **a1** ve **a2** yerine **source** ve **destination** kullanıldığında çok daha iyi okunur.

Nerede resimlendiğini bildiğimiz bir uygulama var. Suçluyu korumak için isimleri değiştirdik ancak hatanın tam şekli şu şekilde:

 ```java
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
 ```

Bu projedeki programcıların bu işlevlerden hangisinin çağrılacağını nasıl bilmeleri gerekiyor?

Belirli kuralların yokluğunda, **moneyAmount** değişkeni **money** ayırt edilemez, **customerInfo** **customer**'dan ayırt edilemez, **accountData** **account**'dan ayırt edilemez ve **theMessage** **message** ayırt edilemez. İsimleri, okuyucunun farklılıkların neler sunduğunu bilmesini sağlayacak şekilde ayırın.

### Telaffuz Edilebilen İsimler Kullanın

İnsanlar kelimeler konusunda iyidir. Beynimizin önemli bir kısmı kelime kavramına ayrılmıştır. Ve kelimeler tanım gereği telaffuz edilebilirdir. Beynimizin konuşulan dille başa çıkmak üzere gelişen devasa kısmından faydalanmamak utanç verici olurdu. Bu yüzden isimlerinizi telaffuz edilebilir yapın

Eğer telaffuz edemiyorsan, aptal gibi görünmeden tartışamazsın. "Peki, burada, three cee enn tee, there, pee var, zee kyew int var, gördün mü?" Bu önemlidir çünkü programlama sosyal bir aktivitedir. ( :D )

Şirket içinde eğlenceli olsun ya da olmasın, kötü adlandırmalara tolerans göstermeyin. Yeni geliştiricilerin değişkenlerin kendilerine açıklanması gerekir.

```java
class DtaRcrd102 {
    private Date genymdhms;
    private Date modymdhms;
    private final String pszqint = "102";
    /* ... */
};
 ```

to

 ```java
class Customer {
    private Date generationTimestamp;
    private Date modificationTimestamp;;
    private final String recordId = "102";
    /* ... */
};
 ```

### Use Searchable Names



