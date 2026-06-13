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

