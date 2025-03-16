
## Təhlükəsizlik Məlumatları: LDAP İnyeksiyası Təhlili

### 1. * Simvolunun istifadə edilməsi
Yalnız `*` simvolunu axtarış sözü kimi daxil etməyi cəhd edin. Bu simvol LDAP-da wild card kimi işləyir, amma SQL-də işləməz. Əgər çox sayda nəticə alırsınızsa, bu, LDAP sorğusu ilə işlədiyinizi göstərən yaxşı bir göstəricidir.  

### 2. Bağlama Qapaqlarının İstifadəsi
Bağlama qapaqlarının bir neçə ədədini daxil etməyi cəhd edin:  
`))))))))))`  
Bu giriş, daxil etdiyiniz məlumatları əhatə edən bütün bağlama qapaqlarını və əsas axtarış filtrini əhatə edənləri bağlayacaq və uyğun olmayan bağlama qapaqları yaradaraq sorğu sintaksisini etibarsızlaşdıracaq. Əgər səhv baş verərsə, tətbiq LDAP inyeksiyasına qarşı zəif ola bilər.  
(Qeyd edək ki, bu giriş başqa növ tətbiq məntiqini də poza bilər, buna görə yalnız LDAP sorğusu ilə işlədiyinizə əmin olduğunuz zaman bu göstərici güclüdür.)

### 3. Bir sıra İfadələr ilə Test
Növbəti kimi bir sıra ifadələr daxil etməyi cəhd edin, beləliklə heç bir səhv baş vermədikdə, sorğunun qalan hissəsini idarə etmək üçün nə qədər bağlama qapağına ehtiyacınız olduğunu müəyyən edin:  
`*);cn;`  
`*));cn;`  
`*)));cn;`  
`*)))));cn;`  

### 4. Atributlar Əlavə Edilməsi
Girişinizin sonuna əlavə atributlar əlavə etməyi cəhd edin, hər birini ayırmaq üçün vergül istifadə edin. Hər atributu sınaqdan keçirin — səhv mesajı, atributun bu kontekstdə keçərli olmadığını göstərir. LDAP ilə sorğulanan kataloqlarda tez-tez istifadə olunan atributlar arasında bunlar daxildir:  
`cn`, `c`, `mail`, `givenname`, `o`, `ou`, `dc`, `l`, `uid`, `objectclass`, `postaladdress`, `dn`, `sn`  
