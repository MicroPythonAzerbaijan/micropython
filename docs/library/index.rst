Micro Python kitabxanaları
======================

Python standard kitabxanaları
-------------------------

Aşağıdakı standart Python kitabxanaları Micro Python üçün qurulub.

Əlavə kitabxanaları, `micropython-lib repository-dən
<https://github.com/micropython/micropython-lib>`_ yükləyə bilərsiniz.

.. toctree::
   :maxdepth: 1

   cmath.rst
   gc.rst
   math.rst
   os.rst
   select.rst
   struct.rst
   sys.rst
   time.rst

Python micro-kitabxanaları
----------------------

Aşağıdakı standart Python kitabxanaları Micro Python fəlsəfəsinə uyğun olaraq "micro-laşdırılıb".
Onlar bu modulun əsas(core) funksionallığını təmin edir və standart Python kitabxanaları
üçün əvəzetmə olması üçün nəzərdə tutulmuşdur.

Modullar öz u-name-ləri, həmçinin non-u-name-ləri ilə də mövcuddur.
non-u-name package yolunuzda olan eyni adlı fayl ilə əvəz oluna bilər. 
Məsələn, ``import json`` ilk öncə ``json.py``
faylını ya da ``json`` direktivini axtarır və əgər package tapılsa onu yükləyir.
Əgər heç bir şey tapılmasa, ``ujson`` modulunun yüklənməsini dayandırır.

.. toctree::
   :maxdepth: 1

   usocket.rst
   uheapq.rst
   ujson.rst

pyboard-a spesifik kitabxanalar
---------------------------------

Aşağıdakı kitabxanalar pyboard-a spesifikdir.

.. toctree::
   :maxdepth: 2

   pyb.rst
   network.rst
