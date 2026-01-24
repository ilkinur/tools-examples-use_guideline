# WireShark

### FTP 

Əsas FTP protokol filtri
```
ftp
```

FTP Control Port (21) filtri
```
tcp.port == 21
```

FTP USER əmri (istifadəçi adları)
```
ftp.request.command == "USER"
```

FTP PASS əmri (şifrələr ⚠️)
```
ftp.request.command == "PASS"
```

Uğurlu loginlər
```
ftp.response.code == 230
```

Uğursuz loginlər
```
ftp.response.code == 530
```

Fayl yükləmə (Download)
```
ftp.request.command == "RETR"
```

Fayl yükləmə (Upload)
```
ftp.request.command == "STOR"
```

Fayl siyahısı (Directory listing)
```
ftp.request.command == "LIST"
ftp.request.command == "NLST"
```

FTP Passive Mode
```
ftp.request.command == "PASV"
```

FTP Active Mode
```
ftp.request.command == "PORT"
```

FTP response code analizi

Kod | Mənası
---|---
230 | Login OK
530	| Login failed
331	| Password gözlənilir
150	| Data transfer başlayır
226	| Transfer tamamlandı

Şübhəli fayl adları:
```
ftp.request.arg contains ".php"
```

-------




