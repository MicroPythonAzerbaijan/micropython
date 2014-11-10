Accel sinifi -- akselerometr idarəsi
====================================

Accel akselerometri idarə edən obyektdir.  Nümunə usage::

    accel = pyb.Accel()
    for i in range(10):
        print(accel.x(), accel.y(), accel.z())

Raw(xam) qiymətlər -32 and 31 arasındadır.


Qurucular(konstruktorlar)
------------

.. class:: pyb.Accel()

   Akselerometr obyektini yaradın və qaytarın.
   
   Qeyd: Əgər obyekti yaratdıqdan sonra akselerometr dəyərlərini oxusanız
   0 qiymətini alacaqsınız. İlk nümunənin hazır olması 20ms vaxt tələb
   edir, bu səbəbdən də bu obyektin yaradılması və dəyərlərinin oxunması
   arasında digər başqa kod olana qədər yaradıldıqdan sonra ``pyb.delay(20)`` 
   qoyulmalıdır. Nümunə::
   
       accel = pyb.Accel()
       pyb.delay(20)
       print(accel.x())


Metodlar
-------

.. method:: accel.filtered_xyz()

   x, y və z qiymətlərindən ibarət 3-lü yığım əldə et.

.. method:: accel.tilt()

   Get the tilt register.

.. method:: accel.x()

   x oxunun qiymətini əldə et.

.. method:: accel.y()

   y oxunun qiymətini əldə et.

.. method:: accel.z()

   z oxunun qiymətini əldə et.
