# SSH
 

```bash
ssh user@<RHOST> -oKexAlgorithms=+diffie-hellman-group1-sha1
```
**İzah:** `ssh` bağlantısında zəif, köhnəlmiş `diffie-hellman-group1-sha1` dəyişmə alqoritmini (`KexAlgorithm`) istifadə etməyə icazə verir. Bu, köhnə sistemlərə bağlantı üçün lazım ola bilər.

---

```bash
ssh -R 8080:<LHOST>:80 <RHOST>
```
**İzah:** Remote port forwarding. Uzaq host (`<RHOST>`) öz 8080 portuna gələn trafiki `LHOST:80` ünvanına yönləndirəcək.

---

```bash
ssh -L 8000:127.0.0.1:8000 <USERNAME>@<RHOST>
```
**İzah:** Local port forwarding. Lokal maşında 8000 portuna gələn trafik `RHOST` hostundakı `127.0.0.1:8000` ünvanına ötürüləcək.

---

```bash
ssh -N -L 1234:127.0.0.1:1234 <USERNAME>@<RHOST>
```
**İzah:** `-N` opsiyası heç bir komanda icra etmədən bağlantı yaradacaq. 1234 port forwarding edilir. Faydalı tunel üçün.

---

```bash
ssh -L 80:<LHOST>:80 <RHOST>
```
**İzah:** Uzaq host (`<RHOST>`) üzərindən lokal hostdakı (`<LHOST>`) 80 portuna tunel yaradır.

---

```bash
ssh -L 127.0.0.1:80:<LHOST>:80 <RHOST>
```
**İzah:** Lokal maşının `127.0.0.1:80` ünvanına gələn bağlantılar `RHOST` üzərindən `LHOST:80` ünvanına yönləndiriləcək.

---

```bash
ssh -L 80:localhost:80 <RHOST>
```
**İzah:** Lokal maşının 80 portu, `RHOST` üzərindəki `localhost:80` ünvanına tunellənir.

---

```bash
ssh -D 9050 user@ip
```
Bu SOCKS Proxy (Dynamic port forwarding) açır.

-D 9050 → sənin öz kompüterində 9050 portunu dinləyir və onu qarşı tərəfə tunelləyir.

Bu halda lokal maşınında proxy server kimi işləyir. Məsələn, browser-i 127.0.0.1:9050 SOCKS proxy-ə qoşsan, bütün trafik əvvəlcə SSH ilə remote serverə gedəcək, ordan da internetə çıxacaq.

Yəni sanki VPN kimi işləyir, amma yalnız SOCKS səviyyəsində.

---

```bash
ssh user@ip -R 9050 -N
```
Bu isə Remote port forwarding edir.

-R 9050 → qarşı tərəfdə (remote serverdə) 9050 portunu dinləyir, və o porta gələn trafik sənin lokal maşına yönləndirilir.

-N → heç bir komanda shell açılmasın, sadəcə tunel yaradılsın.

Yəni fərq nədir?

-D SOCKS proxy lokalda açır.

-R isə portu remote serverdə açır, sənin lokalına forward edir.

🔑 Sadə müqayisə

ssh -D → mənim kompüterimdə proxy aç, bütün trafik remote serverdən çıxsın (VPN effekti).

ssh -R → remote serverdə port aç, ora gələnləri mənim kompüterimə yönləndir (reverse tunnel, yəni tərs istiqamətdə)
