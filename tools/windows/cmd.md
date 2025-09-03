# CMD

```cmd
tasklist => list tasks
taskkill -pid (/pid - same with other) <PID> => kill any task
tree => show directory with altdirectories
mkdir => make directory
rmdir => remove directory
copy filename to_where => copy file
move filename to_where => move file
shutdown => shutdown (restart, make time to shutdown  pc. see /? or /h params all other params)
pathping 1.1.1.1 => like tracerout
sc query /any_params => check all services with params
net start (stop/restart) service_name => start service
reg add (delete) reg_address any_params => registry command
```
---

# Windows CMD `net user` Komandası - Geniş İzah və Nümunələr

`net user` komandası Windows əməliyyat sistemində istifadəçiləri idarə etmək üçün istifadə olunur. Bu sənəd bütün əsas opsiyaları, nümunələri və izahları ilə birlikdə təqdim edir.

---

## Əsas Sintaksis

```
net user [username [password | *] [options]] [/domain]
net user [username] /delete
net user /add
net user
```

* `net user` – bütün istifadəçiləri göstərir.
* `net user [username]` – müəyyən istifadəçinin profil məlumatlarını göstərir.
* `net user [username] /add` – yeni istifadəçi əlavə edir.
* `net user [username] [password]` – istifadəçinin şifrəsini dəyişir.
* `net user [username] /delete` – istifadəçini silir.

---

## Əsas Nümunələr və İzah

### 1. Bütün istifadəçiləri göstərmək

```
net user
```

**İzah:**
Bütün lokal istifadəçilərin siyahısını göstərir.

---

### 2. Müəyyən istifadəçi haqqında məlumat

```
net user Ilkin
```

**İzah:**
"Ilkin" adlı istifadəçinin hesab məlumatlarını göstərir:

* Son giriş vaxtı
* Şifrə qadağaları
* Qrup üzvlüyü
* Hesabın aktiv olub-olmaması

---

### 3. Yeni istifadəçi əlavə etmək

```
net user TestUser MyPass123 /add
```

**İzah:**

* `TestUser` → istifadəçi adı
* `MyPass123` → şifrə
* `/add` → yeni istifadəçi yaradılması

---

### 4. İstifadəçinin şifrəsini dəyişmək

```
net user TestUser NewPass456
```

**İzah:**
`TestUser` adlı istifadəçinin şifrəsini `NewPass456` olaraq dəyişir.

---

### 5. İstifadəçini silmək

```
net user TestUser /delete
```

**İzah:**
`TestUser` hesabını silir.

---

### 6. Hesabı aktiv/passiv etmək

```
net user TestUser /active:no
net user TestUser /active:yes
```

**İzah:**

* `/active:no` → istifadəçi hesabını deaktiv edir.
* `/active:yes` → istifadəçi hesabını aktiv edir.

---

### 7. Şifrə dəyişməsini məhdudlaşdırmaq

```
net user TestUser /passwordchg:no
```

**İzah:**

* `/passwordchg:no` → istifadəçi şifrəsini dəyişə bilməz.
* `/passwordchg:yes` → istifadəçiyə şifrəni dəyişmək icazəsi verir.

---

### 8. Şifrə tələb olunması

```
net user TestUser /passwordreq:yes
net user TestUser /passwordreq:no
```

**İzah:**

* `/passwordreq:yes` → şifrə tələb olunur.
* `/passwordreq:no` → şifrə olmadan hesab yaradıla bilər.

---

### 9. Hesabı qruplara əlavə etmək

```
net localgroup Administrators TestUser /add
```

**İzah:**
`Administrators` qrupuna `TestUser` əlavə edir.

---

### 10. Domen üzərində əmrlər

```
net user /domain
net user Ilkin /domain
```

**İzah:**

* `/domain` → domen istifadəçilərini göstərmək üçün istifadə olunur (domain controller varsa).

---

## Əlavə Opsiyalar

| Opsiya              | İzah                                  |                                        |
| ------------------- | ------------------------------------- | -------------------------------------- |
| /active:{yes        | no}                                   | Hesabı aktiv/deaktiv edir              |
| /comment:"text"     | İstifadəçi haqqında şərh əlavə edir   |                                        |
| /expires:{date      | never}                                | Hesabın sona çatma tarixini təyin edir |
| /homedir\:path      | İstifadəçi üçün ev qovluğu təyin edir |                                        |
| /profilepath\:path  | İstifadəçi profilinin yeri            |                                        |
| /scriptpath\:path   | Hesab üçün logon script faylı         |                                        |
| /times:{times       | all}                                  | Hesabın giriş saatlarını təyin edir    |
| /usercomment:"text" | İstifadəçi haqqında əlavə məlumat     |                                        |

