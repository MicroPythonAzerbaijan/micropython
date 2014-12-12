class LCD -- LCD pyskin touch-sensoru üçün LCD-nin idarə edilməsi
========================================================

LCD sinfi, LCD32MKv1.0 modelli LCD touch-sensor pyskin-i də LCD-ni idarə etmək üçündür.
Bu LCD -NHD-C12832A1Z- 128x32 pikselli, monoxrom(təkrəngli) ekrandır.

pyskin ya X ya da Y koordinatlarına bağlı olmalıdır. LCD obyekt yaratmaq üçün::

    lcd = pyb.LCD('X')      # Əgər pyskin X nöqtəsindədirsə
    lcd = pyb.LCD('Y')      # Əgər pyskin X nöqtəsindədirsə

Daha sonra istifadə edə bilərsiniz::

    lcd.light(True)                 # arxa işığı(backlight) yandırır
    lcd.write('Hello world!\n')     # Mətni ekrana yazdır(print)

Bu sürücü(driver) setting/getting üçün iki dəfə buffer əməliyyatını yerinə yetirir.
Məsələn əgər siz sıçrayan(tullanan) nöqtə yaratmaq istəyirsinizsə, belə edə bilərsiniz::

    x = y = 0
    dx = dy = 1
    while True:
        # nöqtənin yerini təzələyir(update)
        x += dx
        y += dy

        # ekranın qırağında sıçrayan nöqtə yaradır
        if x <= 0 or x >= 127: dx = -dx
        if y <= 0 or y >= 31: dy = -dy

        lcd.fill(0)                 # bufferi təmizləyir
        lcd.pixel(x, y, 1)          # nöqtə çəkir
        lcd.show()                  # bufferi göstərir
        pyb.delay(50)               # 50 ms-lik(millisaniyə) fasilə verir


Konstruktorlar
------------

.. class:: pyb.LCD(skin_position)

   Bu sinif verilən kordinatlarda LCD obyekt yaradır.  ``skin_position`` 'X' və ya 'Y' ola bilər, və
   LCD pyskin kordinatları ilə uyğun olmalıdır.


Metodlar
-------

.. method:: lcd.command(instr_data, buf)

   Bu metod ixtiyari bir əmri LCD-yə göndərir.göstərişlər üçün  ``instr_data`` -nı 0-a keçirin, məlumatları ötürmək üçün isə 1-ə keçirin.
   ``buf`` göstəriş və ya dataların ötürülməsi üçün bufferdir.

.. method:: lcd.contrast(value)

   Bu metod LCD-nin kontrastını təyin etmək üçündür.  Düzgün dəyərlər 0 və 47 aralığıdır.

.. method:: lcd.fill(colour)

   Ekranı verilən rəngdə etmək üçündür.(qara və ağ üçün 1 və 0 ).
   
   Bu metod gizli bufferə yazır.  Bufferi göstərmək üçün ``show()`` əmrindən istifadə edin.

.. method:: lcd.get(x, y)

    Bu metodla ``(x, y)`` kordinatlarında pikseli əldə edirsiniz.  cavab olaraq 1 və ya 0 qayıdır.
   
    Bu metod görünən bufferdən oxuyur.

.. method:: lcd.light(value)

   backlight-ı(arxaişıq) söndürüb yandırmaq üçündür.  True və ya 1 yandırır, False və ya 0 söndürür.

.. method:: lcd.pixel(x, y, colour)

    ``(x, y)`` kordinatlarında pikseli verilən rəngə çevirir (0 və ya 1).
   
   Bu metod gizli bufferə yazır.  Bufferi göstərmək üçün ``show()`` əmrindən istifadə edin.

.. method:: lcd.show()

   Gizli bufferi ekranda göstərir.

.. method:: lcd.text(str, x, y, colour)

   ``(x, y)`` koordinatlarına verilən mətni verilən rəngdə yazır (0 və ya 1).
   
   Bu metod gizli bufferə yazır.  Bufferi göstərmək üçün ``show()`` əmrindən istifadə edin.

.. method:: lcd.write(str)

    ``str`` verilən sətiri(string,dizi) ekrana yazır.  Bu əmr verilənləri dərhal ekrana yazır.
