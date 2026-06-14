# 🚀 Splunk SPL Cheat Sheet


---

## 🔍 1. Sərbəst Mətn Axtarışı (Free Text Search)
Əgər xüsusi bir sahənin (field) adını bilmirsinizsə və ya tez-tez axtarış etmək istəyirsinizsə, birbaşa açar sözü yaza bilərsiniz. Böyük/kiçik hərf fərqi yoxdur (case-insensitive).

* **Nümunə:** `index=windowslogs alice`
* **Mənası:** `windowslogs` indeksində içində necə yazılmasından asılı olmayaraq (Alice, alice, ALICE) **alice** sözü keçən bütün hadisələri (events) tapır.

---

## 📊 2. Müqayisə Operatorları (Relational Operators)
Sahələrin (fields) içindəki dəyərləri müqayisə etmək üçün istifadə olunur.

| Operator | Mənası | Nümunə | İzahı |
| :---: | :--- | :--- | :--- |
| `=` | Bərabərdir | `UserName=Mark` | `UserName` sahəsi dəqiq **Mark** olan logları tapır. |
| `!=` | Bərabər Deyil | `AccountName!=SYSTEM` | `AccountName` sahəsi **SYSTEM olmayan** bütün logları gətirir. |
| `<` | Kiçikdir | `Age<10` | Yaşı **10-dan kiçik** olanları tapır. |
| `<=` | Kiçikdir və ya Bərabərdir | `Age<=10` | Yaşı **10 və ya 10-dan kiçik** olanları tapır. |
| `>` | Böyükdür | `Outbound_Traffic>50` | Çıxan trafik dəyəri **50-dən çox** olanları tapır. |
| `>=` | Böyükdür və ya Bərabərdir | `Outbound_Traffic>=50` | Çıxan trafik dəyəri **50 və ya 50-dən çox** olanları tapır. |

---

## 🧠 3. Məntiqi Operatorlar (Logical Operators)
Birdən çox şərti bir-birinə bağlamaq üçün istifadə olunur. Splunk-da operatorları **BÖYÜK HƏRFLƏRLƏ** (`AND`, `OR`, `NOT`, `IN`) yazmaq şərtdir.

* **`NOT`**: Müəyyən bir şərtin olmamasını tələb edir.
  * **Nümunə:** `NOT UserName=*` (UserName sahəsi ümumiyyətlə mövcud olmayan logları tapır).
* **`AND`**: Hər iki şərt də eyni vaxtda doğru olmalıdır. (Yazmasanız da Splunk bunu avtomatik `AND` kimi qəbul edir).
  * **Nümunə:** `index=windowslogs AccountName!=SYSTEM AccountName=James`
  * **Mənası:** `AccountName` həm SYSTEM olmasın, həm də mütləq **James** olsun.
* **`OR`**: Şərtlərdən ən azı birinin doğru olması kifayətdir.
  * **Nümunə:** `UserName=David OR UserName=John`
  * **Mənası:** İstifadəçi adı **David və ya John** olan logları tapır.
* **`IN`**: `OR` operatorunun daha qısa və rahat formasıdır (uzun siyahılar üçün idealdır).
  * **Nümunə:** `UserName IN(David, John, Alice)`
  * **Mənası:** İstifadəçi adı David, John və ya Alice olanları tapır.

---

## 🌟 4. Ulduz (`*`) və İP Axtarışı (Wildcards & CIDR)
Sözün və ya İP ünvanının bir hissəsini axtarmaq üçün istifadə olunur.

* **Sözün hissəsi üçün (`*`):** * `status=*fail*` ➡️ İçində "fail" sözü keçən hər şeyi tapır: *failed, failure, appfail*.
* **İP ünvanının bir hissəsi üçün (`*`):** * `DestinationIp=172.*` ➡️ 172 ilə başlayan bütün İP-ləri tapır (məsələn: *172.90.0.1* və ya *172.18.5.22*).
* **İP Subnet (Şəbəkə) axtarışı (CIDR):** * `DestinationIp=172.18.0.0/16` ➡️ Bu şəbəkə diapazonuna düşən bütün İP-ləri tapır.

---

## 🔀 5. Dırnaq işarəsi və Mötərizələrin Gücü (Order of Evaluation)

