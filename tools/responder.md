# Responder

**Responder**, əsasən **yerli şəbəkələrdə (LAN)** istifadə edilən və **Man-in-the-Middle (MitM)** hücumlarını həyata keçirmək üçün nəzərdə tutulmuş bir alətdir. Bu alət vasitəsilə istifadəçi **NTLM hash-ları**, **şifrlənmiş parollar**, və digər **şəbəkə autentifikasiya məlumatları** ələ keçirilə bilər.

---

## 🎯 Əsas Məqsədləri

- **LLMNR (Link-Local Multicast Name Resolution)**, **NetBIOS** və **NBNS** sorğularını spoof etmək.
- Windows sistemlərində DNS uğursuzluqlarını istismar edərək saxta cavablar göndərmək.
- **NTLMv1/v2 hash-larını** ələ keçirmək.
- **HTTP, SMB, FTP, LDAP** kimi saxta xidmətlər qurmaq və credential-ları toplamaq.

---

## ⚙️ Tipik İstifadə Ssenarisi

```bash
sudo responder -I eth0
