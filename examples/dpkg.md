# dpkg Reference Guide

`dpkg` (*Debian Package Manager*) Debian və Kali Linux əməliyyat sistemlərində yerli `.deb` paketləri ilə birbaşa işləyən aşağı səviyyəli (low-level) paket idarəetmə alətidir.

---

## 1. Əsas Əmrlər və Parametrlər

| Parametr / Flaq | Sintaksis | Əməliyyat Tipi | İzahı və Texniki İncəlikləri |
| :--- | :--- | :--- | :--- |
| **`-i`** / `--install` | `dpkg -i <file.deb>` | Quraşdırma | `.deb` arxivini açır və faylları sistemə köçürür. **Əsas xüsusiyyət:** Asılılıqları (dependencies) həll etmir. Çatışmayan paket olarsa, quraşdırma yarımçıq qalır (*Unpacked/Half-Configured*). |
| **`-r`** / `--remove` | `dpkg -r <package>` | Silmə | Binar faylları silir, lakin konfiqurasiya fayllarını (`/etc/` daxilindəki) və logları sistemdə saxlayır (*Half-Installed/Config-Files* statusu). |
| **`-P`** / `--purge` | `dpkg -P <package>` | Silmə | Binar faylları, konfiqurasiya fayllarını və bütün əlaqəli məlumatları sistemdən **tamamilə** təmizləyir. |
| **`-l`** / `--list` | `dpkg -l [pattern]` | Soruşma | Sistemdəki yüklü paketləri siyahılayır. Çıxışdakı ilk iki hərf statusu göstərir (məs: `ii` = Installed, `rc` = Removed but Config remains). |
| **`-L`** / `--listfiles` | `dpkg -L <package>` | Audit/Axtarış | Müəyyən bir paket tərəfindən sistemə (məs: `/usr/bin/`, `/usr/share/`) yazılmış **bütün fayl və qovluqların tam yollarını** siyahılayır. |
| **`-S`** / `--search` | `dpkg -S <pattern/file>` | Audit/Axtarış | **Əks-axtarış (Reverse lookup):** Sistemdəki hər hansı bir binar və ya konfiqurasiya faylının **hansı paketə aid olduğunu** tapır (məs: `dpkg -S /usr/bin/msfconsole`). |
| **`-s`** / `--status` | `dpkg -s <package>` | Soruşma | Yüklənmiş paketin metadata məlumatlarını (`/var/lib/dpkg/status` faylından) çıxarır: versiya, asılılıqlar, arch və status. |
| **`-V`** / `--verify` | `dpkg -V [package]` | İnteqritet / Təhlükəsizlik | **Fayl Bütövlüyünün Auditi:** Paket fayllarının MD5 heş dəyərlərini, ölçüsünü və icazələrini ilkin halı ilə müqayisə edir. Dəyişdirilmiş (modified) və ya zədələnmiş faylları üzə çıxarır. |
| **`-I`** / `--info` | `dpkg -I <file.deb>` | Metadata | **Oflayn Analiz:** Hələ sistemə yüklənməmiş `.deb` faylının idarəetmə (`control`) məlumatlarını və asılılıq tələblərini göstərir. |
| **`-c`** / `--contents` | `dpkg -c <file.deb>` | Oflayn Analiz | Hələ yüklənməmiş `.deb` faylının daxilində olan bütün fayl/qovluq strukturunu göstərir (tar arxivinə baxmaq kimi). |
| **`-x`** / `--extract` | `dpkg -x <file.deb> <dir>` | Oflayn Analiz | `.deb` paketini sistemə quraşdırmadan, daxilindəki faylları göstərilən hədəf qovluğuna çıxarır (sistem kökünə müdaxilə etmir). |
| **`-e`** / `--control` | `dpkg -e <file.deb> [dir]` | Oflayn Analiz | `.deb` paketi daxilindəki skriptləri (`preinst`, `postinst`, `prerm`, `postrm`) və `control` faylını çıxarır. |
| **`-C`** / `--audit` | `dpkg -C` | Troubleshooting | Sistemdə yarıda qalmış, səhv yüklənmiş və ya konfiqurasiya gözləyən zədəli paketləri axtarır. |
| **`--print-architecture`** | `dpkg --print-architecture` | Soruşma | Host arxitekturasını gətirir. |
| **`--add-architecture`** | `dpkg --add-architecture <i386>` | Əlavə etmə | Bu əmr vasitəsilə sisteminizə cari əməliyyat sisteminin arxitekturasından fərqli olan proqram paketlərinin yüklənməsinə icazə verilir [64-bit (amd64) Linux sisteminə 32-bit (i386)]. |
| **`--remove-architecture`** | `dpkg --remove-architecture <i386>` | Silmə | Əlavə edilmiş arxitekturanı silir. |
| **`--print-foreign-architectures`** | `dpkg --print-foreign-architecture` | Soruşma | Hosta əlavə hansə kənar arxitektura yüklənibsə gətirir. |
| **`--unpack`** | `dpkg --unpack <paket_adı.deb>` | Half Install | Paketin sistemə tam quraşdırmadan yalnız faylları arxivdən çıxarır və aidiyyəti qovluqlara yerləşdirir (`--configure` ilə `--install` mərhələsini tamamlamaq olar). |
| **`--configure`** | `dpkg --configure -a` | Məcburi Konfiqurasiya | Yarıda qalmış quraşdırmaları və ya dependency həllindən sonra konfiqurasiya olunmamış paketləri tam işlək vəziyyətə gətirir. |
| **`--get-selections`**| `dpkg --get-selections` | Backup/Miqrasiya | Sistemdəki bütün paketlərin və onların statuslarının (install, hold, purge) siyahısını mətn formatında çıxarır. |
| **`--set-selections`**| `dpkg --set-selections` | Backup/Miqrasiya | Mətn faylından paket siyahısını oxuyub, quraşdırma statuslarını işarələyir (`apt-get dselect-upgrade` ilə tamamlanır). |

---

## 2. Qeydlər

* **Dependency Handling:** `dpkg` asılılıqları avtomatik həll edə bilmir. Əgər `dpkg -i` xəta verərsə, asılılıqları düzəltmək üçün növbəti addımda **`apt-get install -f`** və ya **`apt --fix-broken install`** icra edilməlidir.
* **Database Path:** `dpkg` öz verilənlər bazasını və yüklənmə statuslarını **`/var/lib/dpkg/`** kataloqunda saxlayır.
