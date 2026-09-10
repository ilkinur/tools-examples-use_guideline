# iptables Əmrləri və Ətraflı İzahı

`iptables` — Linux əməliyyat sistemlərində şəbəkə trafikini filtrləmək, yönləndirmək və firewall (şəbəkə ekranı) qaydalarını idarə etmək üçün nəzərdə tutulmuş əmr sətri alətidir. Linux nüvəsindəki (**Netfilter**) modulu ilə qarşılıqlı əlaqədə işləyir.

---

## 1. Sintaksis və Əsas Bəndlər

`iptables` əmrinin ümumi strukturu aşağıdakı kimidir:

```bash
iptables [-t cədvəl] -ƏMR [ZƏNCİR] [şərtlər] -j [HƏDƏF]
```

* **`-t` (Table / Cədvəl):** `filter` (standart), `nat`, `mangle`, `raw`, `security`.
* **`-j` (Jump / Hədəf):** `ACCEPT`, `DROP`, `REJECT`, `LOG`, `MASQUERADE`, `DNAT`, `SNAT`.

---

## 2. Zəncirlərin İdarə Edilməsi Əmrləri (Chain Management)

Zəncir yaratmaq, silmək və ya mövcud qaydaları idarə etmək üçün istifadə olunan əsas parametrlər:

| Parametr | Adı / Mənası | Təsviri və Nümunə |
| :--- | :--- | :--- |
| **`-A`** | Append | Zəncirin **ən sonuna** yeni qayda əlavə edir.<br>`iptables -A INPUT -p tcp --dport 22 -j ACCEPT` |
| **`-I`** | Insert | Zəncirin **əvvəlinə** və ya göstərilən sıra nömrəsinə qayda əlavə edir.<br>`iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT` *(1-ci sıraya qoyur)* |
| **`-D`** | Delete | Qaydanı mövqeyinə (nömrəsinə) və ya məzmununa görə silir.<br>`iptables -D INPUT 3` *(INPUT-dakı 3-cü qaydanı silir)* |
| **`-R`** | Replace | Mövcud sıradakı qaydanı yenisi ilə əvəz edir.<br>`iptables -R INPUT 1 -s 192.168.1.5 -j DROP` |
| **`-L`** | List | Zəncirdəki qaydaları siyahılayır.<br>`iptables -L` |
| **`-F`** | Flush | Seçilmiş zəncirdəki (və ya bütün) qaydaları tamamilə silir.<br>`iptables -F INPUT` |
| **`-Z`** | Zero | Paket və bayt sayğaclarını sıfırlayır.<br>`iptables -Z` |
| **`-N`** | New Chain | İstifadəçinin özünəxas yeni zəncir yaradır.<br>`iptables -N MY_CHAIN` |
| **`-X`** | Delete Chain | İstifadəçi tərəfindən yaradılmış boş zənciri silir.<br>`iptables -X MY_CHAIN` |
| **`-P`** | Policy | Zəncirin standart davranış qaydasını (default policy) təyin edir.<br>`iptables -P INPUT DROP` *(Qaydaya düşməyən hər şeyi bloklayır)* |
| **`-E`** | Rename | İstifadəçi zəncirinin adını dəyişdirir.<br>`iptables -E MY_CHAIN NEW_CHAIN` |

---

## 3. Şərt Parametrləri (Match Criteria)

Paketlərin hansı xüsusiyyətə görə tutulacağını müəyyən edən parametrlər:

| Parametr | Adı / Mənası | Təsviri və Nümunə |
| :--- | :--- | :--- |
| **`-p`** | Protocol | Protokolu seçir (`tcp`, `udp`, `icmp`, `all`).<br>`iptables -A INPUT -p icmp -j DROP` |
| **`-s`** | Source | Mənbə IP ünvanını və ya şəbəkəni göstərir.<br>`iptables -A INPUT -s 10.0.0.5 -j DROP` |
| **`-d`** | Destination | Hədəf IP ünvanını göstərir.<br>`iptables -A OUTPUT -d 8.8.8.8 -j DROP` |
| **`-i`** | In-Interface | Trafikin daxil olduğu şəbəkə interfeysi (`eth0`, `wlan0`).<br>`iptables -A INPUT -i eth0 -p tcp --dport 80 -j ACCEPT` |
| **`-o`** | Out-Interface | Trafikin çıxdığı şəbəkə interfeysi (yalnız OUTPUT/FORWARD üçün).<br>`iptables -A OUTPUT -o eth0 -j ACCEPT` |
| **`--sport`** | Source Port | Mənbə portunu seçir (`-p` ilə istifadə olunur).<br>`iptables -A INPUT -p tcp --sport 53 -j ACCEPT` |
| **`--dport`** | Dest Port | Hədəf portunu seçir (`-p` ilə istifadə olunur).<br>`iptables -A INPUT -p tcp --dport 443 -j ACCEPT` |
| **`!`** | Not (İnkar) | Verilən şərtin əksini götürür.<br>`iptables -A INPUT -s ! 192.168.1.10 -j DROP` *(Bu IP-dən başqa hamısını bloklayır)* |

---

## 4. Siyahılama və Görünüş Bayraqları (Display Flags)

`-L` əmri ilə birgə istifadə olunan köməkçi bayraqlar:

* **`-v` (Verbose):** Paket sayğaclarını, bayt miktarını və interfeysləri ətraflı göstərir.
* **`-n` (Numeric):** IP və port adlarını mətne çevirmədən birbaşa rəqəmlərlə (çox daha sürətli) çıxarır.
* **`--line-numbers`:** Qaydaların qarşısında sıra nömrələrini göstərir (silmək üçün rahatdır).

**Nümunə:**
```bash
iptables -L -v -n --line-numbers
```

---

## 5. Genişləndirilmiş Modullar (`-m` / Match Modules)

Genişləndirilmiş funksiyalar üçün `-m` parametri ilə çağırılan əsas modullar:

### 5.1. Bağlantı vəziyyəti (`state`)
`NEW`, `ESTABLISHED`, `RELATED`, `INVALID`.
```bash
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
```

### 5.2. Çoxlu port seçimi (`multiport`)
```bash
iptables -A INPUT -p tcp -m multiport --dports 80,443,22 -j ACCEPT
```

### 5.3. IP aralığı (`iprange`)
```bash
iptables -A INPUT -m iprange --src-range 192.168.1.10-192.168.1.50 -j DROP
```

### 5.4. Paket tezliyini məhdudlaşdırma (`limit`)
```bash
iptables -A INPUT -p icmp -m limit --limit 1/s -j ACCEPT
```
*(Ping flood müdafiəsi)*

### 5.5. MAC ünvanı ilə bloklama (`mac`)
```bash
iptables -A INPUT -m mac --mac-source 00:0F:EA:91:04:08 -j DROP
```

---

## 6. NAT və Port Yönləndirmə (NAT Table)

Cədvəl olaraq `-t nat` göstərilməklə tətbiq olunur:

### 6.1. DNAT (Port Yönləndirmə)
```bash
iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80
```
*(8080 portuna gələn sorğunu daxili şəbəkədəki 192.168.1.100 IP-sinin 80-ci portuna yönləndirir).*

### 6.2. MASQUERADE (Dinamik IP üçün İnteqrasiya / Router rejimi)
```bash
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
*(Daxili şəbəkədən gələn paketləri xarici `eth0` interfeysinin IP-si ilə internetə çıxarır).*

### 6.3. SNAT (Statik Mənbə IP Dəyişməsi)
```bash
iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to-source 203.0.113.5
