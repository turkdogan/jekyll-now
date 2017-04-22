---
layout: post
title: Mutable Immutable Muhabbeti!
---

Not: 28/02/2010 tarihinde daha önce kendi blogumda yazmış olduğum yazıdır.

Oldukça önemli bir kavram olan Değişmezlik kavramı ile ilgili bir iki şey de ben yazayım diyorum. Oldukça basit bir mantığa sahip olmasına rağmen, maalesef kod yazarken çok fazla dikkat etmediğimiz için başımıza bela açabilen bir kavram. Java dilinde birçok önemli sınıf Immutable olarak tasarlanmışken (String, Integer gibi) bazıları ise mutable yani değişebilir olarak tasarlanmış (Date sınıfı).

## Immutable Nesneler
Eminim aşağıdaki senaryo, hemen herkesin başına gelmiştir. Elinizdeki bir yazıyı büyük harfe çevirmek istediğimizi varsayalım. Bu durumda şöyle bir kod yazmış olabiliriz.

```java
    String s = "ankara";
    s.toUpperCase;
    System.out.println"upper ankara: " + s;
```

toUpperCase() methodu String içindeki harfleri büyütür ve döndürür. O yüzden bu kod çalıştırıldığında ve ekranda ANKARA yerine ankara görüldüğünde dumur olunabilir. Yapılması gereken şöyledir:

```java
    String s = "ankara";
    s = s.toUpperCase;
    System.out.println"upper ankara: " + s;
```

Olayı şöyle özetleyelim: String sınıfı bir Immutable sınıf olduğundan, bu sınıfa ait hiçbir method, değeri değiştiremez. Eğer değer değiştirilen bir method çağrılırsa (yukaridaki toUpperCase() methodu gibi) yeni bir String oluşturulur ve method çağrılması sonucunda hesaplanan sonuç yeni oluşturulan String değerine aktarılır ve bu String döndürülür. Varolan String aynen kalır. Bunun ne gibi bir faydası olabilir diye merak ediyor olabilirsiniz. Immutable objelerin faydaları:

-Durum değişikliği olmadığından, geliştirmek ve test etmek kolaydır
-Birden fazla thread çalışan uygulamalarda (hemen her büyük uygulama) güvenli bir şekilde çalışırlar
-Clone, copy constructor gibi tekniklere ihtiyaç kalmaz
-Değişmeyecekleri için Map, Set gibi veri yapılarında anahtar (key) olarak kullanılabilirler.

## StringBuffer StringBuilder
Tecrübeli insanların bangır bangır StringBuffer kullanın demesi de aslında Stringlerin immutable olması ile ilgili. Ne alaka diyenlere hemen bir örnek verelim:

```java
    public static String copyString value, int times 
        String data = "";
        String ekle = value + " ";
        forint i = 0; i < times; i++ 
            data += ekle; 
        
        return data;
```

Yukarıdaki kod, herhangi bir String değerini times kadar kopyalar ve birbirine ekleyerek döndürür. Örneğin copy(gelistir, 3) çağrıldığında, gelistir gelistir gelistir döndürülür. Herşey yolunda gibi görünüyor ama burada dikkat edilmesi gereken += işlemi. String immutable olduğu için her ekleme işleminde yeni bir String oluşturuluyor ve en sonunda bu değer döndürülüyor. Farzedelim ki 1.000.000 kere kopyalamak isteyelim. Bu durumda oluşan gereksiz String obje sayısını söylemeye gerek yok. Sonuç olarak bellek ve performans problem kaçınılmaz olacaktır.

StringBuffer ve StringBuilder aynı amaç için kullanılır ve aynı işi yaparlar. Bu iki nesne, immutable her işlemde yeni bir String objesi yerine, sadece bir obje oluştururlar. Buffer kullanarak yeni gelen Stringleri bu buffera kopyalarlar. Bu buffer dolduğunda ise, boyutu dinamik olarak arttırılır. Aralarındaki tek fark, birden fazla Thread aynı anda bu nesneleri kullanacağı zaman belirginleşir. Birden fazla thread (multithread) olan uygulamalarda StringBuffer kullanılmalıdır. StringBuffer çalışırken diğer Threadlerin erişmesini engeller. Dolayısıyla veri bütünlüğü korunmuş olur. Eğer uygulamada sadece tek bir Thread çalışacaksa o zaman performans açısından StringBuilder kullanımı tercih edilmelidir.

