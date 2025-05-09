# Metasploit’i Kavrama

| Komutlar        | Açıklama                                                                 |
|--------------------|--------------------------------------------------------------------------|
| `msfconsole`       | Metasploit’i başlatır.                                                   |
| `msfdb run`        | Metasploit veritabanını başlatır.                                       |
| `msfdb init`       | Metasploit için veritabanını ayarlar. *(İlk kullanım için, tek seferlik)*|
| `db_status`        | Metasploit veritabanı durumunu kontrol eder.                            |
| `search`           | Modül aramak için kullanılır. *Detay için `-h` kullanınız.*              |
| `info` / `advanced`| Modül hakkında kısa veya geniş bilgi verirler.                           |
| `show`             | Modülleri ve modül içindeki ayarları listelemek için kullanılır.        |
| `use`              | Modülü kullanmak için kullanılır.                                        |
| `options`          | Modül ayarlarını gösterir.                                               |
| `set`              | Modül ayarlarını yapar.                                                  |
| `check`            | İlgili modül aracılığıyla hedefte ön kontrol gerçekleştirir.            |
| `run` / `exploit`  | Modülü veya verilen işlemi çalıştırır.                                  |
| `sessions`         | Alınan shell’leri görüntüler. Shell’ler arası geçiş sağlar.              |
| `jobs`             | Arkaplanda çalışan modül işlemlerini görüntüler.                         |
| `back`             | Seçili modülden geri çıkılmasını sağlar.                                |
| `exit`             | Metasploit’in kapatılmasını sağlar.                                     |

---

# Meterpreter və Sistem Komutları

## Sistem və Şəbəkə Komutları

| Komutlar                    | Açıklama                                                                 |
|-----------------------------|--------------------------------------------------------------------------|
| `sysinfo`                   | Sistem bilgilerini görüntüler.                                          |
| `show_mount`                | Bağlı diskleri (partition’ları) görüntüler.                             |
| `idletime`, `localtime`     | Sistemin boşa olduğu süreyi və local zamanı görüntüler.                 |
| `shutdown`, `reboot`        | Sistemi kapatır veya yeniden başlatır.                                  |
| `ipconfig`                  | Sistemin IP yapılandırmasını görüntüler.                                |
| `portfwd`                   | Paket yönlendirmesi yapar.                                              |
| `route`                     | Rotaların görüntülenmesini və dəyişdirilməsini sağlar.                  |
| `ps`                        | Çalışan işlemleri görüntüler.                                           |
| `kill <PID>`                | İlgili PID’ye ait işlemin sonlandırılmasını sağlar.                     |
| `getpid`                    | Oturumun hangi PID’de çalıştığını görüntüler.                           |
| `migrate <PID>`             | İşlemleri birbirlerine taşır. İlgili işlemi başka işlem arkasında çalıştırır. |
| `clearev`                   | Olay günlüğünün silinmesini sağlar.                                    |
| `shell`                     | İlgili sistemin shell’ine geçiş yapar.                                 |

---

## Dosya Sistemi və Yardımcı Komutlar

| Komutlar                     | Açıklama                                                                 |
|------------------------------|--------------------------------------------------------------------------|
| `help`, `?`                  | Yardım alımını sağlar.                                                  |
| `pwd`                        | Bulunulan dizini görüntüler.                                            |
| `ls`, `ls <dizin>`           | Bulunulan veya belirtilen dizinin içeriğini listeler.                   |
| `cd "c:\Program Files"`, `lcd`| Dizin değiştirilmesini sağlar.                                         |
| `cat <dosya>`                | İlgili dosyanın okunmasını sağlar.                                     |
| `edit <dosya>`               | İlgili dosyanın düzenlenmesini sağlar.                                 |
| `del <dosya>`, `rm <dosya>`  | İlgili dosyanın silinmesini sağlar.                                    |
| `mkdir <dizin>`, `rmdir <dizin>` | Dizin oluşturulmasını və dizinin silinmesini sağlar.              |
| `execute -f <dosya>`         | İlgili dosyanın çalıştırılmasını sağlar. (-f dosya, -H gizle, -i çalıştır və çağır) |
| `search -f <dosya>`          | İlgili dosyanın aranmasını sağlar.                                     |
| `download <neyi> <nereye>`   | İlgili dosyanın indirilmesini sağlar.                                  |
| `upload <neyi> <nereye>`     | İlgili dosyanın gönderilmesini sağlar.                                 |
| `background`, `bg`           | Meterpreter oturumunu Metasploit’in arka planına atar.                 |
| `sessions -i <ID>`           | Arka planda olan, ilgili ID’li Meterpreter’i öne getirir.              |
| `bgrun`, `bglist`, `bgkill`  | Arkaplanda Meterpreter işlemi yürütür, listeler və sonlandırır.        |

