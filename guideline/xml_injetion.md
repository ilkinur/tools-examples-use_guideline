# XML Injection

## 1. XML Injection Nədir?
XML Injection, tətbiqin XML mesajlarını təhlükəsiz şəkildə emal etməməsi nəticəsində baş verən hücum növüdür. Bu hücum zamanı hücumçu tətbiqə xüsusi XML kodları daxil edərək onun işləmə məntiqini dəyişdirə və ya məxfi məlumatlara çıxış əldə edə bilər.

## 2. XML Injection Zəifliyini Aşkar Etmək

### 2.1. Yanlış XML Bağlanma Etiketlərinin Göndərilməsi
Tətbiqə hər parametrlə fərdi şəkildə aşağıdakı XML bağlanma etiketini göndər:
```xml
</foo>
```
- Əgər heç bir xəta yaranmırsa, bu o deməkdir ki, girişlər SOAP mesajına daxil edilmir və ya tətbiq xüsusi təmizləmə (sanitization) prosesindən keçir.
- Əgər xəta alınarsa, tətbiq XML mesajlarını emal edərkən zəiflik ola bilər.

### 2.2. Düzgün XML Etiket Cütlüyünün Göndərilməsi
Bu dəfə tətbiqə aşağıdakı açıq və bağlanmış XML etiketlərini daxil et:
```xml
<foo></foo>
```
- Əgər bu girişdən sonra əvvəlki xəta aradan qalxırsa, bu tətbiqin XML Injection-a qarşı həssas ola biləcəyini göstərir.

### 2.3. XML Məlumatının Geri Oxunması və Dəyişdirilməsi
Bəzi hallarda, XML formatlı məlumat tətbiq tərəfindən istifadə edilərək cavabda geri qaytarıla bilər. Bu halda aşağıdakı iki girişdən birini göndər və nəticələri müşahidə et:
```xml
test<foo/>
test<foo></foo>
```
- Əgər bu girişlərdən biri digəri kimi və ya sadəcə `test` kimi qaytarılırsa, bu, tətbiqin XML strukturlarını dəyişdirdiyini və XML Injection üçün həssas ola biləcəyini göstərir.

### 2.4. SOAP Mesajlarını Şərh Etmə (Commenting Out)
Əgər HTTP sorğusunda bir neçə parametr varsa və onların SOAP mesajına yerləşdirildiyini düşünürsənsə, aşağıdakı test üsulunu sına:
1. Birinci parametrə açıq şərh simvolunu (`<!--`) daxil et.
2. İkinci parametrə bağlanmış şərh simvolunu (`-->`) daxil et.
3. Bu parametrləri tərsinə dəyişərək test et.

- Bu üsul serverin SOAP mesajının bir hissəsini şərhə çevirə bilər və tətbiqin məntiqində dəyişikliklərə səbəb ola bilər.
- Əgər tətbiq fərqli bir xəta qaytarırsa, bu, XML Injection zəifliyinin ola biləcəyini göstərə bilər.