Yukarıda bahsedilen problemi ise StringBuffer ya da StringBuilder kullanarak çözebiliriz. Bu durumda kodu şu şekilde yazabiliriz:

```java
    public static String copyString value, int times 
        StringBuilder sb = new StringBuilder;
        forint i = 0; i < times; i++ 
            sb.appendvalue.append" ";
        
        return sb.toString;
```

Aradaki farkı anlamak için Java VisualVM (jvisualvm) programını deneyebilirsiniz. Bu program, JDK ile birlikte kurulmaktadır ve o an sistemde çalışan Java uygulamaları ile ilgili çeşitli bilgiler sunar. Örneğin uygulamanın ne kadar bellek tükettiğini, işlemciyi ne kadar zorladığını ya da hangi sınıfların ne kadar kullanıldığını bu program ile öğrenebilirsiniz.

## Mutable Neseneler
Mutable nesneler ise yine hataya sebep olabilecek en önemli sebeplerden birisidir. Bu nesnelere ait objelerin değer değiştirilebilir. En dikkat edilmesi gereken durumlar ise immutable bir objenin döndürülmesidir. Bu durumda, objemiz döndürülen sınıf içerisinde istendiği gibi değiştirilebilir. Örnek vermek gerekirse; bir Person sınıfı yazdığımız düşünelim. Bu sınıf içinde bir kişiye ait isim ve doğum tarihi tutulsun. Şu şekilde kodladığımızı varsayalım:

```java
import java.util.Calendar;
import java.util.Date;
 
public class Person 
 
    private String name;
    private Date birthday;
 
    public Person 
        setName"Ahmet";
        setBirthdaynew Date1990, 2, 10;
    
 
    public String getName 
         return name;
    
 
    public void setNameString name         
        this.name = name;
    
 
    public Date getBirthday 
        return birthday;
    
 
    public void setBirthdayDate birthday 
        this.birthday = birthday;
    
 
    public void yasKac 
        int buSene = Calendar.getInstance.getCalendar.YEAR;
        int dogumSenesi = getBirthday.getYear;
        int yas = buSene - dogumSenesi;
        System.out.println"Yas: " + yas;
    
 
    public static void mainString args 
        Person p = new Person;
        p.yasKac;
        p.getBirthday.setYear2020;
        p.yasKac;
```

Çalıştırıldığında şu çıktının olması beklenir:

```java
Yas: 20
Yas: -10
```

Date sınıfı mutable bir sınıf olduğu için ve getDate() methodu direk bu değeri döndürdüğü için, bu değer ile ilgili işlem yapan main methodu istediği gibi doğum tarihini değiştiriyor. Date sınıfına ait birçok methodun kullanımı önerilmemektedir (Deprecated). setYear(), setMonth() gibi methodları, olası hataların oluşmasına engel olmak için kullanmamak gerekiyor. Bu uygulama çalıştırıldığında, kişinin yaşı ilk önce 20, daha sonra -10 olarak görünür. 10 sene sonra doğacak birisine Ahmet ismini vermek çok mantıklı olmasa gerek! Daha vahimi ise Person sınıfına ait p objesinin ise bu işlemden haberi olmaması.

Buradaki işlem, nesneye yönelik programlamanın belki de en önemli prensibini deliyor; Encapsulation. Yani bir sınıf içerisindeki verileri sadece o sınıfa ait bir objenin değiştirebilmesi olayı. Başka hiçbir sınıf, obje içindeki verileri doğrudan değiştirememelidir. Değiştirme işlemi, sadece ilgili methodlar çağrılarak halledilmelidir.

Bahsedilen problemi çözmenin en mantıklı yolu getDate() methodunu şu şekilde tekrar yazmak olabilir:

```java
  public Date getBirthday 
        return new Datebirthday.getTime;
```
Her getBirthday() methodu çağrıldığında, yeni bir Date objesi oluşturulup, var olan tarihin değiştirilmesine izin verilmemektedir. Bu şekilde çalıştırılırsa, yaş değerleri ekranda;

```java
Yas: 20
Yas: 20
```
