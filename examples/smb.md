# SMB examples

`smbclient -L 10.10.10.10 -N` - list with anonymous (-N)

`smbclient \\\\10.10.10.10\\{anything}` - open directory



get all files
```bash
smbclient '\\server\share'
mask ""
recurse ON
prompt OFF
cd 'path\to\remote\dir'
lcd '~/path/to/download/to/'
mget *
```

---

## SMB Relay Attack

### 📌 SMB nədir?

**SMB (Server Message Block)** – Windows əməliyyat sistemlərində fayl paylaşımı, printer çıxışı və digər resurslara şəbəkə üzərindən daxil olmaq üçün istifadə edilən bir protokoldur. Aşağıdakı portlar üzərindən işləyir:

- TCP 445 (ən çox istifadə olunan)
- TCP 139 (əvvəlki versiyalar)


### ⚠️ SMB Relay Attack necə işləyir?

SMB Relay Attack – **Man-in-the-Middle (MITM)** hücumu növüdür. Hücumçu, qurban sistemin göndərdiyi **NTLM autentifikasiya məlumatlarını ələ keçirir** və həmin məlumatları **başqa sistemlərə relay edir** (ötürür). Bu şəkildə qurbanın icazələri ilə başqa sistemlərə daxil ola bilir.

#### 🧱 Hücum Axını:

1. **MITM mövqeyi**: Hücumçu ARP Spoofing və ya DNS Spoofing ilə şəbəkədə ortada yerləşir.
2. **Autentifikasiya sorğusunu ələ keçirmək**: Qurban SMB servisinə qoşulmaq istədikdə NTLM paketi göndərir.
3. **Relay**: Hücumçu bu paketi başqa bir serverə relay edir.
4. **Access**: Server bu cavabı qəbul edir və hücumçu sistemə istifadəçi kimi daxil olur.


### 🎯 Təsir sahəsi

- Windows sistemləri
- NTLM autentifikasiya istifadə edən sistemlər
- SMBv1 istifadə olunan şəbəkələr
- Etibarsız şəbəkə dizaynları


### 💣 Məşhur alətlər

| Alət | Təyinat |
|------|---------|
| `Responder` | NTLM hash toplamaq, MITM qurmaq |
| `ntlmrelayx.py` | Relay hücumlarını həyata keçirmək üçün (Impacket) |
| `Metasploit` | SMB relay modulu ilə istifadə olunur |


### 🛡️ Müdafiə üsulları

1. **SMB Signing aktiv edin** – Paketlərin imzasını tələb edir, relay hücumlarını bloklayır.
2. **NTLM deaktiv edin** – Əvəzində Kerberos istifadə edin.
3. **Firewall qaydaları tətbiq edin** – Yalnız zəruri sistemlər arasında SMB açıq olsun.
4. **LDAP signing və channel binding** – LDAP relay hücumlarına qarşıdır.
5. **Şəbəkə monitorinqi** – Passiv hücum alətlərini aşkarlamaq üçün `IDS/IPS` və `SIEM` istifadə edin.


### 🧪 Praktik ssenari

1. Hücumçu şəbəkəyə `responder` aləti ilə yerləşir.
2. Qurban sistem SMB servisinə qoşulmaq istəyir.
3. NTLM challenge-response məlumatları `responder` tərəfindən ələ keçirilir.
4. Hücumçu `ntlmrelayx.py` ilə bu məlumatları başqa bir serverə relay edir.
5. Server bu məlumatları qəbul edir və hücumçu həmin istifadəçi hüquqları ilə sistemə daxil olur.

---

## 🔐 Cached Domain Logon Hash

**Cached Domain Logon Hash** — Windows əməliyyat sistemlərində domen istifadəçilərinin lokal olaraq saxlanan NTLM əsaslı autentifikasiya məlumatıdır. Bu mexanizm, istifadəçinin domen nəzarətçisinə (DC) qoşulu olmadığı hallarda belə sistemə daxil olmasına imkan verir.


### 🧠 Necə işləyir?

1. Domen istifadəçisi sistemə daxil olduqda, istifadə olunan NTLM hash lokal olaraq sistemdə saxlanılır.
2. Növbəti giriş zamanı əgər DC əlçatan deyilsə, Windows bu **cache-lənmiş hash** vasitəsilə istifadəçini təsdiqləyir.
3. Bu proses **"cached credentials"** adlanır və `MSCache` formatında saxlanılır.


### 🗂️ Harada saxlanılır?

- Fayl: `C:\Windows\System32\config\SECURITY`
- Registry yol: `HKLM\Security\Cache`
- Məlumatlar `SYSTEM` və `SECURITY` faylları ilə şifrələnmiş şəkildə saxlanır.


### ⚔️ Hücum baxımından riski

Əgər bir hücumçu sistemə **lokal administrator** səviyyəsində daxil olarsa, bu **cache-lənmiş hash-ları çıxarıb** digər sistemlərə daxil olmaq üçün istifadə edə bilər.

#### Hücum addımları:

1. Hücumçu sistemə daxil olur.
2. Cached hash-ları çıxarır (`Mimikatz`, `lsadump`, `secretsdump.py` ilə).
3. Bu hash ilə digər sistemlərə **"pass-the-hash"** texnikası ilə daxil olur.


### 🔧 Ən çox istifadə olunan alətlər

| Alət         | İstifadə məqsədi |
|--------------|------------------|
| `Mimikatz`   | Cached logon hash-ların çıxarılması |
| `lsadump`    | SYSTEM və SECURITY fayllarından hash çıxarma |
| `secretsdump.py` (Impacket) | Uzaqdan hash dump etmək |
| `psexec.py` / `wmiexec.py` | Pass-the-hash hücumları üçün |

---


