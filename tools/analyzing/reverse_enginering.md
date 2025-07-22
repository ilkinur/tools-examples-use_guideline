# Reverse Enginering

`xdd` - verilən inputu `hexdump` versiyada verir.  
`readelf` - elf formatındakı fayl haqda çox geniş məlumat verir.  
`objdump` - fayl haqda məlumat verir.  
`checksec --file=file` - fayl haqda məlumat.  
`rabin2 file` - fayl haqda məlumat.  
`file file` - fayl haqda məlumat.  
`strings file` - fayldakı stringləri gətirir.  
`ldd` - paylaşılan object və ya library haqda məlumat verir.  
`gdb` - debuger.  
`cutter (ghidra)` - gui code dissasembler.  
`.init` - başlatma kodları - constructor.  
`.fini` - bitirmə kodları - destructor.  
`.text` - proqramın icra edilə bilən kodunun saxlandığı yerdir.  
`.plt` - funksiyanın çağırılmasını icra edir. `.got`-da yoxsa ora yazır.  
`.got` - funksiyanın kitabxanadakı ünvanını tutur.  
`.rela` - bir proqramın yaddaşda düzgün işləməsi üçün yenidən ünvanlaşması (relocation) lazım olan yerlərin siyahısını saxlayan bir relocation table-dir.  
`mov <destination>, <source>` - rəqəmi köçürür.  
`add <destination>, <source1>, <source2>` - s-lərdən d-yə əlavə edir.  
`sub <dest>, <src1>, <src2>` - src1-src2=dest yazır.  
`mul <dest>, <src1>, <src2>` - dest=src1*src2  
`ldr <des>, <source>` - yaddaşdan datani registeriyə yazır.  
`str source, [destination]` - bir registerdəki məlumatı yaddaşa yazmaq üçün istifadə edilir.  
`b <label>` - şərtsiz sıçrayış.  
`b {condition} <label>` - şərtli sışrayış (`beg, bne, bgt, blt`).  
`bl <label>` - funksiya çağırışı (branch üith link).  
`bx <register>` - registerdəki ünvana sıçrayış edir.  
`blx <label>` - thumb və ya arm rejimində kodun ünvanı.  
`cmp` - müqayisə edir.  
`push <register>` - staka data əlavə edir.  
`pop <register>` - stakdan datanı çıxarır.  

