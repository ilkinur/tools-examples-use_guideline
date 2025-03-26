# XSS (Cross-Site Scripting) Açıqları

## XSS nədir?
XSS (Cross-Site Scripting) hücumu veb tətbiqlərdə təhlükəsizlik zəifliyindən istifadə edərək, zərərli skriptlərin digər istifadəçilərin brauzerlərində icra olunmasına səbəb olan bir kiberhücum növüdür. Bu, əsasən, istifadəçi tərəfindən daxil edilən məlumatların lazımi şəkildə yoxlanılmaması və təhlükəsiz şəkildə emal edilməməsi nəticəsində baş verir.

## XSS növləri

### 1. Reflected XSS (Əks olunmuş XSS)
Bu tip XSS hücumunda zərərli skript, serverə göndərilən sorğunun cavabında dərhal istifadəçiyə qaytarılır. Bu, əsasən URL parametrində və ya POST sorğusunda olur.

**Nümunə:**
```
http://example.com/search?q=<script>alert('XSS')</script>
```
Bu halda, əgər server istifadəçi girişini təmizləmədən səhifədə göstərirsə, skript icra olunacaq.

### 2. Stored XSS (Saxlanılan XSS)
Stored XSS hücumu zamanı zərərli skript serverdə saxlanılır və sonradan digər istifadəçilərə təqdim edilir. Bu daha təhlükəli sayılır.

**Nümunə:**
Bir istifadəçi şərh bölməsinə aşağıdakı kodu yazarsa və server bunu filtrləməzsə:
```html
<script>alert('XSS Hücumu!')</script>
```
Bu şərh verilənlər bazasında saxlanacaq və digər istifadəçilər səhifəyə daxil olduqda skript icra olunacaq.

### 3. DOM-Based XSS (DOM-əsaslı XSS)
DOM-Based XSS hücumu istifadəçi brauzerində DOM manipulyasiyası vasitəsilə icra olunur. Bu hücum serverə sorğu göndərmədən, yalnız səhifədəki JavaScript kodları ilə həyata keçirilir. Hücum, istifadəçi tərəfindən daxil edilən məlumatların səhifənin DOM strukturuna təhlükəsiz şəkildə əlavə olunmaması nəticəsində baş verir.

#### **DOM-Based XSS necə işləyir?**

1. Hücumçu veb səhifənin URL parametrinə və ya DOM daxilində bir sahəyə zərərli kod yerləşdirir.
2. JavaScript bu parametri DOM-ə daxil edərkən skript icra olunur.
3. Serverə heç bir sorğu göndərilmədiyi üçün təhlükəsizlik skanerləri bu hücumu aşkarlamaq çətin olur.

#### **DOM-Based XSS nümunəsi**
Aşağıdakı nümunədə istifadəçi giriş məlumatları təhlükəsiz şəkildə işlənmədiyindən DOM-Based XSS baş verə bilər:

```html
<!DOCTYPE html>
<html>
<head>
    <title>DOM-Based XSS</title>
</head>
<body>
    <p>İstifadəçi məlumatı:</p>
    <div id="output"></div>
    <script>
        var userInput = window.location.hash.substring(1);
        document.getElementById("output").innerHTML = userInput;
    </script>
</body>
</html>
```

Əgər istifadəçi aşağıdakı URL-i brauzerə daxil edərsə:
```
http://example.com/#<script>alert('XSS')</script>
```
Səhifə `#` işarəsindən sonrakı hissəni DOM-a birbaşa daxil etdiyi üçün `<script>` etiketi icra olunacaq və XSS hücumu baş verəcək.


## XSS-dən qorunma üsulları

1. **Giriş məlumatlarını təhlükəsiz şəkildə emal etmək:**
   - `htmlspecialchars()`, `htmlentities()` kimi PHP funksiyalarından istifadə edin.
   - JavaScript-də `innerHTML` yerinə `textContent` istifadə edin.

2. **CSP (Content Security Policy) tətbiq etmək:**
   - Saytda inline skriptləri bloklamaq üçün `Content-Security-Policy` başlığından istifadə edin.

3. **Girişlərin filtr edilməsi və doğrulanması:**
   - İstifadəçilərdən daxil olan məlumatları ciddi şəkildə yoxlayın.
   - Yalnız gözlənilən formatdakı məlumatlara icazə verin.

4. **HTTP-Only və Secure Cookie-lərdən istifadə etmək:**
   - `HttpOnly` flag-i ilə cookie-ləri JavaScript vasitəsilə oxumaq mümkün olmasın.

5. **Kodu audit etmək və təhlükəsizlik testləri aparmaq:**
   - XSS hücumlarına qarşı testlər aparın və avtomatlaşdırılmış təhlükəsizlik skanerlərindən istifadə edin.