### 💬 Dırnaq İşarəsi (`""`)
Mətni bütöv bir ifadə kimi axtarmaq və ya Splunk operatorlarını düzgün oxutmaq üçün istifadə olunur.
* `failed login` ➡️ "failed" və "login" sözlərini fərqli yerlərdə də olsa axtarır.
* `"failed login"` ➡️ Yan-yana yazılmış dəqiq **"failed login"** ifadəsini axtarır.
* `"TO BE OR NOT TO BE"` ➡️ Dırnaq içində olduqda, Splunk buradakı `OR` və `NOT` sözlərini operator kimi deyil, adi mətn kimi qəbul edir.

### 📦 Mötərizələr `()`
Splunk normalda `OR` operatorunu `AND` operatorundan daha üstün tutur (əvvəl onu hesablayır). Səhv nəticə almamaq üçün şərtləri mötərizə ilə qruplaşdırmaq lazımdır.

**Məqsəd:** Bizə lazımdır ki, logda eyni anda həm *alice*, həm *bob* olsun, YA DA təkbaşına *charlie* olsun.

❌ **SƏHV YAZILIŞ:** `alice AND bob OR charlie`
* *Splunk bunu belə başa düşür:* `alice AND (bob OR charlie)` (Yəni mütləq alice olmalıdır, yanında da ya bob ya charlie olmalıdır. Bu bizim istədiyimiz deyil).

✅ **DÜZGÜN YAZILIŞ:** `(alice AND bob) OR charlie`
* *Splunk bunu belə başa düşür:* Ya (alice və bob) birlikdə olsun, ya da sadəcə charlie olsun.

---

### 🔄 Boru İşarəsi (`|`) Nədir?
Splunk-da komandalar bir-birinə `|` (pipe) işarəsi ilə bağlanır. Bu işarə özündən əvvəlki komandanın tapdığı nəticəni növbəti komandaya ötürür.

---

## 🛠️ Ən Çox İstifadə Olunan Filtrləmə Komandaları

### 1. 📋 `fields` (Sahələri Seçmək / Gizlətmək)
Loglarda yüzlərlə fərqli məlumat sütunu (sahə) ola bilər. `fields` komandası ekranı təmizləmək və yalnız sizə lazım olan sütunları görmək üçün istifadə olunur.

* **Yalnız müəyyən sahələri göstərmək üçün:**
  * `index=windowslogs | fields host User SourceIp`
  * *Mənası:* Loglardan yalnız `host`, `User` və `SourceIp` sütunlarını mənə göstər, qalanlarını gizlət.
* **Müəyyən bir sahəni silmək/gizlətmək üçün (`-` işarəsi istifadə olunur):**
  * `index=windowslogs | fields - Password`
  * *Mənası:* `Password` sütunundan başqa bütün sahələri göstər (mənfi işarəsi həmin sahəni nəticədən çıxarır).

### 2. 🧽 `dedup` (Təkrarları Silmək)
Eyni olan təkrarlanan məlumatları təmizləyir. Əgər eyni fəaliyyət üçün sistem ardıcıl olaraq onlarla eyni logu göndəribsə, `dedup` hər dəyərdən yalnız 1 unikal nümunə saxlayır.

* **Nümunə:** `index=windowslogs | fields EventID User Image Hostname SourceIp | dedup SourceIp`
* **Mənası:** Eyni İP ünvanından gələn təkrarlanan logları sil və hər İP-dən yalnız bir fərqli hadisə göstər.

### 3. 🏷️ `rename` (Adı Dəyişmək)
Loglardakı sahə adlarını daha anlaşıqlı formaya salmaq üçün istifadə olunur. Bu, xüsusilə SOC hesabatları hazırlayarkən skrinşotların daha oxunaqlı və peşəkar görünməsinə kömək edir.

* **Sahə adını dəyişmək:**
  * `index=windowslogs | fields EventID User Image Hostname SourceIp | rename User as Employee`
  * *Mənası:* Cədvəldəki mürəkkəb `User` başlığını `Employee` (İşçi) olaraq dəyiş.
