## **URL-Kodlaşdırma və Təhlükəsizlik**

Veb tətbiqlərinə hücum məqsədilə HTTP sorğusuna məlumat kimi əlavə etdiyiniz zaman aşağıdakı simvolları URL-kodlaşdırmalısınız:  
`space % ? & = ; + #`  

_(Əlbəttə, bu simvollardan bəzən xüsusi mənası ilə istifadə etmək lazım gəlir. Məsələn, sorğunun URL-ində əlavə parametr əlavə edərkən onlar olduğu kimi istifadə edilməlidir.)_

---

### **İzah**  
**URL-kodlaşdırma** (percent-encoding) veb tətbiqlərə hücumlar zamanı çox vacibdir. Çünki bəzi simvollar URL-də xüsusi məna daşıyır və səhv kodlaşdırılmadıqda təhlükəsizlik zəifliklərindən istifadə üçün qapı aça bilər.  

### **Niyə bu simvollar URL-kodlaşdırılmalıdır?**  
Bu simvollar HTTP sorğularında xüsusi rol oynayır:  

- **`space` (boşluq)** – URL-lərdə boşluqlar ola bilməz. Ona görə də `%20` və ya `+` kimi kodlaşdırılır.  
- **`%`** – URL kodlaşdırmanın özü `%` işarəsindən istifadə etdiyi üçün səhv başa düşülməməsi üçün kodlaşdırılmalıdır (`%25`).  
- **`?`** – URL-də sorğu parametrlərinin başlanğıcını göstərir, ona görə də məlumat kimi daxil edilirsə, kodlaşdırılmalıdır (`%3F`).  
- **`&`** – Sorğuda bir neçə parametri ayırmaq üçün istifadə edilir (`%26`).  
- **`=`** – Açar-dəyər cütlüklərində açarı dəyərdən ayırmaq üçün istifadə olunur (`%3D`).  
- **`;`** – URL sintaksisində bəzi sistemlərdə parametr ayırıcı kimi istifadə oluna bilər (`%3B`).  
- **`+`** – Boşluğu ifadə etmək üçün istifadə olunduğu hallarda problem yarada bilər (`%2B`).  
- **`#`** – URL-də "fragment" hissəsinin başlanğıcını göstərir, server tərəfindən emal olunmadığı üçün dəyər kimi istifadə ediləcəksə, kodlaşdırılmalıdır (`%23`).  

### **Praktiki Tətbiq və Hücum Senariləri**  

1. **SQL Injection qarşısını almaq üçün**  
   - `%` simvolu kodlaşdırılmasa, SQL ifadələrində dəyişiklik edilə bilər.  

2. **XSS qarşısını almaq üçün**  
   - `#`, `&`, `=` kimi simvollar URL parametrlərində zərərli skriptlərin icra olunmasına səbəb ola bilər.  

3. **Server-side request forgery (SSRF) qarşısını almaq üçün**  
   - `?`, `&`, `=` simvolları səhv işlənərsə, server istənməyən daxili sorğular göndərə bilər.  

**Nəticə:**  
URL-kodlaşdırmadan istifadə etməklə, tətbiqlərin bu tip hücumlardan qorunması təmin edilir.
