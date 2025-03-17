# Yolun Kənarına Çıxma (Path Traversal) Texnikaları  

Həmişə həm irəli, həm də geri əyrixətlərdən istifadə etməklə yolun kənarına çıxma (**path traversal**) ardıcıllıqlarını sınaqdan keçirin. Çünki bir çox giriş filtr sistemi yalnız birini yoxlayır, halbuki fayl sistemləri hər ikisini dəstəkləyə bilər.  

## Sadə URL-kodlaşdırılmış ifadələrdən istifadə edin  
Bütün girişlərinizdəki hər bir nöqtə və əyrixəti kodlaşdırdığınıza əmin olun.  

| Simvol | URL-kodlaşdırılmış versiya |
|--------|-----------------------------|
| `.` (nöqtə) | `%2e` |
| `/` (irəli əyrixət) | `%2f` |
| `\` (geri əyrixət) | `%5c` |

## 16-bit Unicode kodlaşdırma  

| Simvol | Unicode kodlaşdırılmış versiya |
|--------|-----------------------------|
| `.` (nöqtə) | `%u002e` |
| `/` (irəli əyrixət) | `%u2215` |
| `\` (geri əyrixət) | `%u2216` |

## İkiqat URL-kodlaşdırma (Double Encoding)  

| Simvol | İkiqat kodlaşdırılmış versiya |
|--------|-----------------------------|
| `.` (nöqtə) | `%252e` |
| `/` (irəli əyrixət) | `%252f` |
| `\` (geri əyrixət) | `%255c` |

## Uzun UTF-8 Unicode kodlaşdırma  

| Simvol | UTF-8 kodlaşdırılmış versiya |
|--------|-----------------------------|
| `.` (nöqtə) | `%c0%2e`, `%e0%40%ae`, `%c0ae` və s. |
| `/` (irəli əyrixət) | `%c0%af`, `%e0%80%af`, `%c0%2f` və s. |
| `\` (geri əyrixət) | `%c0%5c`, `%c0%80%5c` və s. |

## Təkrar-təkrar filtrləri yan keçmək  
Əgər tətbiq filtr sistemini sırf traversal ardıcıllıqlarını silməklə təmizləməyə çalışırsa və bunu rekursiv etmirsə, o zaman filtr sistemini yan keçmək mümkün ola bilər. Bunun üçün bir ardıcıllığı digərinin daxilində qoyaraq test edin:  

```
....//
....\/ 
..../\ 
....\\ 
```

Bu metodlar əksər hallarda filtr sistemlərini yan keçmək üçün effektiv ola bilər. Test edərkən mümkün olan bütün variantları yoxlamaq vacibdir.