* **Qarışıq JSON formatlarını sadələşdirmək (Düzləşdirmək):**
  * `index=jsondata | rename request.* as *`
  * *Mənası:* JSON daxilindəki `request.path` və `request.ip` kimi uzun adların önündəki `request.` hissəsini sil və birbaşa `path` və `ip` elə ki, hər dəfə uzun yazmayasan.

### 4. 🎯 `regex` (Mətn Şablonu ilə Axtarış)
Məlumatı dəqiq bir sözlə deyil, müəyyən bir qaydaya və ya mətn şablonuna (PCRE - Regular Expressions) uyğun axtarmaq üçün istifadə olunur.

* **Nümunə:** `index=windowslogs | regex Image = "\.exe$"`
* **Mənası:** `Image` (Fayl yolu) sahəsinin daxilində yalnız sonu **.exe** ilə bitən logları mənə tap gətir (Buradakı `$` işarəsi mətnin sonunu bildirir).

---

## 📋 1. `table` Komandası (Cədvəl Yaratmaq)

Logların arasındakı qarışıqlığı təmizləyir və yalnız seçdiyiniz sahələri təmiz, oxunaqlı bir cədvəl halına salır. Xüsusilə hadisələrin baş vermə xronologiyasını (timeline) qurmaq üçün idealdır.

* **Nümunə:** `index=windowslogs | table _time EventID Hostname SourceName`
* **Mənası:** Tapılan loglardan yalnız vaxtı (`_time`), hadisə İD-sini (`EventID`), kompüter adını (`Hostname`) və mənbəyi (`SourceName`) götür və səliqəli cədvəl qur.

---

## 🛠️ 2. Faydalı Strukturlaşdırma Komandaları

Cədvəl komandası ilə birlikdə və ya təkbaşına istifadə edə biləcəyiniz sürətli komandalar:

| Komand | Nümunə | İzahı (Sadə dildə) |
| :--- | :--- | :--- |
| **`head`** | `... \| head 20` | Ən yeni (ən başda olan) **ilk 20 logu** gətirir. Axtarışı sürətləndirmək üçün əladır. |
| **`tail`** | `... \| tail 20` | Ən köhnə (ən sonda olan) **son 20 logu** gətirir. |
| **`sort`** | `... \| sort User` | Logları `User` (İstifadəçi) adına görə **əlifba sırası ilə** düzür. |
| **`reverse`** | `... \| reverse` | Logların sıralamasını tam tərsinə çevirir (məsələn, köhnədən yeniyə doğru sıralayır). |

---

## 📊 1. Ümumi Transformasiya Komandaları

Məlumatların içində ən çox və ya ən az təkrarlanan elementləri tapmaq üçün istifadə olunur.

* **`top` (Ən çox təkrarlananlar):** Göstərilən sahədə ən çox keçən dəyərləri tapır.
  * **Nümunə:** `index=windowslogs | top User limit=5`
  * **Mənası:** Ən çox logu olan ilk 5 istifadəçini (User) və onların faiz göstəricisini gətirir. (Normalda ilk 10-u gətirir, `limit=5` ilə sayı məhdudlaşdırırıq).
* **`rare` (Ən az təkrarlananlar):** `top` komandasının tam tərsinə, sistemdə ən nadir baş verən dəyərləri tapır (Anomaliyaları və şübhəli fəaliyyətləri tapmaq üçün əladır).
  * **Nümunə:** `index=windowslogs | rare User limit=5`
  * **Mənası:** Sistemdə ən az görünən, ən nadir 5 istifadəçini tapır.

### 🖍️ `highlight` (Rəngləmə/İşıqlandırma)
Mətn şəkilli logların (Raw data) içində axtardığınız sözlərin daha rahat gözə çarpması üçün onları rəngli markerlə qeyd edir.
* **Nümunə:** `index=windowslogs | highlight User EventID Image "Process accessed"`
* **Mənası:** Logların içindəki istifadəçi adlarını, EventID-ləri və "Process accessed" sözünü rəngli göstər.

---

## 📈 2. `stats` Komandası (Riyazi və Statistik Hesablamalar)

Böyük həcmdə log məlumatlarından trendləri və fərqlilikləri tapmaq üçün riyazi hesablamalar aparır.