---

## Geniş Nümunələr

### 11. Hesabın sona çatma tarixini təyin etmək

```
net user TestUser /expires:12/31/2025
```

**İzah:**
`TestUser` hesabının 31 Dekabr 2025 tarixində sona çatmasını təyin edir.

---

### 12. Ev qovluğu təyin etmək

```
net user TestUser /homedir:C:\Users\TestUser
```

**İzah:**
`TestUser` üçün ev qovluğu `C:\Users\TestUser` olaraq təyin edilir.

---

### 13. Hesab üçün logon script təyin etmək

```
net user TestUser /scriptpath:\\Server\Scripts\login.bat
```

**İzah:**
`TestUser` daxil olduqda `login.bat` script faylı işləyir.

---

### 14. Hesab üçün giriş vaxtlarını təyin etmək

```
net user TestUser /times:M-F,08:00-17:00
```

**İzah:**
`TestUser` yalnız Bazar ertəsi-Cümə 08:00-17:00 arasında daxil ola bilər.

---

### 15. Şərh əlavə etmək

```
net user TestUser /comment:"Bu test istifadəçisidir"
```

**İzah:**
Hesaba şərh əlavə edir ki, hesab haqqında əlavə məlumat əldə olunsun.

---

# Windows CMD `net group` Komandası - Bütün Əmrlər və Nümunələr

`net group` komandası Windows əməliyyat sistemində qrupları idarə etmək üçün istifadə olunur. Bu sənəd bütün əsas opsiyaları, nümunələri və izahları ilə birlikdə təqdim edir.

---

## Əsas Sintaksis

```
net group [groupname [/comment:"text"] [/domain]]
net group [groupname] username [/add] [/delete] [/domain]
net group
```

* `net group` – bütün lokal qrupları göstərir.
* `net group [groupname]` – müəyyən qrup haqqında məlumat göstərir.
* `net group [groupname] username /add` – istifadəçini qrupa əlavə edir.
* `net group [groupname] username /delete` – istifadəçini qrupdan silir.
* `/comment:"text"` – qrup haqqında şərh əlavə edir.
* `/domain` – domen qrupları üzərində əmrlər icra etmək üçün istifadə olunur.

---

## Nümunələr və İzah

### 1. Bütün qrupları göstərmək

```
net group
```

**İzah:**
Bütün lokal qrupların siyahısını göstərir.

---

### 2. Müəyyən qrup haqqında məlumat

```
net group Administrators
```

**İzah:**
`Administrators` qrupunun üzvlərini və qrup haqqında digər məlumatları göstərir.

---

### 3. Yeni istifadəçini qrupa əlavə etmək

```
net group Developers Ilkin /add
```

**İzah:**
`Ilkin` istifadəçisini `Developers` qrupuna əlavə edir.

---

### 4. İstifadəçini qrupdan silmək

```
net group Developers Ilkin /delete
```

**İzah:**
`Ilkin` istifadəçisini `Developers` qrupundan silir.

---

### 5. Qrup üçün şərh əlavə etmək

```
net group Developers /comment:"Bu qrup proqramçıları əhatə edir"
```

**İzah:**
`Developers` qrupu üçün şərh əlavə edir.

---

### 6. Domen üzərində əmrlər

```
net group /domain
net group Administrators /domain
```

**İzah:**

* `/domain` → domen qruplarını göstərmək və idarə etmək üçün istifadə olunur.

---

## Əlavə Opsiyalar

| Opsiya          | İzah                                      |
| --------------- | ----------------------------------------- |
| /add            | İstifadəçini qrupa əlavə edir             |
| /delete         | İstifadəçini qrupdan silir                |
| /comment:"text" | Qrup haqqında şərh əlavə edir             |
| /domain         | Domen qrupları üzərində əmrlər icra etmək |

---

## Geniş Nümunələr

### 7. Çoxlu istifadəçiləri qrupa əlavə etmək

```
net group TestGroup User1 /add
net group TestGroup User2 /add
```

**İzah:**
`User1` və `User2` istifadəçilərini `TestGroup` qrupuna əlavə edir.

---

### 8. Domen qrupu üzvlərini göstərmək

```
net group DomainAdmins /domain
```

**İzah:**
Domen üzərindəki `DomainAdmins` qrupunun üzvlərini göstərir.

---

### 9. Qrup şərhini yeniləmək

```
net group Managers /comment:"Menecerlər qrupu"
```

**İzah:**
`Managers` qrupuna yeni şərh əlavə edir.

---



