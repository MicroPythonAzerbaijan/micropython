.. _pyb.LED:

class LED -- LED obyekti
========================

LED obyekti individual LED-i (Light Emitting Diode) idarə edir.


Konstruktorlar
--------------

.. class:: pyb.LED(id)

   Verilmiş LED-lə əlaqəli olan LED obyekti yaradın:
   
     - ``id`` LED nömrəsidir, 1-4.


Metodlar
--------

.. method:: led.intensity([value])

   LED intensivliyini əldə edin və yaxud təyin edin.  İntensivlik intervalı 0-la (sönmüş) 255 (tam qoşulu) arasındadır.
   Arqument verilmədiyi halda LED intensivliyinin qiymətini qaytarın.
   Əgər arqument verilsə, LED intensivliyini təyin edin və ``None`` qaytarın.

.. method:: led.off()

   LED-i söndürün.

.. method:: led.on()

   LED-i yandırın.

.. method:: led.toggle()

   LED-i "sönmüş" və "qoşulu" halları arasında dəyişin.