---

## Yetki, Keylogger, Webcam və Script Komutları

| Komutlar                             | Açıklama                                                                 |
|--------------------------------------|--------------------------------------------------------------------------|
| `getuid`                             | Bağlı oturumun (kullanıcının) yetki sınıfını görüntüler.                |
| `getprivs`                           | Bağlı oturumda yetkilendirilmiş işlemleri görüntüler.                   |
| `getsystem -t 0`                     | Bağlı kullanıcının sistem kullanıcısı yetkilerine yükseltilmesini dener.|
| `reg <komut>`                        | Hedef sistemin Registry’sini yönetir. (`reg -h` ile yardım alınabilir.) |
| `hashdump`                           | Sistem üzerindeki şifre hash’lerini görüntüler.                         |
| `webcam_list`                        | Sistem üzerindeki webcam’leri listeler.                                 |
| `webcam_snap`                        | Hedefteki webcam görüntüsünü alır.                                      |
| `screenshot`                         | Hedefin ekran görüntüsünü alır. (espia eklentisinin çalışırlığını gerektirir.) |
| `keyscan_start`, `keyscan_stop`     | Hedefte tuş kaydedici (keylogger) çalıştırır ve durdurur. (`migrate` gerekir.) |
| `keyscan_dump`                       | Tuş kaydedici (keylogger) kayıtlarını çeker. (Stop edilmeden önce çalıştırılmalı.) |
| `irb`                                | Metasploit scriptleri oluşturup çalıştırabileceğimiz ortamı açar.       |
| `run <script>`                       | İlgili script’in kullanılmasını sağlar.                                 |
| `load <eklenti>`                     | Metasploit eklentisini yükler. (`load -l` ile eklentiler listelenir.)   |


# Meterpreter - Incognito Modulunun İstifadəsi

`incognito` Meterpreter modulu, əsasən Windows sistemlərində istifadə olunan və istifadəçi tokenləri üzərində manipulyasiya etməyə imkan verən güclü bir post-exploitation vasitəsidir.

---

## 🔍 Nədir `load incognito`?

`load incognito` komandası ilə Meterpreter sessiyasına `incognito` eklentisi əlavə olunur. Bu eklenti ilə sistemdəki digər istifadəçilərin identifikasiyasını (tokenlərini) istifadə edərək onların səlahiyyətləri ilə əmrlər yerinə yetirmək mümkündür.

---

## 🛠️ Əsas İmkanları

### ✅ 1. Token İmpersonation (Təqlid)
Sistemdə aktiv olan istifadəçi tokenlərini təqlid edərək onların hüquqları ilə işləməyə imkan verir.

### ✅ 2. Token Enumeration
Mövcud sessiyada olan bütün tokenləri siyahı şəklində göstərir.

### ✅ 3. Yeni Token İstifadəsi
Əgər sistemdə yüksək hüquqlu (məsələn, `DOMAIN\\admin`) bir istifadəçi sessiyası varsa, onu təqlid edərək yüksək hüquqlar qazanmaq mümkündür.

---

## 🧪 İstifadə Nümunələri

### 🔹 Modulu yükləmək:
```bash
meterpreter > load incognito
```

## Mövcud tokenləri görmək:
```bash
meterpreter > list_tokens -u
```

## Token təqlidi (impersonation):
```bash
meterpreter > impersonate_token "DOMAIN\\admin"
```

## Öz hüquqlarına qayıtmaq:
```bash
meterpreter > rev2self
```

# Unquoted Service Path

Windowsda bu adda bir açıq var hansıki bu açıqdan istifadə edərək proqram çalışdıra və ya imtiyaz qaldıra bilərik. Əgər çalışdırılan proqramların olduğu yer adında boşluq varsa və o dırnaqcıq içində yazılmayıbsa onda o boşluqa qədər olan yerin adında payload yaradıb ora qoyarsaq ilk o işə salınacaq.
Bunu msfconsole-da `post/windows/gather/enum_services` post modulu ilə baxa bilərik.

# Scriptlər

`post/windows/manage/enable_rdp` - windowsda rdp açır.  
`post/windows/manage/persistence_exe` - windowsda verilen exeni hədəf maşına yükləyir açılışda çalışdırır və registery-də əlavə edir.




