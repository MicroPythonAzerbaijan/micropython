.. _pyb.I2C:

class I2C -- iki kabelli serial protokol.
=======================================

I2C cihazlar arasında əlaqə üçün iki-kabelli(two-wire) protokoldur. Fiziki cəhətdən bu I2C,
iki kabeldən ibarətdir: Saat və tarixə müvafiq olaraq SCL və SDA.

I2C obyektlər yaradılmış iki şinə(bus)təhkim olunur. Onlar yaradıldığı
zaman və ya daha sonra başladıla bilər::

    from pyb import I2C

    i2c = I2C(1)                         # şin(bus) 1-də yarat
    i2c = I2C(1, I2C.MASTER)             # master kimi yarat və başla
    i2c.init(I2C.MASTER, baudrate=20000) # master kimi başla
    i2c.init(I2C.SLAVE, addr=0x42)       # verilən ünvanda(address)də slave(qul) kimi başlat
    i2c.deinit()                         # müvafiq(periferik) olaraq söndür

i2c obektləri print etmək sizə onun confiqurasiyaları haqda məlumat verir.

slave üçün göndərmək və qəbul etmənin sadə metodları:: 

    i2c.send('abc')      # 3 byte göndər
    i2c.send(0x42)       # rəqəm tərəfindən verilən tək bayt(byte) göndər
    data = i2c.recv(3)   # 3 bayt qəbul et receive 3 bytes

yerindəcə qəbul etmək üçün ilk olaraq bytearray(массив байтов, bayt dizisi) yaradılır::

    data = bytearray(3)  # bufer yarat
    i2c.recv(data)       # 3 bayt qəbul et, onları məlumata(data) yaz

siz timeout(zaman aşımı) təyin edə bilərsiniz (millisaniyə şəklində)::

    i2c.send(b'123', timeout=2000)   # 2 saniyədən sonra timeout(zaman aşımı)

master qəbuledicinin ünvanını(address)ini müəyyən etməlidir::

    i2c.init(I2C.MASTER)
    i2c.send('123', 0x42)        # slave-a 0x42 ünvanı(address)i ilə 3 bayt gondər
    i2c.send(b'456', addr=0x42)  # ünvan(address) üçün açar söz(keyword)

Master həmçinin digər metodlara da sahibdir::


    i2c.is_ready(0x42)           # slave 0x42-nin hazır olduğunu yoxla
    i2c.scan()                   # şini(bus) slave-lər üçün scan et. düzgün addreslər siyahısı ilə geri qayıdır
    i2c.mem_read(3, 0x42, 2)     # slave 0x42-nin yaddaşından 3 bayt oxu, slave-dəki 2 ünvandan(address)dən başlayır 
    i2c.mem_write('abc', 0x42, 2, timeout=1000)  # slave 0x42-nin yaddaşına 3 bayt yaz, slave-dəki 2 ünvandan(address)dən başlayır. taymaout 1 san


Konstruktorlar
------------

.. class:: pyb.I2C(bus, ...)

   verilən şində(bus) I2C obyekti quraşdırır.  ``bus`` 1 və ya 2 ola bilər.
   Əlavə parametrlər verilməzsə I2C obyekti yaradılır lakin işə salınmır.
   (Əgər varsa bu şinin(bus) son başladılma parametrləri ola bilər).
   Əgər əlavə argumentlər verilərsə şin(bus) işə salınacaq.
   işəsalma parametrləri üçün ``init`` -ə baxın.
   
   I2C şinlərinin fiziki pin-ləri:
   
     - ``I2C(1)`` X nöqtəsindədir: ``(SCL, SDA) = (X9, X10) = (PB6, PB7)``
     - ``I2C(2)`` Y nöqtəsindədir: ``(SCL, SDA) = (Y9, Y10) = (PB10, PB11)``


Metodlar
-------

.. method:: i2c.deinit()

   Bu metod I2C şinini(bus) söndürür.

