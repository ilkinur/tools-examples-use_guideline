## **Brauzerin Konfiqurasiyası və Tətbiq Təhlili**

### **1. Proksini Qurmaq**  
- Brauzerinizi **Burp** və ya **WebScarab**-ı yerli proksi kimi istifadə edəcək şəkildə konfiqurasiya edin.  
- _(Əgər bunu necə edəcəyinizi bilmirsinizsə, xüsusi təfərrüatlar üçün 19-cu fəsilə baxın.)_  

### **2. Tətbiqi Ətraflı Təhlil Edin**  
- **Tam şəkildə gəzinin:**  
  - Bütün **linklərə/URL-lərə** daxil olun.  
  - Bütün **formaları göndərin**.  
  - **Çoxaddımlı funksiyaları** tamamlayın.  
- **Fərqli brauzer konfiqurasiyalarında test edin:**  
  - **JavaScript aktiv** və **deaktiv** edilmiş halda.  
  - **Kukilər aktiv** və **deaktiv** edilmiş halda.  
  - Bəzi tətbiqlər fərqli brauzer rejimlərini dəstəkləyir və bunun nəticəsində **fərqli məzmun və kod yollarına** daxil ola bilərsiniz.  

### **3. Sayt Xəritəsini Nəzərdən Keçirin**  
- **Proksi/spider aləti** tərəfindən yaradılan **sayt xəritəsini** analiz edin.  
- **Manuel baxmadığınız** hər hansı məzmun və ya funksiyaları müəyyənləşdirin.  
- Hər bir elementin **necə aşkar edildiyini** yoxlayın:  
  - **Burp Spider** istifadə edirsinizsə, **Linked From** detalına baxın.  
- Tapılan elementlərə **brauzerinizdən əl ilə daxil olun**.  
  - Bu, serverin cavabının **proksi/spider aləti** tərəfindən təhlil edilməsini və **əlavə məzmunun müəyyənləşdirilməsini** təmin edəcək.  
- Bu prosesi **təkrarlayın** (rekursiv şəkildə) ki, **daha çox məzmun və ya funksionallıq aşkar edilsin**.  

### **4. Avtomatlaşdırılmış Tarama (İstəyə Bağlı)**  
- **Tətbiqi aktiv şəkildə taramağa icazə verməzdən əvvəl:**  
  - **Təhlükəli və ya tətbiq sessiyasını poza biləcək URL-ləri müəyyənləşdirin.**  
  - Spider-in bu URL-ləri **tarama sahəsindən kənar tutmasını** təmin edin.  
- **Spider-i işə salın** və **aşkar etdiyi əlavə məzmunu nəzərdən keçirin**.  

### **5. Sayt Xəritəsinin Analizi**  
- **Proksi/spider aləti** tərəfindən yaradılan **sayt xəritəsi** hədəf tətbiq haqqında **dəyərli məlumatlar** təqdim edir.  
- Bu məlumatlar, sonradan tətbiqin **hücum səthini müəyyənləşdirmək** üçün istifadə edilə bilər.

## Veb Tətbiqinin Təhlükəsizlik Analizi

### Giriş Nöqtələrinin Təhlili
- İstifadəçi giriş nöqtələrini müəyyən edin, o cümlədən:
  - URL-lər
  - Sorğu sətiri (query string) parametrləri
  - POST məlumatları
  - Kukilər (cookies)
  - Digər HTTP başlıqları (headers)

### URL və Sorğu Sətirinin Formatının Təhlili
- Tətbiqin istifadə etdiyi sorğu sətiri formatını yoxlayın.
- Standart formatdan fərqli bir format istifadə olunursa, parametrlərin URL daxilində necə ötürüldüyünü müəyyən etməyə çalışın.
- Xüsusi formatlar adətən ad/dəyər (name/value) modelindən istifadə edirlər, ona görə də bu formatın necə tətbiq olunduğunu başa düşməyə çalışın.

### Kənar Məlumat Kanallarının Aşkarlanması
- Tətbiqin emal prosesinə istifadəçi tərəfindən idarə edilə bilən və ya üçüncü tərəfdən daxil edilən məlumatların hansı kanallar vasitəsilə gətirildiyini təyin edin.

### HTTP Server Banner-in Yoxlanması
- HTTP server banner-inə baxın.
- Bəzi hallarda tətbiqin müxtəlif bölmələri fərqli arxa hissə komponentləri tərəfindən idarə olunduğuna görə müxtəlif "Server" başlıqları alına bilər.

### Xüsusi HTTP Başlıqları və HTML Mənbə Kodundakı İdentifikatorlar
- İstənilən xüsusi HTTP başlıqlarını və HTML mənbə kodundakı proqram təminatı identifikatorlarını yoxlayın.

### Veb Serverin Fingerprint Analizi
- Httprint alətindən istifadə edərək veb serverin fingerprint analizini aparın.

### Server və Komponentlərin Versiyalarının Təhlili
- Əgər server və digər komponentlər haqqında detallı məlumat əldə edilərsə, istifadə olunan proqram təminatının versiyalarını araşdırın.
- Məlum zəifliklərə qarşı həssas olub-olmadığını təyin etmək üçün bu versiyaların zəiflik bazalarında (CVE, NVD və s.) olub-olmadığını yoxlayın.

### Tətbiqin URL Xəritəsinin Araşdırılması
- Fərqli fayl uzantıları, qovluqlar və digər maraqlı elementlər müəyyən olunmalıdır.
- Sessiya identifikatorları və onların strukturları analiz olunmalıdır ki, istifadə edilən texnologiyalar müəyyən edilsin.

### Texnologiyaların Tanınması
- İstifadə edilən texnologiyaları müəyyən etmək üçün ümumi texnologiya siyahılarından və Google axtarışlarından istifadə edin.
- Eyni texnologiyaları istifadə edən digər veb saytları və tətbiqləri tapın.

### Üçüncü Tərəf Komponentlərinin Təhlili
- Qeyri-adi kukilər, skriptlər, HTTP başlıqları və digər komponentlər üçün Google-da axtarış aparın.
- Eyni komponentləri istifadə edən digər tətbiqləri araşdıraraq onların əlavə funksionallıqlarını müəyyən edin.
- Əgər mümkündürsə, komponentin bir nüsxəsini yükləyib quraşdıraraq onun funksionallığını tam təhlil edin və zəifliklərini araşdırın.
- Zəifliklər bazalarında (CVE, Exploit-DB və s.) həmin komponentin məlum zəifliklərinin olub-olmadığını yoxlayın.

### Parametrlərin və Onların Dəyərlərinin Təhlili
- Tətbiqə təqdim edilən bütün parametrlərin adlarını və dəyərlərini onların dəstəklədiyi funksionallıq kontekstində nəzərdən keçirin.
- Proqramçı kimi düşünməyə çalışın və müşahidə edilən davranışın arxasında hansı server tərəfli mexanizmlər və texnologiyaların dayandığını təsəvvür etməyə çalışın.

