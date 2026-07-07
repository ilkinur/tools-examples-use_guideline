# Active Directory DCSync hücumu
Hücumçu hədəf aldığı Domain Controller (DC) serverinə birbaşa daxil olmadan (yəni ora qoşulmadan və ya fayl sisteminə sızmadan), 
sanki başqa bir DC imiş kimi davranaraq istifadəçilərin şifrə heşlərini (password hashes) əldə edir.

### DCSync Hücumunun Məntiqi Nədir?
Böyük korporativ şəbəkələrdə ehtiyat nüsxə yaratmaq və ya yükü balanslaşdırmaq üçün adətən birdən çox Domain Controller (DC) olur. 
Bu DC-lər öz aralarında məlumatları (istifadəçi adları, şifrələr və s.) sinxronizasiya etməlidirlər. Bu prosesə Replication (Sinxronizasiya) deyilir.

Sinxronizasiya prosesi zamanı Microsoft-un MS-DRSR (Directory Replication Service Remote Protocol) protokolundan istifadə olunur.

Hücumun mahiyyəti: Hücumçu şəbəkədə kifayət qədər yüksək imtiyaza sahib bir istifadəçi hesabını ələ keçirdikdən sonra, 
sistemə "Mən də bir Domain Controller-əm, zəhmət olmasa mənə filan istifadəçinin (məsələn, Administratorun) şifrə heşini göndər" sorğusu yollayır. 
Hədəf DC isə sorğunun qanuni olduğunu düşünərək şifrə heşini hücumçuya təslim edir.

### DCSync Hücumu Üçün Hansı İmtiyazlar Lazımdır?
Hər yoldan ötən istifadəçi DCSync edə bilməz. Hücumun baş tutması üçün ələ keçirilən hesabın Active Directory üzərində xüsusi ACE (Access Control Entries) icazələri olmalıdır.
Bu icazələr aşağıdakılardır:

- DS-Replication-Get-Changes
- DS-Replication-Get-Changes-All
- DS-Replication-Get-Changes-In-Filtered-Set (bəzi hallarda)

Default olaraq bu icazələrə malik olan qruplar:
- Administrators
- Domain Admins
- Enterprise Admins
- Domain Controllers

### DCSync Hücumu Necə Edilir?

Metod A: Mimikatz ilə (Şəbəkə daxilindən)
Hücumçu artıq sistemə sızıb və lazımi imtiyazlara sahibdirsə, 
Mimikatz-ı işə salaraq tək bir komanda ilə istənilən istifadəçinin (məsələn, krbtgt və ya Administrator) heşini çəkə bilər.

Mimikatz-ı açır:  
`mimikatz.exe`

DCSync komandasını icra edir:  
`lsadump::dcsync /domain:hedefdomain.local /user:krbtgt`

Metod B: Impacket (secretsdump.py) ilə (Kənardan)
Əgər hücumçunun əlində lazımi icazələri olan istifadəçinin login-şifrəsi varsa, Linux terminalından AD serverinə heç daxil olmadan bu komandanı yazır:
`impacket-secretsdump hedefdomain.local/istifadeci_adi:sifre@192.168.1.10 -just-dc-user krbtgt`  
Bu komanda birbaşa DC-yə (192.168.1.10) qoşulur, sinxronizasiya sorğusu göndərir və krbtgt istifadəçisinin NTLM heşini ekrana çap edir.  

Burada krbtgt hesabı seçilir, çünki bu hesabın heşi (NTLM) ələ keçirilərsə, hücumçu Golden Ticket yaradaraq şəbəkədə limitsiz müddətə "tanrı" rejimini aktivləşdirə bilər.

### DCSync Hücumundan Necə Qorunmaq Olar?
DCSync hücumunu bloklamaq çətindir, çünki bu, Active Directory-nin öz rəsmi funksiyasını sui-istifadə edir.
Lakin aşağıdakı tədbirlərlə risk minimuma endirilir:  
- İmtiyazların Auditi: AD mühitində Domain Admins/Administrators qrupları xaricində heç bir hesaba DS-Replication-Get-Changes icazələrinin verilmədiyindən əmin olmaq lazımdır.
- SIEM və Monitorinq: Şəbəkədə qeyri-adi kompüterlərdən (məsələn, adi işçi kompüterindən) DC-yə doğru gələn sinxronizasiya (Replication) sorğuları dərhal izlənilməli və bloklanmalıdır. (ID 4662 auditi aktiv edilməlidir).
- Tiered Administration (Pilləli İdarəetmə): Domain Admin hesablarının adi işçi kompüterlərinə daxil olması (login olması) qətiyyən qadağan edilməlidir ki, onların şifrələri və ya sessiyaları oğurlanmasın.