.. method:: i2c.init(mode, \*, addr=0x12, baudrate=400000, gencall=False)

   Bu metod I2C şinini(bus) verilən parametrlərdə işə salır:
   
     - ``mode`` həmçinin ``I2C.MASTER`` və ya ``I2C.SLAVE`` olmalıdır
     - ``addr`` 7 bitlik ünvandır(address)dir (yalnız slave üçün işləkdir)
     - ``baudrate`` is the SCL clock rate (yalnız master üçün işləkdir)
     - ``gencall`` ümumi sorğu modunu (general call mode)dəstəkləmək üçündür

.. method:: i2c.is_ready(addr)

   Bu metod I2C cihazını verilən ünvanın(address)in cavab verib vermədyini yoxlayır. Yalnız master modda etibarlıdır.

.. method:: i2c.mem_read(data, addr, memaddr, timeout=5000, addr_size=8)

   Bu metod I2C cihazının yaddaşından oxuyur:
   
     - ``data`` oxunacaq rəqəm və ya buffer ola bilər
     - ``addr`` I2C cihazın addresidır
     - ``memaddr`` I2C cihazındakı yaddaşın yeridir.(ünvanı)
     - ``timeout`` oxumaq üçün verilən fasilə(timeout)(millisaniyələrlə)
     - ``addr_size`` memaddr-ın genişiyidir: 8 və ya 16 bit
   
   Oxunan məlumatları qaytarır.
   Bu yalnız master modda etibarlıdır.

.. method:: i2c.mem_write(data, addr, memaddr, timeout=5000, addr_size=8)

   Bu metod I2C cihazının yaddaşına yazmaq üçün istifadə olunur:
   
     - ``data`` yazılacaq rəqəm və ya buffer ola bilər
     - ``addr`` I2C cihazın addresidır
     - ``memaddr`` I2C cihazındakı yaddaşın yeridir.(ünvanı)
     - ``timeout`` yazmaq üçün gözlənilən vaxt(timeout)(millisaniyələrlə)
     - ``addr_size`` memaddr-ın genişiyidir: 8 və ya 16 bit
   
    ``None`` geri qayıdır.
   Bu yalnız master modda etibarlıdır.

.. method:: i2c.recv(recv, addr=0x00, timeout=5000)

   Bu metod şində(bus) məlumatı qəbul etmək üçündür:
   
     - ``recv`` rəqəm ola bilər,hansıki qəbul ediləcək baytların rəqəmləri ola bilər,
       və ya buffer ola bilər, hansı ki qəbul ediləcək baytlarnan doldurulacaq
     - ``addr`` qəbul ediləcək datanın ünvanı(address) (yalnız master modda tələb olunur)
     - ``timeout`` qəbul olunma üçün gözlənilən vaxt(timeout)(millisaniyələrlə)
   
   qayıdış dəyəri(return value): əgər ``recv`` rəqəmdirsə onda qəbul olunan baytların yeni bufferi olacaq,
   digər halda ``recv``  -də verilən bufferlə eyni buffer olacaq.

.. method:: i2c.scan()

   Bu metod 0x01 ünvanından(address)indən 0x7f addresinə qədər olan bütün I2C ünvanları(address)ləri scan edir və siyahı(list) şəklində geri qaytarır.
   Yalnız master modda etibarlıdır.

.. method:: i2c.send(send, addr=0x00, timeout=5000)

   Bu metod məlumatı(data) şinə(bus) göndərir:
   
     - ``send`` göndəriləcək məlumat (rəqəm və ya buffer obyekti ola bilər)
     - ``addr`` göndəriləcək ünvan(address) (yalnız master modda tələb olunur)
     - ``timeout`` göndərmək üçün millisaniyələrlə verilən timeout
   
   Qayıdış dəyəri(return value): ``None``.


Sabit vahidlər(Constants)
---------

.. data:: I2C.MASTER

   şinin(bus) master modda başladılması üçün istifadə olunur

.. data:: I2C.SLAVE

   şinin(bus) slave modda başladıması üçün istifadə olunur
