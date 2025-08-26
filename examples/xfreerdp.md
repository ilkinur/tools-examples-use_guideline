# 🖥️ xfreerdp3 Komandasının İzahı

```bash
xfreerdp3 /u:User /p:'Password' +clipboard /dynamic-resolution /cert:ignore /v:10.10.10.10 /drive:share,/opt/
```

Bu əmrlə **RDP (Remote Desktop Protocol)** vasitəsilə Windows serverə qoşuluruq.  
İstifadə olunan parametrlərin detalları aşağıdadır:

---

## 🔑 Əsas Parametrlər

- **`xfreerdp3`**  
  FreeRDP-in Linux üçün olan client-i. RDP bağlantısı üçün istifadə olunur.

- **`/u:User`**  
  Remote serverə qoşulmaq üçün istifadəçi adı (**User**).

- **`/p:'Password'`**  
  Remote serverin parolu (**Password**).

- **`/v:10.10.10.10`**  
  Qoşulacağımız serverin IP ünvanı (**10.10.10.10**).

---

## 📋 Əlavə Parametrlər

- **`+clipboard`**  
  Clipboard paylaşımını aktiv edir.  
  Yəni lokal və remote arasında copy/paste işləyəcək.

- **`/dynamic-resolution`**  
  Remote Desktop pəncərəsi ölçüsünə uyğun olaraq avtomatik ekran çözünürlüğünü tənzimləyir.  
  (Məsələn, pəncərəni böyütsən, remote ekran da böyüyür).

- **`/cert:ignore`**  
  Sertifikat yoxlamasını ləğv edir.  
  Adətən self-signed sertifikatlar olduqda "sertifikat xətası" almamaq üçün istifadə olunur.

- **`/drive:share,/opt/`**  
  Lokal qovluğu remote sistemə **network drive** kimi paylaşır.  
  - `share` → Remote sistemdə görünəcək ad.  
  - `/opt/` → Lokal maşında paylaşılacaq qovluğun yolu.  

  Yəni remote Windows-da "This PC" → `\\tsclient\share` olaraq açıb, həmin qovluğu görə biləcəksən.

---

