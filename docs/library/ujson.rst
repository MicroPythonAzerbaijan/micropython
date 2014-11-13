:mod:`ujson` -- JSON şifrələnməsi və deşifrələnməsi
==========================================

.. module:: ujson
   :synopsis: JSON encoding and decoding

Bu modul Python obyektləri ilə JSON məlumat formatlarını bir-biriləri
arasında çevirməyə icazə verir.

Funksiyalar
---------

.. function:: dumps(obj)

   JSON string-i kimi təmsil edilən ``obj`` obyektini qaytarır.

.. function:: loads(str)

   
   JSON ``str`` təhlil edin və obyekt qaytarın.  Əgər string düzgün formalaşdırılmayıbsa
   ValueError artırılır.
