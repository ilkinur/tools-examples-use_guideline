# Volatility Framework – Geniş İstifadəçi Bələdçisi

## 1. Volatility nədir?

**Volatility Framework** RAM (memory) dump fayllarının analiz edilməsi üçün istifadə olunan açıq mənbəli **digital forensics** və **incident response** alətidir. Əsas məqsədi işləyən və ya artıq sönmüş sistemlərin yaddaşında olan **prosesləri, şəbəkə əlaqələrini, injected kodları, rootkit-ləri, credential-ları** və digər izləri aşkar etməkdir.

Volatility əsasən aşağıdakı sahələrdə istifadə olunur:

* 🕵️‍♂️ Incident Response
* 🔐 Malware Analysis
* 🧠 Memory Forensics
* 🚨 Threat Hunting
* ⚖️ Digital Forensics

---

## 2. Volatility versiyaları

| Versiya      | Status | Qısa izah                                    |
| ------------ | ------ | -------------------------------------------- |
| Volatility 2 | Legacy | Python 2 əsaslı, köhnə OS-lər üçün uyğundur  |
| Volatility 3 | Aktiv  | Python 3 əsaslı, modular, müasir OS-lər üçün |

> **Tövsiyə:** Yeni layihələr üçün **Volatility 3** istifadə edin

---

## 3. Dəstəklənən əməliyyat sistemləri

Volatility aşağıdakı OS-lərin memory dump-larını analiz edə bilir:

* Windows (XP → Windows 11)
* Linux
* macOS (məhdud dəstək)

---

## 4. Memory dump nədir və necə əldə olunur?

Memory dump – sistemin RAM məzmununun fayla yazılmış formasıdır.

### Windows üçün alətlər:

* DumpIt
* FTK Imager
* WinPmem

### Linux üçün:

* LiME
* /dev/mem (köhnə kernel-lər)

---

## 5. Volatility quraşdırılması

### Python mühiti

```bash
python3 -m venv venv
source venv/bin/activate
pip install volatility3
```

### GitHub-dan

```bash
git clone https://github.com/volatilityfoundation/volatility3.git
cd volatility3
python3 vol.py -h
```

---

## 6. Əsas anlayışlar

### Plugin nədir?

Volatility-də hər analiz modulu **plugin** adlanır.

Məsələn:

* pslist → prosesləri göstərir
* netscan → şəbəkə əlaqələri
* malfind → injected kod

---

## 7. Profil və simvollar (Volatility 3)

Volatility 3 avtomatik olaraq **symbols** istifadə edir.

* Windows: PDB simvollar
* Linux: vmlinux + System.map

---

## 8. Əsas istifadə sintaksisi

```bash
python3 vol.py -f memory.raw windows.pslist
```

| Parametr       | Açıqlama          |
| -------------- | ----------------- |
| -f             | Memory dump faylı |
| windows.pslist | Plugin            |

---

## 9. Ən çox istifadə olunan plugin-lər (Windows)

### 🔍 Proses analizi

#### pslist

Aktiv prosesləri göstərir

```bash
windows.pslist
```

#### psscan

Gizlədilmiş və terminated proseslər

```bash
windows.psscan
```

#### pstree

Proses ağacı

```bash
windows.pstree
```

---

### 🧬 Malware aşkarlanması

#### malfind

Injected və ya şübhəli kod

```bash
windows.malfind
```

#### dlllist

Yüklənmiş DLL-lər

```bash
windows.dlllist
```

#### handles

Proses handle-ları

```bash
windows.handles
```

---

### 🌐 Şəbəkə analizi

#### netscan

Aktiv TCP/UDP əlaqələri

```bash
windows.netscan
```

---

### 🔑 Credential və registry

#### hivelist

Registry hive-lar

```bash
windows.registry.hivelist
```

#### printkey

Registry key oxuma

```bash
windows.registry.printkey --key "HKLM\\Software"
```

#### hashdump

SAM hash-lar (admin icazəsi tələb edir)

```bash
windows.hashdump
```

---

## 10. Linux üçün əsas plugin-lər

* linux.pslist
* linux.psscan
* linux.netstat
* linux.check_afinfo
* linux.bash

```bash
linux.pslist
```

---

## 11. Forensic analiz ssenariləri

### Scenario 1 – Suspicious process

1. pslist
2. pstree
3. malfind
4. dlllist

### Scenario 2 – Backdoor aşkar edilməsi

1. netscan
2. pslist (PID uyğunluğu)
3. malfind

### Scenario 3 – Credential harvesting

1. lsass PID tap
2. malfind
3. dump memory segment

---

## 12. Output formatları

```bash
--output json
--output text
--output csv
```

```bash
windows.pslist --output json > pslist.json
```

---

## 13. Volatility + ELK / SIEM inteqrasiyası

* JSON output → Logstash
* IOC-lərin Kibana dashboard-da vizuallaşdırılması
* Threat Hunting üçün istifadə

---
