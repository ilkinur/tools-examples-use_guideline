# Radare2

### rabin2
Rabin2, Radare2’nin bir yardımcı aracıdır. Binary’den info vermektedir. 

`rabin2 -I file` - Binary info  
`rabin2 -z file` - bize data segmentindeki stringleri döküyor (-zz details)  

`radare2 (r2) file` - analiz edir. Sonra aa daha detalli meluamt ucun aaa veya aaaa yazila biler.  
`?` - help command  
`seek (s)` - memorydeki belirli bir adrese gidebiliyoruz. Yani bizim sıçrama komutumuz kısacası.  s--  komutu ile bir önceki adrese gidebiliriz. s- n komutu ile n adres undo. . s-- ve s++  1 adres geri qabaqa getmek ucun.  
`fs` - flanglari gosterir. Bir flagin içeriğini görüntülemek için ise `fs <flag adı>; f` komutu kullanılır.  
`iz (izz)` - tüm stringleri ekrana basacaktır  
`axt @@ str.*` - Sadece str. etiketine sahip tüm stringleri bastırar.  
`afl` - Analyze Functions List  
`pdf` - print disassemble functions   
`pd` - print disassemble  
`dr` - display register  hangi registerların olduğunu görmek için kullanıyoruz.   
`pxr @ <register adı>(<register adresi>)` - Registerların içeriğini ekrana basmak icin.  
`x/16x @ rip ` - komutu ile rip registerının ne kadarlık bir alanının gösterileceğini belirtir ve ekrana basarız. Daha fazla veri göstermek istiyorsak 16x size’ını daha da artırmalıyız.  

`/ fatih` -  komutu kodların içerisinde ‘fatih’ stringini arayacaktır ve bulduğu sonuçları bize adresleri ile dönecektir.  
`/x! 52` -  komutu bize binarydeki hex olarak 52 olan sonuçları ekrana basacaktır.  
`/a jmp eax` - komutu, kodu disassemble edecek ve sonra da** jmp eax**’ı bulup adresiyle birlikte.  
`/x 1234`- komutu, hex stringlerde 1234’ü arayacaktır ve adresini ekrana basacaktır.  
`/ca komutu` - eğer var ise memory’deki AES anahtarlarını bulur.  
`/cr komutu` - eğer var ise memory’deki** RSA Private Key**’leri bulur.  

### Debugging
`r2 -d dosya.exe`  
Debugger komutlarına `d?` komutu ile ulaşabilirsiniz.  
`db 0x25466d` - komutu ile belirtilen adrese breakpoint koyabiliriz.  
`db` - komutu ile var olan breakpointleri görebiliriz.  
`db- 0x25466d` - komutu ile belirtilen breakpointi silebiliriz.  
`db-*` - komutu ile var olan tüm breakpointleri kaldırabiliriz.  
`dbd 0x25566d` - var olan breakpointi kaldırmadan, devre dışı bırakabiliriz  
`dbe 0x25566d` - devre dışı olan breakpointi, etkin hale getirebiliriz.  
`dc` - programı direk çalıştırır. (Continue execution)  
`dcc` - programı call’a kadar çalıştırır. (Continue Until Call)  
`dcr` - rogramı return’a kadar çalıştırır. (Continue Until Return)  
`dcu main` - programı main’e kadar çalıştırır.  
`dcu 0x256674d` - programı belirtilen adrese kadar çalıştırır.  
`ds` - fonksiyonların içine girerek (girilebiliyorsa) bir defa ilerletir.(Step Into)  
`ds 10` - programı fonksiyon içine girerek 10 instruction daha çalıştırır.  
`dso` - fonksiyonların içine girmeden bir defa ilerletir.  
`dso 10` - programı fonksiyon içine girmeden 10 instruction daha çalıştırır.  
`ood` - programımızı debug modunda restart eder.  
`dr` - Breakpoint-də REGİSTER-lərə baxmaq.  
`ds` - Addım-addım icra (single-step). Call-ları içəri girmədən keçmək istəyirsənsə: `dso` . Bir neçə addım birdən: `ds 5`  



### Memory
`dm` - bize memory map’i gösterir.  
`dm=` - Ascii art bars şeklinde memory map’i ekrana basar.  
`dm.` - mevcutta bulunan adresin memory map adını ve adresini ekrana basar.  
`dm- 0x25884689` - belirtilen adresteki memory mapi deallocate eder.  
`dms` - memory snapshotlarını görüntüler.  
`dms <address>` - verilen adresin memory snapshot’ını alır.  
`dms-<id>` - ID’si girilen memory snapshot’ı siler.  
`dmsA <id>` -  ID’si girilen memory snapshot’ı uygular.  


### Binary Patching
`r2 -w dosya.exe` - ilk -w parametri ile baslatmaq lazimdi.  
Programımızı patchlemek için `vv` komutu sekmeli görünüm moduna alıyoruz.  
Aktif olarak bulunduğumuz satırın komutunu değiştirmek için büyük ‘A’ tuşuna basıyoruz. Ve istediğimiz assembly kodunu yazıp Enter’a basıyoruz.  


## Görsel Modu Kullanma
Farklı varyasyonları bulunmaktadır (V). Fakat bizim kullanacağımız ve en işlevli varyasyonu olan vv modunu kullanacağız. 
`VV` komutunu girererek konsol ekranı üzerinden görsel arayüze ulaşabilirsiniz. Görsel modun da kendi içinde farklı modları bulunmaktadır. `P` tuşuna basarak farklı modlara geçiş yapabilirsiniz. Ayrıca Mouse’u kullanabilirsiniz. Menülere Mouse ile tıklayabilir ve ok tuşları, enter ile seçiminizi yapabilirsiniz. Görsel modda bir çok özellik bulunmaktadır.  
`k` ve `j` tuşlarıyla da aşağı yukarı şekilde kodların içinde gezinebilirsiniz. Enter ile de odaklandığınız sekmeyi tam ekran yapabilirsiniz. Ayrıca `vv` komutu ile sekmeli görünüme geçiş yapabilirsiniz. `:` komutu ile görsel modda iken Radare2 komutlarını kullanabilirsiniz. Örneğin : s main şeklinde kullanımı mevcuttur.  
`;` komutu ile bulunduğunuz satıra yorum ekleyebilir veya var olan yorumu silebilirsiniz. `;` bu komut ile yorum ekleyebilirsiniz. `; -` bu komut ile yorumu silebilirsiniz.  
`+` ve `–` tuşlarına basarak zoom in veya zoom out yapabilirsiniz. Bu aynı zamanda IDA’daki graph view özelliğini kazandırmaktadır.  
`q` tuşu ile de Graph Mode’dan çıkış yapıp konsol moduna geri dönebilirsiniz.  




