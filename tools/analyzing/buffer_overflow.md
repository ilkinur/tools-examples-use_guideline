# Buffer Overflow

`buffer` - verilənlərin geçici saxlandığı yerdir.  
`EİP` - instruction pointer olaraq adlanır. CPU-un an etibari ilə code segmenti içərisindəki hansı instruction çalışacağını göstərir.  
`code segment` - işləmcinin çalışdırdığı maşın əmrlərinin olduğu bölgə. `EİP` registeri bu bölgədən bir yaddaşa işarə edir. Sadəcə oxunabilər və yazılabilər.  
`ESP` - stack pointer. Stack veri yapısının LİFO elementini göstərir.  
`msf_pattern_create` - buffer overflovvda qırılma nöqətsini tapır.  
`msf_pattern_offset` - qırılma nöqtəsinin doldurduğu dəyişən verilir beləcə dəqiq qırılma uzunluğu tapılır.  
