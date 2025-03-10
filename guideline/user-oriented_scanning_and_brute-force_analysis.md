## **İstifadəçi Yönümlü Tarama və Güc-Yoxlama Təhlili**

### **1. Tarama Nəticələrinin Təhlili**
- İstifadəçi yönümlü tarama və ilkin güc-yoxlama testlərinin nəticələrini nəzərdən keçirin.
- Aşağıdakı siyahıları tərtib edin:
  - **Aşkar edilən alt kataloqların adları**
  - **Fayl adlarının əsas hissələri (file stems)**
  - **Fayl uzantıları (extensions)**

### **2. Adlandırma Sxemlərinin Təhlili**
- Siyahıları nəzərdən keçirin və tətbiqdə istifadə olunan **adlandırma sxemlərini** müəyyən edin.
- Məsələn, əgər **AddDocument.jsp** və **ViewDocument.jsp** varsa, ola bilər ki, **EditDocument.jsp** və **RemoveDocument.jsp** də mövcuddur.
- Tərtibatçının adlandırma tərzini təyin etməyə çalışın:
  - **Detallı:** AddANewUser.asp
  - **Qısa:** AddUser.asp
  - **Abreviatura ilə:** AddUsr.asp
  - **Şifrəli:** AddU.asp
- Bu tərzlərdən istifadə edərək **hələ aşkar edilməmiş məzmunları təxmin edin**.

### **3. Rəqəmlər və Tarixlərə Əsaslanan Adlandırma**
- Bəzi adlandırma sxemləri **rəqəmlər və tarixlərdən** istifadə edə bilər.
- Məsələn, bir şirkətin veb saytında **AnnualReport2004.pdf** və **AnnualReport2005.pdf** varsa, növbəti hesabatın adını təxmin etmək asan ola bilər.
- Bəzi hallarda şirkətlər **maliyyə nəticələrini ictimaiyyətə açıqlamadan əvvəl** veb serverə yerləşdirirlər və ağıllı jurnalistlər bunları aşkar edə bilirlər.

### **4. Müştəri Tərəfli Kodların Analizi**
- **HTML və JavaScript kodlarını** nəzərdən keçirin və gizli server məzmununa dair ipuçları axtarın.
- Bunlara aşağıdakılar daxildir:
  - **Qorunan və ya əlaqələndirilməmiş funksiyalar haqqında HTML şərhləri**
  - **SUBMIT düyməsi deaktiv edilmiş HTML formaları**
  - **Server-side include (SSI) fayllarına istinadlar**
- Bu fayllar **açıq yüklənə bilən** ola bilər və **məlumat bazası bağlantı sətirləri və şifrələr** kimi həssas məlumatlar ehtiva edə bilər.
- Bəzən tərtibatçılar **fayllarda verilənlər bazası adları, arxa sistem komponentlərinə istinadlar və SQL sorğularını** şərh şəklində saxlayırlar.
- **Java appletlər və ActiveX kontrolleri** də həssas məlumatları ehtiva edə bilər.

### **5. Siyahıları Genişləndirmək və Təxminlər Əsasında Yeniləmək**
- Tapılmış məzmunlara əsaslanaraq **potensial adlar üzrə yeni siyahılar** yaradın.
- **Aşağıdakı fayl uzantılarını** da əlavə edin:
  - **txt, bak, src, inc, old** (backup və kod versiyalarını aşkar etmək üçün)
  - **Java, cs və digər inkişaf dillərinə aid uzantılar** (mənbə kodlarının yüklənə biləcəyini yoxlamaq üçün)
- **Paros aləti** bu testi avtomatik olaraq həyata keçirir.

### **6. Müvəqqəti Faylların Aşkar Edilməsi**
- **İnkişaf alətləri və mətn redaktorları tərəfindən avtomatik yaradılmış müvəqqəti faylları** axtarın.
- Bunlara aşağıdakılar daxildir:
  - **.DS_Store** (OSX sistemlərində qovluq indeks faylı)
  - **file.php~1** (file.php redaktə edilərkən yaradılan müvəqqəti fayl)

### **7. Avtomatlaşdırılmış Aşkaretmə Testləri**
- **Kataloqlar, fayl adları və uzantılar siyahılarını birləşdirərək avtomatik testlər aparın.**
- Məsələn, verilmiş bir qovluqda:
  - **Hər bir fayl adını hər bir uzantı ilə birləşdirərək sorğular göndərin.**
  - **Hər bir kataloqu hər bir məlum kataloqun alt qovluğu kimi test edin.**

### **8. Fokuslanmış Güc-Yoxlama Testləri**
- **Müəyyən bir adlandırma sxemi aşkar edilərsə, onu əsas götürərək daha dəqiq güc-yoxlama testləri aparın.**
- Məsələn, **AddDocument.jsp** və **ViewDocument.jsp** aşkar edilibsə:
  - **edit, delete, create** kimi hərəkətləri siyahıya əlavə edin və **XxxDocument.jsp** formasında sorğular göndərin.
  - **user, account, file** kimi obyekt adlarını siyahıya əlavə edərək **AddXxx.jsp** formatında sorğular göndərin.

### **9. Rekursiv Aşkaretmə Testləri**
- Hər bir tapıntını **rekursiv şəkildə** istifadə edərək **yeni tarama və avtomatik aşkaretmə testləri** aparın.
- **Tətbiq haqqında daha çox məlumat əldə etmək üçün:**
  - **Yeni aşkar edilmiş məzmunları analiz edin.**
  - **Yeni nümunələr əsasında əlavə təxminlər aparın.**
- **Tək məhdudiyyət:** sizin **təsəvvürünüz, vaxtınız və tətbiq haqqında daha çox məlumat əldə etməyə verdiyiniz əhəmiyyətdir.**

