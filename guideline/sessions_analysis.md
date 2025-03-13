# Sessiya Tokenlərinin Təhlili

## Tokenin Yoxlanması və Dəyişdirilməsi
- Tətbiqdən bir sessiya tokeni əldə edin və onu sistematik şəkildə dəyişdirin.
- Bütün tokenin yoxlanılıb-yoxlanılmadığını və ya müəyyən hissələrinin nəzərə alınıb-alınmadığını təyin etmək üçün testlər aparın.
- Tokenin hər bir baytını (və ya bitini) dəyişdirərək yenidən tətbiqə göndərin və qəbul edilib-edilmədiyini yoxlayın.
- Əgər tokenin müəyyən hissələri əhəmiyyətli deyilsə və dəyişdirildikdə qəbul olunursa, bu hissələri daha dərin təhlildən çıxara bilərsiniz.

## Müxtəlif İstifadəçi Girişlərinin Araşdırılması
- Fərqli zamanlarda müxtəlif istifadəçi hesabları ilə daxil olun və serverin göndərdiyi tokenləri qeyd edin.
- Əgər öz qeydiyyatınızı yarada bilirsinizsə, oxşar istifadəçi adlarından istifadə edin (məsələn, A, AA, AAA, AAAA, AAAB, AAAC, AABA və s.).
- İstifadəçi adı ilə yanaşı, elektron poçt ünvanı kimi digər istifadəçi məlumatlarını da dəyişdirərək sessiya tokenlərindəki dəyişiklikləri müşahidə edin.

## Tokenlərdəki Mümkün Korelyasiyaların Təhlili
- Tokenləri analiz edərək istifadəçi adı və digər dəyişdirilə bilən məlumatlarla əlaqəli hər hansı bir uyğunluq tapmağa çalışın.

## Tokenlərdə Kodlaşdırma və Gizlətmə Təhlili
- Tokenlərdə hər hansı kodlaşdırma və ya gizlətmə (obfuscation) mexanizmlərini yoxlayın.
- İstifadəçi adı eyni simvollardan ibarət olduqda, token daxilində oxşar simvol ardıcıllıqlarının olub-olmadığını araşdırın (bu, XOR gizlətməsindən istifadəni göstərə bilər).
- Yalnız onaltılıq (hex) simvolları ehtiva edən ardıcıllıqları yoxlayın (bu, ASCII sətirlərinin və ya digər məlumatların hex kodlaşdırıldığını göstərə bilər).
- Sonu "=" işarəsi ilə bitən və ya yalnız Base64 simvollarını ehtiva edən ardıcıllıqlara diqqət yetirin (a-z, A-Z, 0-9, +, /).

## Sessiya Tokenlərini Geri Mühəndislik Etmək
- Əgər sessiya tokenlərindən hər hansı bir mənalı məlumat əldə edə bilirsinizsə, bu məlumatlardan digər istifadəçilərə məxsus tokenləri təxmin etmək üçün istifadə edə bilərsiniz.
- Sessiya tələb edən bir səhifə tapın (məsələn, düzgün sessiya olmadıqda səhv mesajı və ya yönləndirmə qaytaran bir səhifə).
- Burp Intruder kimi bir alətdən istifadə edərək çoxlu sayda sorğu göndərin və ehtimal olunan tokenlərdən istifadə edin.
- Əgər səhifə uğurla yüklənirsə, bu, düzgün sessiya tokeni ilə giriş əldə etdiyinizi göstərə bilər.

