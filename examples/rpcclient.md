# 🛠️ rpcclient İstifadə

## 📌 Nədir `rpcclient`?

`rpcclient` — Samba paketinə daxil olan CLI (command-line interface) alətidir və Windows sistemlərində **RPC (Remote Procedure Call)** vasitəsilə uzaqdan məlumat toplamaq və idarəetmə əməliyyatları aparmaq üçün istifadə olunur.

---

## 🎯 Əsas Məqsədlər

* Windows sistemlərindən məlumat toplamaq
* Domain və user enumeration
* SID → Username çevrilməsi
* Password policy yoxlama
* Share və group məlumatları əldə etmək

---

## ⚙️ Qurulma

```bash
sudo apt install samba-common-bin
```

---

## 🔐 Authentication (Giriş üsulları)

### 1. Username + Password

```bash
rpcclient -U username target_ip
```

### 2. Anonymous (Null session)

```bash
rpcclient -U "" target_ip
```

### 3. NTLM Hash ilə

```bash
rpcclient -U username%LMHASH:NTHASH target_ip
```

---

## 📡 Əsas Komandalar

### 📁 Server məlumatı

```bash
srvinfo
```

### 👤 İstifadəçilərin siyahısı

```bash
enumdomusers
```

### 👥 Qrupların siyahısı

```bash
enumdomgroups
```

### 🧾 Domain məlumatı

```bash
querydominfo
```

### 🔑 Password policy

```bash
getdompwinfo
```

---

## 🔎 SID ilə İşləmə

### SID əldə etmək

```bash
lsaquery
```

### SID → Username

```bash
lookupsids S-1-5-21-XXXX
```

### Username → SID

```bash
lookupnames username
```

---

## 📂 Shares və Resurslar

```bash
netshareenum
```

---

## 🧠 Advanced Enumeration

### User haqqında detallı info

```bash
queryuser RID
```

### Group haqqında info

```bash
querygroup RID
```

### Group üzvləri

```bash
querygroupmem RID
```

---


## 🧪 Praktik Nümunə

```bash
rpcclient -U "" 192.168.1.10
```

Daha sonra:

```bash
enumdomusers
querydominfo
```


