class CAN -- controller area network(beynəlxalq standart olduğu üçün tərcümə olunmur) communication bus.
Ətraflı baxın: http://en.wikipedia.org/wiki/CAN_bus
======================================================

Standart CAN communication protokolu dəstəkləyir.
CAN implements the standard CAN communications protocol.
Fiziki səviyyədə, bu, 2 pillədən ibarətdir: RX və TX.
Qeyd edək ki, pyboard-ı CAN bus-a (şinə) qoşmaq üçün CAN transceiver-dən istifadə etməlisiniz.
CAN transceiver - pyboard-dan gələn CAN məntiqi ilə işləyən siqnalları, şinə uyğun voltaj-a çevirir.

# Tərcümə edilməyə ehtiyac olan hissə
Note that this driver does not yet support filter configuration
(it defaults to a single filter that lets through all messages),
or bus timing configuration (except for setting the prescaler).
#

İstifadə nümunəsi(heç bir şey qoşulmamış da işləyir)::

    from pyb import CAN
    can = pyb.CAN(1, pyb.CAN.LOOPBACK)
    can.send('message!', 123)   # 123 id-yə mesaj göndər.
    can.recv(0)                 # FIFO 0-da mesaj qəbul etmək.


Konstruktorlar
------------

.. class:: pyb.CAN(bus, ...)

   Verilmiş bus-da CAN obyektini konstruk et. ``bus`` 1-2 ola bilər və yaxud, 'YA' və yax 'YB'.
   Əlavə parameter göstərilmədən, CAN obyekti yaradılır lakin inisializasiya olunmur
   (it has the settings from the last initialisation of the bus, if any).
   Əgər əlavə arqumentlər verilərsə, bus inisializasiya olunur.
   initialisation parametrləri üçün ``init``-ə baxın. 
   
   CAN şinlərinin fiziki pinləri aşağıdakılardır:
   
     - ``CAN(1)`` = ``YA``: ``(RX, TX) = (Y3, Y4) = (PB8, PB9)``
     - ``CAN(2)`` = ``YB``: ``(RX, TX) = (Y5, Y6) = (PB12, PB13)``


Metodlar
-------

.. method:: can.init(mode, extframe=False, prescaler=100, \*, sjw=1, bs1=6, bs2=8)

   CAN şinini (bus) verilmiş parametrlərlə inisializasiya edir:
   
     - ``mode`` bunlardan biridir:  NORMAL, LOOPBACK, SILENT, SILENT_LOOPBACK

   Əgər ``extframe`` True-dursa o zaman şin freymlərdə artırılmış(extended) identifikatorlardan istifadə edir (29 bits).
   Əks halda, standart 11 bitlik identifikatorlardan istifadə edir.

.. method:: can.deinit()

   CAN şinini söndürür.

.. method:: can.any(fifo)
   
   FİFO-da mesaj gözləyirsə, ``True`` qaytarır, əks halda ``False``.
   

.. method:: can.recv(fifo, \*, timeout=5000)

   Bus-da olan məlumatı almaq:
   
     - ``fifo``    is an integer, which is the FIFO to receive on.
     - ``timeout`` is the timeout in milliseconds to wait for the receive.
   
   Geri dönən məlumat: buffer of data bytes.

.. method:: can.send(send, addr, \*, timeout=5000)

   BUS-a (şinə) məlumat göndərmək:
   
     - ``send`` Göndəriləcək data(integer və yaxud buffer obyekti).
     - ``addr`` Hansı addresə göndəriləcək.
     - ``timeout``  Göndərmək üçün, timeout qiyməti. Qiymət milli saniyədə göstərilir.
   
   Geri dönən məlumat: ``None``.


Konstantlar
---------

.. data:: CAN.NORMAL
.. data:: CAN.LOOPBACK
.. data:: CAN.SILENT
.. data:: CAN.SILENT_LOOPBACK

   CAN şininin mode-nu təyin edən konstantlar.
