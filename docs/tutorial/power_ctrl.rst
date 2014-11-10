Enerjiyə nəzarət
=============

:meth:`pyb.wfi` enerji istifadəsini azaltmaq üçün istifadə edə bilərsiz.
Event-i gözləyərkən (məs. interrupt) əlavə enerji istifadə olunur.
Siz bundan aşağıdakı hallarda istifadə edəcəksiniz::

    while True:
        do_some_processing()
        pyb.wfi()

Tezliyə nəzarət üçün istifadə edin :meth:`pyb.freq`::

    pyb.freq(30000000) # set CPU frequency to 30MHz
