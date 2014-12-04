.. _pyb.ADC:

class ADC -- analogdan rəqəmsala çevirici: Pindəki analog dəyərləri oxuyur
======================================================================

İstifadə qaydası::

    import pyb

    adc = pyb.ADC(pin)              # pindən analoq obyeklər yarat
    val = adc.read()                # analoq dəyəri oxu

    adc = pyb.ADCAll(resolution)    # ADCAll obyekt yarat
    val = adc.read_channel(channel) # verilən kanalı oxuyur
    val = adc.read_core_temp()      # MCU temperaturu oxuyur
    val = adc.read_core_vbat()      # MCU VBAT-ı oxuyur
    val = adc.read_core_vref()      # MCU VREF-i oxuyur


Konstruktorlar
------------

.. class:: pyb.ADC(pin)

   Verilən pinlə bağlı ADC obyekt yaradır.
   Bu sizə daha sonra həmən pindən analog dəyərləri oxumağa
   icazə verir.


Metodlar
-------

.. method:: adc.read()

   Bu metod Analoq pindəki dəyərləri oxuyur və geri qaytarır.
   Geri qaytarılan dəyərlər 0 və 4095 aralığında olur.

.. method:: adc.read_timed(buf, freq)

   Bu metod verilən bufferdən analoq dəyərləri verilən tezlikdə oxuyur.
   Məsələn bufer bytearray və ya array.array ola bilər. Əgər bufer 8-bit elementlərlə
   istifadə olunubsa örnək nəticə 8 bit-ə düşürüləcək
   
   
   Nümunə::
   
       adc = pyb.ADC(pyb.Pin.board.X19)    # X19-cu pində ADC yaradır
       buf = bytearray(100)                # 100 baytlıq buffer yaradır
       adc.read_timed(buf, 10)             # bufferdən 10Hz tezlikdə analoq dəyərləri oxuyur
                                           #  əməliyyat 10 saniyəyə bitəcək
       for val in buf:                     # bütün dəyərləri dövr edir
           print(val)                      # dəyəri print(ekrana yazdırır) edir
   
   Bu funksiya yaddaşdan (RAM) istifadə etmir.
