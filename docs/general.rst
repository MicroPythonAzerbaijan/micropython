Pyboard haqqında ümumi məlumat
=====================================

Lokal faylsistem və SD kart
----------------------------

Pyboard-da ``/flash`` adlanan daxili faylsistem var (drive).
``/flash`` mikro kontrollerin flash yaddaşında saxlanılır.
Əgər mikro SD kart slot-a daxil edilibsə bu ``/sd`` kimi görsənəcək.

Pyboard boot-up olduqda, hansı fayl sistemdən boot olacağını seçməlidir.
Əgər SD kart yoxdursa, o zaman o, daxili faylsistem olan ``/flash``-i seçəcək,
əks təqdirdə isə  SD kartı.

(Qeyd edək ki, köhnə board versiyalarından, ``/flash``-in adı ``0:/`` və ``/sd``-nin adı ``1:/``-dir.)

Boot faylsistemi 2 məqsəd üçün istifadə olunur: ``boot.py`` və ``main.py``
faylları bu faylsistemdə axtarılır və bu faylsistem USB kabeli PC-yə qoşduqda görünən faylsistemdir.

Faylsistem PC-də USB flash drayv kimi görsənəcək.
Siz drayvda faylları saxlaya bilərsiniz, ``boot.py`` və ``main.py`` fayllarını
dəyişə bilərsiniz.


*Pyboard-ınızı reset-ləməzdən əvvəl onu eject etməyi unutmayın(Linux-da unmount).*

Boot tipləri (boot modes)
----------

Əgər normal şəkildə pyboard-ı power up etmisizsə, pyboard standart mode-la boot olacaq:
ilk öncə ``boot.py`` çalışdırılacaq daha sonra USB tənzimlənəcək və ``main.py`` işləyəcək.

Siz bu boot ardıcıllığını user switch-i (USR) basıb saxlamaqla dəyişə bilərsiniz. 
USR switch-i sıxıb saxlayın, LED binary-də (ikili say sistemində) saymağa başlayacaq.
Nə vaxt ki, LED-lər siz istəyən mode-a gəlib çatır, o zaman USR switch-i buraxın.
Daha sonra görəcəksiniz ki, müvafiq mode-un LED-i tez-tez yanıb sönəcək və board boot olacaq.

Mode-la aşağıdakılardır:

1. Yalnız yaşıl LED, *standard boot*: ``boot.py`` daha sonra ``main.py`` çalışdırılacaq.
2. Yalnız narıncı LED, *safe boot*: boot-up zamanı heç bir script-i işlətməyəcək.
3. Yaşıl və narıncı LED birlikdə, *filesystem reset*: flash faylsistemi reset edir, fabrika tənzimləmələrinə geri qaytarır
və safe mode-a boot edir.

Əgər sizin faylsisteminiz korrupt olubsa, bu halı düzəltmək üçün 3-cü mode-a boot olun.

Errorlar: yanıb sönən LED-lər
---------------------

İndiki halda siz 2 növ error görə bilərsiniz:

1. Əgər qırmız və yaşıl LED-lər ardıcıl olaraq yanıb sönürsə, o zaman Python script-də problem var
(məsələn, ``main.py``). REPL-dən istifadə edib kodunuzu debug edə bilərsiniz. 

2. Əgər 4 LED-in hər biri yavaş-yavaş yanıb sönürsə deməli nə isə ciddi bir problem var.
Bunu həll etmək üçün isə hard reset edin.

