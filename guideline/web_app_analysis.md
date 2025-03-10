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
