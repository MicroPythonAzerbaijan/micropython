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
You can override this boot sequence by holding down the user switch as
the board is booting up.  Hold down user switch and press reset, and then
as you continue to hold the user switch, the LEDs will count in binary.
When the LEDs have reached the mode you want, let go of the user switch,
the LEDs for the selected mode will flash quickly, and the board will boot.

The modes are:

1. Green LED only, *standard boot*: run ``boot.py`` then ``main.py``.
2. Orange LED only, *safe boot*: don't run any scripts on boot-up.
3. Green and orange LED together, *filesystem reset*: resets the flash
   filesystem to its factory state, then boots in safe mode.

If your filesystem becomes corrupt, boot into mode 3 to fix it.

Errors: flashing LEDs
---------------------

There are currently 2 kinds of errors that you might see:

1. If the red and green LEDs flash alternatively, then a Python script
    (eg ``main.py``) has an error.  Use the REPL to debug it.
2. If all 4 LEDs cycle on and off slowly, then there was a hard fault.
   This cannot be recovered from and you need to do a hard reset.

