# Commix

Commix (Command Injection Exploiter) — Linux və ya UNIX sistemlərində işləyən command injection (əmr inyeksiyası) boşluqlarını avtomatik aşkarlayan və istismar edən məşhur açıq mənbə (open-source) təhlükəsizlik alətidir. 

Aşağıdakı əmrlə Commix-i sadə bir test üçün işə sala bilərsən:  
`commix --url="http://target.com/page.php?param=value"`

Əgər POST sorğusu test olunacaqsa:  
`commix --url="http://target.com/login.php" --data="username=test&password=test"`

Əgər cookie və ya header ilə test aparmaq istəsən:  
`commix --url="http://target.com/page.php" --cookie="session=abc123"`
