.. _pyb.DAC:

class DAC -- rəqəmsaldan analoga çevirici
=========================================

DAC analoq dəyərlərin pin X5 və ya pin X6 -da ixrac olunmasında(output) (təyin olunmuş gərginlikdə) istifadə olunur.
Gərginlik(voltage) 0 və 3.3V aralıqda ola bilər

*Bu modul API dəyişikliklərinə məruz qala bilər.*

Nümunə::

    from pyb import DAC

    dac = DAC(1)            # pin X5-də DAC 1 yarat
    dac.write(128)          # dəyəri DAC-a yaz(X5-i 1.65V edir)
    
Davamlı sinus dalğası ixrac(output) etmək üçün::

    import math
    from pyb import DAC

    # sinus dalğasının aid oldugu bufer yarat
    buf = bytearray(100)
    for i in range(len(buf)):
        buf[i] = 128 + int(127 \* math.sin(2 \* math.pi \* i / len(buf)))

    # 400Hz -də sinus dalğasını ixrac(output) et
    dac = DAC(1)
    dac.write_timed(buf, 400 \* len(buf), mode=DAC.CIRCULAR)


Konstruktorlar
------------

.. class:: pyb.DAC(port)

   Bu kostruktor yeni DAC obyekti quraşdırır.
   
   ``port`` pin obyekti və ya ədəd(tam ədəd) ola bilər(1 və ya 2).
   DAC(1) X5 pin-dədir və DAC(2) pin X6-da dır.

Metodlar
-------

.. method:: dac.noise(freq)

   Bu metod psevdo-random(yarı-təsadüfilik) səs siqnalı yaradır. Yeni random(təsadüfi) sample DAC-a yazılır və
   verilən tezlikdə ixrac(output) edilir.
   
.. method:: dac.triangle(freq)

   Bu metod üçbucaq dalğa yaradır. DAC-ın ixrac dəyəri verilən tezliklərə görə dəyişir və
   üçbucaq dalğanın özünü təkrarlama tezliyi 256(və ya 1024ç yoxlamaq lazımdır) dəfə daha kiçikdir.
   

.. method:: dac.write(value)

   DAC ixracına birbaşa giriş(hal hazırda yalnız 8 bit).

.. method:: dac.write_timed(data, freq, \*, mode=DAC.NORMAL)

   Bu metod DMA transferdən istifadə edərək yaddaşın(RAM) DAC-a çıxışı üçündür.
   Daxil olunmuş məlumat(input data) baytlar(8 bitlik məlumat) şəklində emal olunur.
   
   
   ``mode`` ola bilər: ``DAC.NORMAL`` və ya ``DAC.CIRCULAR``.
   
   TIM6 köcürmənin tezliyini idarə etmək üçün istifadə olunur.