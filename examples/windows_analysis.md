# Windows System Analysis

İlk öncə sistem haqda məlumat almaq lazımdı cmd-də `systeminfo` yazaraq çıxan məlumatları bir fayla yazıb saxlamaq lazımdı. Daha sonra bu faylı `wesng` tooluna veririk
sistemdə mümkün olan bütün açıqlıqları detallıca gətirir və bizə təkcə bunu analiz etmək hansının işə yarayıb hansının yaramadığını yoxlamaq lazımdır.

#### Privilege Escalation

İlk öncə `getuid`[meterpreter] ilə sistemə girdiyimiz userin imtiyazlarını görə bilərik. Ən yüksək imtiyaz `NT_AUTHORİTY\SYSTEM`-dir.  
`getprivs`[meterpreter] ( `whoami /priv`[cmd] ) ilə userin sistemdə edə biləcəyi işləri görə bilərik.

`load incognito`[meterpreter] əlavələri hədəfə yükləyəcək və `load`[meterpreter] yazdıqda işlədə biləcək əlavələrin listi çıxacaq.
`list_tokens -u`[meterpreter] ilə daha yüksək imtiyazlı istifadəçiləri token ilə təqlid edə bilib bilməyəcəyini göstərir.
Əgər belə bir imtiyaz olarsa onda `impersonate_token "NT_AUTHORİTY\SYSTEM"` yazaraq o imtiyaza sahib olacaq.


