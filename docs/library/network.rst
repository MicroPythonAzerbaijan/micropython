****************************************
:mod:`network` --- şəbəkə konfiqurasiyası
****************************************

.. module:: network
   :synopsis: network configuration

Bu modul şəbəkə drayverlərini və marşrutlaşdırma konfiqurasiyasını təmin edir.


CC3k sinifi
==========

Konstruktorlar
------------

.. class:: CC3k(spi, pin_cs, pin_en, pin_irq)

   Verilmiş SPİ şin və pinlərlə CC3000-u başladın(rus. инициализировать) və CC3k obyektini qaytarın.


Metodlar
-------

.. method:: cc3k.connect(ssid, key=None, \*, security=WPA2, bssid=None)


WIZnet5k sinifi
==============

Bu sinif sizə W5200 və W5500 çipsetlərinə əsaslanan WIZnet5x00 Ethernet adapterlərini
idarə etməyə icazə verir (təkcə W5200 test edilib).

Nümunə::

    import wiznet5k
    w = wiznet5k.WIZnet5k()
    print(w.ipaddr())
    w.gethostbyname('micropython.org')
    s = w.socket()
    s.connect(('192.168.0.2', 8080))
    s.send('hello')
    print(s.recv(10))


Qurucular(konstruktorlar)
------------

.. class:: WIZnet5k(spi, pin_cs, pin_rst)

   WIZnet5k obyektini yaradın və qaytarın.


Metodlar
-------

.. method:: wiznet5k.ipaddr([(ip, subnet, gateway, dns)])

   IP adresi, subnet mask(alt ağ maskesi), gateway(шлюз) və DNS əldə et/quraşdır.

.. method:: wiznet5k.regs()

   WIZnet5k registerlərini dump(свалка, zibillik) et.
