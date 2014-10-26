Pyboard-ı USB mouse kimi istifadə etmək
=====================================

Pyboard USB devays olduğu üçün, onu həmçinin mouse kimi istifadə edə bilərsiniz.
Buna nail olmaq üçün, USB tənzimləmələri ``boot.py`` faylından dəyişmək lazımdır.
Əgər hələ də, ``boot.py`` faylı ilə işləməməsinizsə o zaman fayl aşağıdakı kimidir ::

    # boot.py -- boot-up zamanı çalışır
    # can run arbitrary Python, but best to keep it minimal

    import pyb
    #pyb.main('main.py') # main script to run after this one
    #pyb.usb_mode('CDC+MSC') # act as a serial and a storage device
    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Mouse mode-u aktivləşdirmək üçün, ən sonuncu sətrdəki comment-i silin. ::

    pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Əgər ``boot.py`` faylını nə vaxtsa dəyişmisinizsə, o zaman minimum kod,
aşağıdakı kimi olacaq ::

    import pyb
    pyb.usb_mode('CDC+HID')

Bu kod, pyboard boot-up olanda özünü CDC (serial) və HID
(human interface device, bizim halda mouse) kimi tənzimləyir.

Pyboard Eject/unmount edin və daha sonra da RST switch ilə reset edin.
İndi sizin PC-niz pyboard-ı mouse kimi görəcək! 

Mouse event-lərin əllə ötürülməsi
----------------------------

py-mouse-un nəsə etməsini təmin etmək üçün, biz PC-yə mouse event-lərini göndərməliyik.
Biz bunu ilk öncə, əllə REPL prompt-dan edəcik.
Pyboard-ınıza serial proqramla qoşulun və aşağıdakını yazın ::

    >>> pyb.hid((0, 10, 0, 0))

Sizin mouse-unuz 10 pixel sağa hərəkət etməlidir!
Yuxarıdakı komandada siz 4 növ məlumatı göndərirsiniz: düymənin statusunu, x, y, scroll.
Göndərdiyiniz 10 rəqəmi x oxu boyunca(yəni sağa) 10 pixel hərəkət etməli olduğunu göstərir. 

Gəlin mouse-ı sağa sola hərəkət etdirək ( oscillate = yellənmək) ::

    >>> import math
    >>> def osc(n, d):
    ...   for i in range(n):
    ...     pyb.hid((0, int(20 * math.sin(i / 10)), 0, 0))
    ...     pyb.delay(d)
    ...
    >>> osc(100, 50)

``osc`` funksiyasına göndərdiyimiz n - mouse event-lərinin sayıdır.
d isə event-lər arasındakı vaxt aralığıdır (gecikməsidir).
Müxtəlif rəqəmlərlə test edib baxa bilərsiniz.

**Tapşırıq: mouse-u dairə cızmağa məcbur edin.**

Akselerometrlə işləyən mouse
-------------------------------------

İndi isə, daha maraqlı bir şey edək.
Akselerometrin köməyi ilə, Pyboard-ın özünün tərpənmə bucağı ilə hərəkət edən mouse düzəldək.
Aşağıdakı kodu biz bir başa REPL-ə, həmçinin ``main.py``-a yaza bilərik.
İndiki halda biz kodu ``main.py``-a yazacıq, həmçinin safe mode-a necə keçəcəyimizi öyrənəcik.

İndiki halda pyboard serial USB device və HİD (mouse) kimi fəaliyyət göstərir.
Bu səbəbdən faylsistemə keçib ``main.py``-ı edit edə bilmirik.

Siz həmçinin ``boot.py`` faylını da edit edə bilmirik ki, HID-mode-dan çıxıb normal moda keçə bilək...

Bu hala çarə kimi, biz *safe mode*-a keçməliyik.
Bu daha əvvəlki dərsdə daha ətraflı göstərilmişdir
[safe mode tutorial](tut-reset), lakin hər ehtimala qarşı burda da qeyd edirik:

1. USR switch-i basılı saxlayın.
2. USR basılı ola-ola, RST switch-i basıb buraxın.
3. LED daha sonra yaşıldan narıncıya keçəcək daha sonra da yaşıl+narıncı olacaq və əvvəlki vəziyyətinə qayıdacaq.
4. USR-i basılı saxlamaqda davam edin *yalnız narıncı LED yanılı qalana kimi*, daha sonra da USR switch-i buraxın.
5. Narıncı LED tez-tez 4 dəfə yanıb-sönəcək, daha sonra isə dayanacaq.
6. Siz artıq Safe mode-dasınız.

Safe mode-da ``boot.py`` və ``main.py`` faylları çalışdırılmır, beləliklə
pyboard default settings-lərlər qalxır (boot-up).
Bu o deməkdir ki, siz faylsistemə daxil ola bilərsiniz (USB drayv görsənəcək) və siz ``main.py`` faylını editləyə bilərsiniz.
(``boot.py`` faylını olduğu kimi saxlayın, çünki biz ``main.py``-ı editlədikdən sonra yenidən HID-mode-a qayıdacıq)

``main.py``-a aşağıdakı kodu yazın ::

    import pyb

    switch = pyb.Switch()
    accel = pyb.Accel()

    while not switch():
        pyb.hid((0, accel.x(), accel.y(), 0))
        pyb.delay(20)

Faylı yaddaşa verin, pyboard drayvı eject/unmount edin və RST switch-lə reset-ləyin.
İndi pyboard-ın hərəkət bucağına uyğun mouse da tərpənəcək.

Bunu sınayın və mouse-u sabit saxlamağa çalışın!

Mouse hərəkətini sonlandırmaq eçen USR switch-i sıxın.

Hiss edəcəksiniz ki, y-oxu tərs olur (rusca: инвертируется).
Bunu düzəltmək üçün,
yuxarıdakı kodda ``pyb.hid()`` sətrindəki y koordinatının əvvəlinə minus yazmaq lazımdır (-).

Pyboard-ı əvvəlki vəziyyətinə qaytarmaq
--------------------------------

Əgər Pyboard-ınızı indiki halı ilə saxlasanız, o hər dəfə qoşulanda özünü mouse kimi aparacaq.
Təbii ki, siz onu əvvəlki halına qaytarmaq istəyəcəksiniz.
Bunu etmək üçün, siz ilk öncə ``safe mode``-a keçməlisiniz, daha sonra ``boot.py`` faylını dəyişməlisiniz.
``boot.py`` faylında ``CDC+HID`` olan sətri kommentləyin. ::

    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Faylı yaddaşa verin, eject/unmount edin və resetləyin.
İndi sizin pyboard öz əvvəlki vəziyyətindədir.

