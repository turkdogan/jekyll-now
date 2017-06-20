---
layout: post
title: AWK Programlama Dili
---

Not: 2010 tarihinde daha önce kendi blogumda yazmış olduğum yazıdır.

Bugün hocamız sınav sonuçlarını açıklamış. Öğrenci numarası ve yanında da aldığı not yazılı. Aşağıda notların bir kısmı listeleniyor.
 
```
002 80
015 56
043 80
053 10
053 80
086 95
100 65
119 90
132 80
137 70
148 70
156 80
...
```
 
Diyelim ki sınava giren kişi sayısını ve sınıf ortalamasının kaç olduğunu öğrenmek istediniz. Bunun için birkaç alternatif var:
<ul>
<li>Kağıt kalem yardımıyla toplama çıkarma yapmak</li>
<li>Hesap makinesi kullanmak
<li>C, Java gibi bir dil kullanılabilir (kolay gelsin!).</li>
<li>Python, Ruby gibi daha uygun bir dil tercih edilebilir</li>
<li>AWK kullanmak</li>
<li>...</li>
</ul>
 
Bence en verimli ve hızlı yol AWK dilini tercih etmek. AWK programlama dili yazı dosyaları ile çalışmayı kolaylaştıran, dosyanın içeriğindeki kolonları doğrudan kullanabilen, tonla iş yapabileceğiniz süper bir script dilidir. Aşağıda bu dilin oldukça basit bir kullanım biçimini yazılmıştır.
 
Verilen notlardan sınıf ortalamasını bulmak için yapılması gereken:: 

``` bash
awk "{toplam += $2; sayi++} END {print toplam, sayi, toplam/sayi}" notlar.txt
``` 
yazmaktan ibaret. Notların notlar.txt dosyasında tutulduğu varsayılıyor. $2 ise ikinici kolona yani notların bulunduğu kolona işaret ediyor. $1 ibaresi kullanılsaydı, öğrenci numaralarını hesaplamış olurduk. Sonuç şu şekilde gösterilir:
 
``` bash
4037 57 70.8246
```
 
Ortalama 70.82 imiş. Ortalamayı geçmişiz :)
 
Son bir not: AWK, Grep, Sed gibi süper Linux teknolojilerini Windows'ta da kullanmak isteyebilirsiniz. Bunun için <a href="http://www.cygwin.com/" target="_blank">Cygwin </a>'i kurmanız tavsiye edilir.