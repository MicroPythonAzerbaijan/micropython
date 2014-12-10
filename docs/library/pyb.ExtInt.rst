.. _pyb.ExtInt:

class ExtInt -- I/O pinləri xarici əməliyyatları dayandırmaq üçün tənzimləyir.
==================================================================

Burada cəmi 22 dayandırıcı(interrupt) sıra(cərgə,line) vardır. Onlardan 16-sı GPIO pinləri vaitəsilə yerinə yetirilə bilər, digər 6 sıra(cərgə,line) isə daxili mənbələr üçün
saxlanılır.

0-15 arası sıralardan verilən sıra ixtiyari bir portdan müvafiq bir sıraya gedən yolu göstərə bilər.
beləliklə sıra 0, Px0-a, A,B,C....-də x-in harda olduğunu göstərə bilər və
sıra 1 Px1-ə A, B, C....-də x-in harda olduğunu göstərə bilər.::

    def callback(line):
        print("line =", line)

Qeyd: ExtInt avtomatik olaraq verilən gpio sırasını tənzimləyəcək.::

    extint = pyb.ExtInt(pin, pyb.ExtInt.IRQ_FALLING, pyb.Pin.PULL_UP, callback)

İndi, hər dəfə qırağa düşən kənar pin X1 də olanda geri callback(geri çağırmaq), işə düşəcək.

Diqqət: Mexaniki düymələrdə(pushbutton) sıçrama(bounce) var və switchi işə salanda çoxlu sayda dalğalar yaradır.
Debouncing haqqında daha ətraflı məlumat üçün: http://www.eng.utah.edu/~cs5780/debouncing.pdf linkə baxa bilərsiniz.

eyni pin-ə iki callback əməliyyatını yazmaq istisna hala səbəb ola bilər.
Əgər pin rəqəm kimi keçsə onda o, daxili dayandırma(interrupt) əməliyyatı hesab edilir və 16-22 aralıqda olmalıdır.

Digər bütün pin ogyektlər pin mapper-dən keçir və gpio pinlərdən biri ilə birləşir.::

    extint = pyb.ExtInt(pin, mode, pull, callback)

Əsaslandırılmış modlar: pyb.ExtInt.IRQ_RISING, pyb.ExtInt.IRQ_FALLING,
pyb.ExtInt.IRQ_RISING_FALLING, pyb.ExtInt.EVT_RISING,
pyb.ExtInt.EVT_FALLING, and pyb.ExtInt.EVT_RISING_FALLING.

Yalnız IRQ_xxx modları test olunub. EVT_xxx modları, sleep mod ve WFE göstərişləri ilə əlaqəlidir.

Düzgün pull dəyərləri: pyb.Pin.PULL_UP, pyb.Pin.PULL_DOWN, pyb.Pin.PULL_NONE.

Həmçinin burda C API vardır, belə ki EXTI dayandırma cərgələrini tələb edən drayverlər də bu koddan
istifadə edə bilər. Mövcud funksiyalar üçün extint.h faylına baxın. Nümunə üçün usrsw.h faylına baxın.


Konstruktorlar
------------

.. class:: pyb.ExtInt(pin, mode, pull, callback)

   Bu sinif(class) ExtInt obyekti yaradır:
   
     - ``pin`` dayandırma əməliyyatıarının aktiv edildiyi pindir.(pin obyekti və ya hansısa doğru bir pinin adı ola bilər).
     - ``mode`` aşağıdakılardan biri ola bilər:
       - ``ExtInt.IRQ_RISING`` - yüksələn tərəfi(qırağı, edge) başladır;
       - ``ExtInt.IRQ_FALLING`` - düşən tərəfi(qırağı, edge) başladır;
       - ``ExtInt.IRQ_RISING_FALLING`` - qalxan və ya düşən tərəfi(qırağı,edge) başladır.
     - ``pull`` aşağıdakılardan biri ola bilər:
       - ``pyb.Pin.PULL_NONE`` - qalxan və ya düşən müqavimət yoxdur;
       - ``pyb.Pin.PULL_UP`` - qalxan(yüksələn, pull-up) müqaviməti aktiv edir;
       - ``pyb.Pin.PULL_DOWN`` - düşən müqaviməti aktiv edir.
     - ``callback`` saxlama əməliyyatını başlatmaq üçün triggerləri çağıran funksiyadır.
       callback funksiyası 1 argument qəbul edir: Bu funksiyada dayandırma əməliyyatı üçün
       trigger rolunu oynayacaq cərgə(sıra, line) göstərilir.


sinfi metodlar(Class methods)
-------------

.. method:: ExtInt.regs()

   Bu metod, dəyərləri EXTI reestrlərinə(registers) atır. Dump the values of the EXTI registers.


Metodlar
-------

.. method:: extint.disable()

   ExtInt obyekti ilə əlaqəli olan kəsmə(dayandırma, interrupt) əməliyyatını söndürür. Disable the interrupt associated with the ExtInt object.
   Bu, debouncing üçün yararlı ola bilər.

.. method:: extint.enable()

   Dayandırılmış saxlama əməliyyatını aktiv edir.

.. method:: extint.line()

   Yol göstərilən pinin(mapped pin)cərgə(sıra,line)nömrəsini göstərir.

.. method:: extint.swint()

   Programdan callback funksiyasını başladır.


Sabit vahidlər(Constants)
---------

.. data:: ExtInt.IRQ_FALLING

   düşən kənarı(edge) dayandırır.

.. data:: ExtInt.IRQ_RISING

   yüksələn(qalxan) kənarı(edge) dayandırır

.. data:: ExtInt.IRQ_RISING_FALLING

   düşən və ya yüksələn(qalxan) kənarı(edge) dayandırır
