:mod:`time` -- zaman ilə bağlı funksiyalar
=====================================

.. module:: time
   :synopsis: time related functions

``time`` modulu cari vaxt və tarixi əldə etmək və gözləmə(pauza) üçün funksiyaları
təmin edir.

Funksiyalar
---------

.. function:: localtime([secs])

   Zaman ifadəsini 1 Yanvar 2000-dən bu yana 8-li yığımlar şəklində saniyələrdə
   çevirir, hansı ki, (il, ay, gün, saat, dəqiqə, saniyə, həftə günü, il günü)
   təşkil edir. Əgər saniyələr təmin olunmayıbsa və ya yoxdursa, onda cari vaxt
   istifadə olunmuş RTC-dən. il əsrləri təşkil edir (məsələn 2014).

   * ay 1-12
   * ay günü 1-31
   * saat 0-23
   * dəqiqə 0-59
   * saniyə 0-59
   * həftə günü 0-6 (Bazar ertəsi - Bazar)
   * il günü 1-366

.. function:: mktime()

   Bu localtime funksiyasının tərsidir. Onun arqumenti tam dolu 8-li yığımdır
   hansı ki, zamanı hər bir yerli vaxt kimi ifadə edir. O, tam ədəd kimi
   1 Yanvar 2000-dən bu yana saniyələrin sayını qaytarır.

.. function:: sleep(seconds)

   Gözləmə üçün verilən saniyələrin sayı.  Saniyə kəsr şəklində də verilə bilər.
   Bu zaman kəsr ədədinin kəsr hissəsində göstərilmiş qədər saniyə gözləmə 
   təmin olunacaq.

.. function:: time()

   1/1/2000 tarixindən bu yana tam ədəd kimi saniyələrin sayını qaytarır.
