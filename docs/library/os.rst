:mod:`os` -- ilkin(təməl)  "operating system" (əməliyyat sistemi) servisləri.
==============================================

.. module:: os
   :synopsis: ilkin(təməl)  "operating system" (əməliyyat sistemi) servisləri

``os`` modulu fayl sistemlə işləməyə imkan verən funksiyaları özündə birləşdirir, həmçinin ``urandom``.


Pyboard spesifikasıyaları
-----------------

Pyboard-da fayl sistem root direktoriyası ``/``-dır və bütün fiziki drayvlara buradan keçmək mümkündür.
İndiki halda onlar aşağıdakılardır:

    ``/flash``      -- daxili flash fayl sistemi

    ``/sd``         -- SD kart (əgər qoşuludursa)

Boot olduqda (start olduqda) SD kart daxil edilibsə, ``/sd``-dən əgər olmayıbsa, ``/flash``-dən başlayır (boot olur).


Funksiyalar
---------

.. function:: chdir(path)
    
   chdir = Change Directory.
   İşlək direktoriyanı dəyişmək (Linux-da tanıdığımız CD komandası).

.. function:: getcwd()

   getcwd = Get Current Working Directory.
   Hal-hazırda işlək olan direktoriyanı qaytarır.

.. function:: listdir([dir])

   listdir = list directory.
   Arqumentsiz olaraq çağrıldıqda hal-hazırkı işlək direktoriya daxilində bütün faylları göstərir.
   Arqumentlə çağrıldıqda isə, verilmiş direktoriya daxilində bütün faylları göstərir.

.. function:: mkdir(path)
   
   mkdir = make directory.
   Yeni direktoriya (qovluq, folder) yaratmaq.

.. function:: remove(path)

   Verilmiş faylı silmək.

.. function:: rmdir(path)
   
   rmdir = remove directory.
   Verilmiş direktoriyani silmək.
   
.. function:: stat(path)
   
   Faylın və yaxud direktoriyanın statusunu götürmək üçündür.
   Linux-da stat əmri.

.. function:: sync()
   
   Bütün fayl sistemləri sinxronlaşdırır.
   
.. function:: urandom(n)

   Bayt obyektini n random (təsadüfi) baytla qaytarır.
   Random rəqəm Hardware random (təsadüfi) rəqəm generatoru tərəfindən yaradılır.

Konstantlar
---------

.. data:: sep

   separation character used in paths - Tərcüməyə ehtiyac duyulmayan yer.