| Funksiya | Nümunə | İzahı |
| :--- | :--- | :--- |
| **`avg`** (Orta Dəyər) | `| stats avg(ProcessCount)` | Seçilmiş sahənin orta qiymətini (riyazi ortasını) hesablayır. |
| **`max`** (Maksimum) | `| stats max(Price)` | Sahədəki ən böyük (maksimum) dəyəri tapır. |
| **`min`** (Minimum) | `| stats min(UserAge)` | Sahədəki ən kiçik (minimum) dəyəri tapır. |
| **`sum`** (Cəm) | `| stats sum(Cost)` | Həmin sütundakı bütün rəqəmləri toplayır. |
| **`count`** (Say) | `| stats count by SourceIp` | Hansı İP-dən neçə dəfə log gəldiyini (təkrarlanma sayını) tapır. |

* **Praktiki Nümunə:** `index=windowslogs | stats count by EventID | sort EventID`
* **Mənası:** Hər bir EventID-nin sistemdə neçə dəfə baş verdiyini say və EventID sırasına görə düz.

---

## 📉 3. Qrafiklər Yaratmaq (`chart` və `timechart`)

Bu komandalar nəticələri elə bir cədvəl formasına salır ki, Splunk panellərində asanlıqla qrafiklər (diaqramlar) vizuallaşdırmaq mümkün olsun.

* **`chart` (Ümumi Qrafik):** Müəyyən sahələrə görə qrafik hazırlayır.
  * **Nümunə:** `index=windowslogs | chart count by User`
  * **Mənası:** Hansı istifadəçinin nə qədər fəaliyyəti olduğunu göstərən cədvəl qur (bunu sütunlu diaqrama çevirmək olar).
* **`timechart` (Zamana Görə Qrafik):** Məlumatların zaman daxilində necə dəyişdiyini (azaldığını və ya çoxaldığını) göstərir. Trendləri və qəfil sıçrayışları görmək üçün idealdır.
  * **Nümunə:** `index=windowslogs Image!="" | timechart span=30m count by Image limit=5`
  * **Mənası:** Boş olmayan proqram loglarını götür, hər **30 dəqiqəlik (span=30m)** intervalla ən çox işləyən ilk 5 proqramın zamana görə dəyişmə qrafikini qur.

---

## 🧬 4. Məlumatların Zənginləşdirilməsi və Manipulyasiyası

Əlinizdəki mövcud loglara yeni məlumatlar əlavə etmək və ya onları daha oxunaqlı formaya salmaq üçün istifadə olunur.

### 🗺️ `iplocation` (İP-yə görə Coğrafi Məkan Tapmaq)
Logdakı İP ünvanının hansı ölkəyə və ya şəhərə aid olduğunu Splunk-ın daxili bazasından tapıb loga əlavə edir.
* **Nümunə:** `index=windowslogs | iplocation SourceIp | stats count by Country`
* **Mənası:** `SourceIp` sahəsindəki İP-lərin hansı ölkələrə məxsus olduğunu tap və hansı ölkədən neçə log gəldiyini say.

### 📁 `lookup` (Xarici Fayldan Məlumat Çəkmək)
Əlinizdə olan bir CSV faylı ilə logları birləşdirir. Məsələn, logda yalnız `Hostname` var, amma siz həmin kompüterin hansı şöbəyə aid olduğunu bilmək istəyirsinizsə, lookup istifadə edirsiniz.
* **Nümunə:** `index=windowslogs | lookup user_roles Hostname OUTPUT UserRole | stats count by Hostname UserRole`
* **Mənası:** `user_roles` adlı hazır siyahıya bax, logdakı `Hostname` ilə siyahıdakı adı uyğunlaşdır və oradan işçinin rolunu (`UserRole`) götürüb logun yanına yapışdır.

### 🧮 `eval` (Yeni Sahə Yaratmaq və ya Dəyişmək)
Splunk-ın ən güclü komandalarından biridir. Log daxilində riyazi hesablamalar aparmağa, şərtlər qoymağa və ya anlaşılmayan rəqəmləri mətnə çevirməyə imkan verir.

