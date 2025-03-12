# Injections

- SQL injection


## SQL İnjection Aşkarlanması Üçün Testlər

1. **Sadə riyazi ifadələr ilə test edin** – Əgər tətbiqdə rəqəmli bir dəyər varsa, onu ekvivalent bir riyazi ifadə ilə əvəz etməyə çalışın. Məsələn, əgər orijinal dəyər 2-dirsə, 1+1 və ya 3-1 göndərin. Əgər tətbiqin cavabı dəyişməz qalırsa, bu, zəifliyin ola biləcəyinə işarədir.

2. **Bu testi təsdiqləmək üçün əlavə yoxlamalar aparın** – Bu metod ən çox, dəyişdirilən elementin tətbiqin davranışına təsir etdiyini təsdiqlədiyiniz hallarda etibarlıdır. Məsələn, əgər tətbiq PageID adlı rəqəmli parametrlə hansı məzmunun qaytarılacağını təyin edirsə və 2 əvəzinə 1+1 yazdıqda eyni nəticə alınırsa, bu, SQL inyeksiyasının mümkünlüyünü göstərir. Lakin, əgər parametrə istənilən giriş göndərmək mümkündürsə və tətbiq eyni davranışı nümayiş etdirirsə, bu test zəifliyin olub-olmadığını müəyyən edə bilmir.

3. **Daha mürəkkəb ifadələrdən istifadə edin** – Əgər ilkin test müsbət nəticə verərsə, SQL-ə xas açar sözlər və sintaksis daxil edən daha mürəkkəb ifadələrdən istifadə edərək zəifliyin daha da aşkarlanmasını təmin edə bilərsiniz. Məsələn, ASCII funksiyasından istifadə edə bilərsiniz. ASCII funksiyası verilən simvolun ASCII dəyərini qaytarır. A hərfinin ASCII dəyəri 65 olduğu üçün aşağıdakı ifadə SQL-də 2-yə ekvivalent olacaq:
   ```sql
   67-ASCII('A')
   ```

