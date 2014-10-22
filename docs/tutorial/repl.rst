Micro Python REPL prompt-la tanışlıq
==================================
REPL = Read Evaluate Print Loop, pyboard daxilində olan interaktiv Micro Python prompt-dur.  
REPL-dən istifadə etmək, kodları test etməyin ən asan yoludur. 
``main.py``-a kod yazmaqdan əlavə REPL-dan istifadə edə bilərsiniz.

REPL-dən istifadə üçün, pyboard-ı USB ilə PC-yə qoşmalısınız.
Bunu necə edəcəyiniz isə, sizin əməliyyat sisteminizdən asılıdır.

Windows
-------
USB device-dan istifadə etmək üçün, pyboard driver-i install etməlisiniz.
Driver pyboard-ın USB flash drayvında yerləşir və onun adı ``pybcdc.inf``-dır.

Driver-i qurmaq üçün, Device Manager-ə gedib, pyboard-ı həmin listdən tapmaq lazımdır.
Pyboard olan yerdə warning işarəsi görəcəksiniz (çünki hələ işləmir),
sağ klik edərək -> Properties -> Install Driver seçin.
Daha sonra, manual olaraq driver-in olduğu yerə gedin və pyboard-ın USB drayvını seçin.
Seçdikdən sonra, yazılmalıdır.  
Drayveri qurduqdan sonra yenidən Device Manager-e gedib install olunmuş pyboard-u tapın
və hansı COM port olduğunu müəyyən edin (məs. COM4).

İndi siz terminal proqramınızdan istifadə etməlisiniz.
HyperTerminal buna nümunə ola bilər. Pulsuz olaraq isə PuTTY-dən istifadə edə bilərsiniz:
[`putty.exe`](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
Serial proqramınızdan istifadə etməklə, müəyyən etdiyiniz COM port-a qoşulmalısınız.
PuTTY ilə, sol paneldə "Session"-a klikləyin, daha sonra, "Serial" radio düymıəsinə basın,
daha sonra da COM portun "Serial Line"-da qeyd edin və ən sonda "Open" düyməsini basın.

Mac OS X
--------

Terminalı acın və aşağıdakını işlədin::

    screen /dev/tty.usbmodem*
    
İşiniz bitdikdə, çıxmaq istəsəniz, CTRL+A CTRL+\\ -nı sıxın.

Linux
-----

Terminalı acın və aşağıdakını işlədin::

    screen /dev/ttyACM0
    
Siz həmçinin, screen əvəzinə ``picocom`` və ya ``minicom``-dan istifadə edə bilərsiniz.
Siz ``/dev/ttyACM1``-dan  və yaxud daha yüksək rəqəmli ``ttyACM``-dən istifadə etməlisiniz.
Həmçinin siz bu device-lardan istifadə etmək üçün müvafiq icazəniz olmalıdır (məs, ``uucp`` və ya ``dialout`` qrupu və ya sudo).


REPL prompt-dan istifadə
---------------------
İndi isə birbaşa pyboard-da Micro Python kod yazmağı sınayaq.
Serial proqramınızı açın (PuTTY, screen, picocom və.s).
Boş screen, həmçinin yanıb sönən cursor görəcəksiniz.
Enter-i basın və siz, Micro Python prompt görəcəksiniz, başqa sözlə ``>>>``.
Düzgün İşlədiyini yoxlamaq üçün sadə kod yazaq: ::

    >>> print("hello pyboard!")
    hello pyboard!
    
``print("hello pyboard!")`` yazdıqdan sonra ENTER-i basdıqda ``hello pyboard!`` mesajını görmüş oluruq.

Əgər siz Python bilirsinizsə, o zaman sadə komandaları test edə bilərsiniz.

Əgər aşağıdakı kodlardan hər hansı biri işləməsə siz hard və ya soft reset edib yenidən sınaya bilərsiniz;
Aşağıya diqqət edək.

Bir neçə əlavə komandaları test edək. Misal üçün: ::

    >>> pyb.LED(1).on()
    >>> pyb.LED(2).on()
    >>> 1 + 2
    3
    >>> 1 / 2
    0.5
    >>> 20 * 'py'
    'pypypypypypypypypypypypypypypypypypypypy'

Board-ın resetlənməsi
-------------------

Əgər nəsə düz getməsə, siz board-ınızı 2 yolla reset edə bilərsiniz.
Birinci üsul CTRL+D-dir. Bu həmçinin soft reset adlanır.
CTRL+D-dən sonra aşağıdakına bənzər mesaj görəcəksiniz ::

    >>> 
    PYB: sync filesystems
    PYB: soft reboot
    Micro Python v1.0 on 2014-05-03; PYBv1.0 with STM32F405RG
    Type "help()" for more information.
    >>>

Əgər bu üsul kömək etməsə hard reset-dən istifadə edə bilərsiniz (Söndürüb-yenidən-yandırmaq).
Bunun üçün RST switch-i basmaq lazımdır(micro-USB soketə ən yaxın olan xırda qara düymə).
Hard reset sizin sessiyanızı sonlandıracaq, hal-hazırda aktiv olan bütün proqramlardan çıxacaq(PuTTY, screen, etc).

Hard reset etməzdən əvvəl, serial proqramlarınızdan çıxmağınız məsləhətdir daha sonra pyboard drive-i çıxarıb yenidən taxa bilərsiniz.