* **Nümunə:** ```splunk
  index=windowslogs
  | eval LogonTypeDesc = case(LogonType == 3, "Network Logon", LogonType == 5, "Service")
  | stats count by LogonType LogonTypeDesc

  # 🕵️‍♂️ Splunk SPL Anomaliyaların Tapılması Komandaları (Azərbaycan Dilində)

Anomaliyaların (qeyri-adi hadisələrin) tapılması üçün istifadə edilən daha mürəkkəb Splunk komandalarının və funksiyalarının çox sadə dildə izahı:

---

### 🔄 `eventstats` (Statistika Hesablama və Logları Saxlama)
Bu komanda eynilə `stats` komandası kimi işləyir — yəni sayır, orta qiymət tapır və ya cəmləyir. Lakin çox vacib bir fərqi var: `stats` komandası bütün logları silib ekranda yalnız bircə yekun cədvəl saxladığı halda, **`eventstats` orijinal logların heç birini silmir**. O, hesabladığı statistik rəqəmi mövcud logların yanına yeni bir sütun (sahə) olaraq yapışdırır.
* **Nə üçün lazımdır?** Logların özünü silmədən, növbəti addımlarda həm xam məlumatlar, həm də hesablama nəticələri üzərində filtrləməyə davam etmək üçün əvəzedilməzdir.

### 2. 🎯 `where` (Matematik və Şərtli Filtrləmə)
Bu komanda ekrandakı nəticələri süzgəcdən keçirmək üçün istifadə olunur (eynilə `search` komandası kimi). Lakin `where` daha ağıllı və güclüdür. O, iki fərqli sütundakı dəyərləri bir-biri ilə müqayisə edə bilir və ya riyazi tənliklər qurmağa imkan verir (Məsələn: `A sahəsi > B sahəsi * 2`).
* **Nə üçün lazımdır?** Hesablanmış xüsusi dəyərlərə (məsələn, anomaliya dərəcəsi 3-dən böyük olanlar: `where zscore > 3`) əsasən yalnız şübhəli logları seçib ayırmaq üçün istifadə olunur.

---

## 📊 Riyazi və Zaman Funksiyaları

### 3. ⏱️ `strftime` (Vaxt Formatını Dəyişmək)
Splunk-ın başa düşdüyü qarışıq zaman göstəricisini (`_time`) bizim oxuya biləcəyimiz formata salır.
* **Nümunə:** `strftime(_time, "%H")` ➡️ Logun daxil olduqu vaxtdan yalnız **Saat** hissəsini (məsələn: 13 və ya 18) rəqəm olaraq qoparır.

### 4. 🔢 `tonumber` (Mətni Rəqəmə Çevirmək)
Sistemdə yazı (mətn) formatında olan rəqəmləri riyazi hesablamalar apara biləcəyimiz həqiqi rəqəm tipinə çevirir. (Məsələn, dırnaq içindəki `"12"` mətnini riyazi `12` rəqəmi edir).

### 5. 📉 `stdev` (Standart Meyl - Standard Deviation)
Məlumatların orta qiymətdən nə qədər uzaqlaşdığını (dəyişkənliyini) ölçən riyazi funksiyadır.
* **Sadə İzahı:** Bir işçi hər gün dəqiq saat 09:00-da sistemə girirsə, onun standart meyli `0`-a yaxın olur (yəni davranışı sabitdir). Əgər gah gecə, gah günorta xaotik daxil olursa, bu rəqəm böyük olur.

### 6. 🧮 `abs` (Modul / Mütləq Qiymət)
Riyaziyyatdan bildiyimiz modul funksiyasıdır. Çıxma əməliyyatının nəticəsi mənfi (minus) alınsa belə, onu müsbətə (plus) çevirir. (Məsələn, `abs(-5)` bizə `5` cavabını verir).

---

## 🤖 Qabaqcıl Komandalar (Maşın Öyrənməsi)

### 7. 🧠 `fit` və `apply` (Machine Learning)
Splunk-ın süni intellekt və maşın öyrənməsi alətlədir. 
* **`fit`** komandası keçmiş logları analiz edərək işçilərin "normal" davranış şablonunu öyrənir (modeli təlimatlandırır).
* **`apply`** isə həmin öyrənilmiş şablonu yeni gələn loglara tətbiq edir və gələcəkdə baş verə biləcək qeyri-adi təhlükələri avtomatik tanıyır.