4. **Tək dırnaq (‘) filtrasiyasını aşın** – Əgər tətbiq tək dırnaqları filtr edirsə, bu halda SQL-in rəqəmləri mətn tipinə avtomatik çevirmə xüsusiyyətindən faydalana bilərsiniz. Məsələn, 1 simvolunun ASCII dəyəri 49 olduğu üçün aşağıdakı ifadə SQL-də 2-yə ekvivalent olacaq:
   ```sql
   51-ASCII(1)


## URL Kodlaşdırma Mövzusunda Məsləhətlər

Tətbiqdə SQL inyeksiya kimi zəiflikləri araşdırarkən edilən ümumi səhvlərdən biri, bəzi simvolların HTTP sorğularında xüsusi mənaya malik olduğunu unutmaqdır. Əgər bu simvolları hücum yükünüzə daxil etmək istəyirsinizsə, onların düzgün şəkildə URL kodlaşdırıldığından əmin olmalısınız. 

Xüsusilə aşağıdakı məqamları nəzərə alın:

- **& və = işarələri** – Bunlar sorğu sətrində və POST məlumat blokunda ad/dəyər cütlərini birləşdirmək üçün istifadə olunur. Onları müvafiq olaraq `%26` və `%3d` kimi kodlaşdırmalısınız.
- **Boşluqlar (spaces)** – Sorğu sətrində boşluqlara icazə verilmir və göndərildikdə bütöv sətri yararsız hala gətirə bilər. Boşluqları `+` və ya `%20` ilə kodlaşdırmalısınız.
- **+ işarəsi** – Boşluqları kodlaşdırmaq üçün istifadə olunur. Əgər həqiqi `+` simvolunu daxil etmək istəyirsinizsə, onu `%2b` şəklində kodlaşdırmalısınız. Buna görə də, `1+1` ifadəsi `1%2b1` kimi göndərilməlidir.
- **Nöqtəli vergül (;)** – Cookie sahələrini ayırmaq üçün istifadə olunur və `%3b` kimi kodlaşdırılmalıdır.

## SQL INSERT Bəyanatına İnjection Edərkən Məsləhətlər

**INSERT bəyanatına inyeksiya etməyə çalışarkən**, əvvəlcədən neçə parametrin tələb olunduğunu və onların hansı tiplərdə olduğunu bilməyə bilərsiniz. Bu vəziyyətdə, istədiyiniz istifadəçi hesabı yaradılana qədər **VALUES** bölməsinə əlavə sahələr daxil etməyə davam edə bilərsiniz. 

Məsələn, istifadəçi adını (`username`) hədəfləyərək aşağıdakı girişləri sınaya bilərsiniz:

```sql
foo')--
foo', 1)--
foo', 1, 1)--
foo', 1, 1, 1)--
```

Çünki **əksər verilənlər bazaları tam ədədləri (integer) mətnə (string) avtomatik çevirir**, hər mövqedə tam ədəd dəyərindən istifadə etmək mümkündür. Bunun nəticəsində **foo** adlı istifadəçi hesabı və **1** şifrəsi olan qeyd yaradılacaq. Digər sahələrin sırası fərqli olsa belə, nəticə dəyişməyəcək.

Əgər **1 dəyəri qəbul edilmirsə**,  əvəzinə **2000** istifadə etməyə çalışın. Çünki bir çox verilənlər bazası bu dəyəri **tarix (date-based) məlumat tipi** kimi avtomatik qəbul edə bilər.

## SQL UNION və ORDER BY ilə Sütun Sayını Müəyyən Etmək

Tətbiqin icra etdiyi sorğunun neçə sütun qaytardığını öyrənmək üçün iki əsas üsul mövcuddur:

### 1. UNION SELECT ilə Sütun Sayısını Tapmaq

NULL dəyərlərin hər hansı bir məlumat növünə çevrilə bilməsi xüsusiyyətindən istifadə edərək, müxtəlif sayda sütunlarla sorğu inyeksiya edə bilərsiniz. Doğru sayda sütun daxil edildikdə, inyeksiya edilmiş sorğu icra ediləcək:

```sql
' UNION SELECT NULL--
' UNION SELECT NULL, NULL--
' UNION SELECT NULL, NULL, NULL--
```

Əgər verilənlər bazası xətaları qaytarmırsa, sorğunun uğurla icra edildiyini **əlavə bir sətirin qaytarılmasından** anlaya bilərsiniz. Bu sətirdə ya **NULL** dəyəri, ya da boş bir mətn olacaq.

### 2. ORDER BY ilə Sütun Sayısını Tapmaq

Sorğunun **ORDER BY** hissəsinə ardıcıl sütun nömrələri əlavə edərək, sütun sayını müəyyən edə bilərsiniz:

```sql
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

Əvvəlki sorğular orijinal nəticələri fərqli ardıcıllıqda qaytaracaq. **Xəta baş verən zaman isə keçərsiz sütun nömrəsi təyin etdiyiniz üçün əsl sütun sayını tapmış olursunuz.**

### 3. Mətn Tipli Sütunu Tapmaq

Sütun sayını müəyyən etdikdən sonra, **string (mətn) tipli bir sütunu** tapmaq lazımdır ki, verilənlər bazasından məlumat çıxarmaq mümkün olsun. Bunun üçün NULL dəyərlərindən istifadə edərək əvvəlki sorğunu yenidən inyeksiya edin və hər dəfə bir NULL dəyərini `a` ilə əvəzləyin:

```sql
' UNION SELECT 'a', NULL, NULL--
' UNION SELECT NULL, 'a', NULL--
' UNION SELECT NULL, NULL, 'a'--
```

Əgər inyeksiya edilmiş sorğu icra olunarsa və **əlavə bir sətirdə 'a' dəyəri görünərsə**, deməli həmin sütun **mətn tipindədir**. Bu sütundan istifadə edərək verilənlər bazasından məlumat çıxarmaq mümkündür.

### 4. Oracle Verilənlər Bazasında UNION SELECT

**Qeyd:** Oracle verilənlər bazasında **hər bir SELECT sorğusunda FROM ifadəsi olmalıdır**. Buna görə də, `UNION SELECT NULL` inyeksiyası, sütun sayısından asılı olmayaraq, xəta verəcəkdir. 

Bu problemi həll etmək üçün **DUAL** adlı qlobal cədvəldən məlumat seçmək olar:

```sql
' UNION SELECT NULL FROM DUAL--
```


