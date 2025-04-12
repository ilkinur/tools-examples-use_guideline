# Chisel və ProxyChains ilə Şəbəkə Tunelləmə

## Chisel-in Xüsusiyyətləri
- TCP və UDP portları tunelləyə bilir.
- NAT bypass dəstəkləyir.
- TLS şifrələməsi dəstəklənir.
- Linux, Windows və MacOS-da işləyir.
- HTTP və HTTPS üzərindən əlaqə yarada bilir.

---


## Chisel ilə Şəbəkə Tunelləmə

Chisel istifadə edərək tunel qurmaq üçün bir server və bir müştəri konfiqurasiya etməliyik.

### 1. Serveri İşə Salmaq
Chisel serveri açıq bir port üzərində işlədilir və müştərilərin qoşulmasına icazə verir.

```bash
chisel server --reverse --port 8080
```
Bu əmrlə:
- `--reverse` - Müştərilərin tunel yaratmasına icazə verir.
- `--port 8080` - Serverin 8080 portunda işləməsini təmin edir.

### 2. Müştərinin Qoşulması
Müştəri tərəfi serverə qoşularaq tunel yaradır:

```bash
chisel client <server_ip>:8080 R:1080:socks
```
Burada:
- `<server_ip>` - Serverin IP ünvanıdır.
- `R:1080:socks` - Müştəri 1080 portunda socks proxy yaratmış olur.

---

## ProxyChains və Chisel ilə Komanda İşlətmək

Chisel ilə yaradılan SOCKS5 proxy-nin ProxyChains ilə istifadəsi mümkündür.


### ProxyChains Konfiqurasiyası
Faylı açın və aşağıdakı düzəlişi edin:
```bash
nano /etc/proxychains.conf
```
Aşağıdakı sətri faylın sonuna əlavə edin:
```bash
socks5 127.0.0.1 1080
```

### ProxyChains ilə Şəbəkədə Komanda İşlətmək
ProxyChains vasitəsilə tunel üzərindən komanda işlətmək üçün:
```bash
proxychains -q nmap -sT -Pn -p 22 <target_ip>
```
Bu əmr `target_ip` ünvanında olan 22 (SSH) portunu skan edir.

Başqa bir nümunə:
```bash
proxychains -q ssh user@target_ip
```
Bu isə tunel üzərindən `target_ip`-yə SSH ilə qoşulmanı təmin edir.

---------------------------------------------  

```bash
chisel client YOUR_SERVER_IP:8080 R:2222:localhost:22
```

YOUR_SERVER_IP — VPS-in IP ünvanı.

`R:2222:localhost:22` — serverdə (VPS-də) 2222 portu açılır və ora gələn trafik clientdəki `localhost:22` portuna yönlənir.
