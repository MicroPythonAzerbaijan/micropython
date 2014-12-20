.. _pyb.Pin:

class Pin -- I/O pinləri idarə etmək üçündür.
=============================

pin, I/O pinləri idarə etmək üçün əsas obyektdir.  Pinin rejimlərini(giriş, çıxış, və.s) təyin etmək üçün
öz metodları var. Həmçinin rəqəmsal məntiqi səviyyədə get/set metodlara malikdir.
Analog idarəetmə üçün ADC sinfinə(ADC class) baxın.

İstifadə modeli:

Bütün lövhə(board) pinlər əvvəldən pyb.Pin.board.Name kimi təyin olunmuşdur ::

    x1_pin = pyb.Pin.board.X1

    g = pyb.Pin(pyb.Pin.board.X1, pyb.Pin.IN)

board pinlərə uyğun CPU pinlər ``pyb.cpu.Name`` kimi mövcuddur .
PYBv1.0 -də, ``pyb.Pin.board.X1`` və
``pyb.Pin.cpu.B6`` eyni pinlərdir.

siz həmçinin string-lərdən də istifadə edə biləriniz::

    g = pyb.Pin('X1', pyb.Pin.OUT_PP)

İstifadəçilər həmçinin öz adlarını da əlavə edə bilərlər(pinləri adlandıra bilərlər)::

    MyMapperDict = { 'LeftMotorDir' : pyb.Pin.cpu.C12 }
    pyb.Pin.dict(MyMapperDict)
    g = pyb.Pin("LeftMotorDir", pyb.Pin.OUT_OD)

və lüğətdən istədikləri veriləni çağıra bilərlər::

    pin = pyb.Pin("LeftMotorDir")

İstifadəçilər həmçinin öz mapping funksiyalarını əlavə edə bilərlər::

    def MyMapper(pin_name):
       if pin_name == "LeftMotorDir":
           return pyb.Pin.cpu.A0

    pyb.Pin.mapper(MyMapper)

Belə ki, əgər siz: ``pyb.Pin("LeftMotorDir", pyb.Pin.OUT_PP)`` adlandırsanız
onda ``"LeftMotorDir"`` birbaşa mapper funksiyaya keçəcək.

Ümumilikdə, aşağıdakı qaydalar bir sıra pin nömrələrə qoşulmanı təyin edir:

1. Birbaşa pin obyekti təyin etmək.
2. İstifadəçi tərəfindən verilən mapping funksiyası.
3. İstifadəçi tərəfindən təyin olunan mapping funksiyası(obyekt dictionary key(lüğət açarı) kimi istifadə oluna bilməlidir).
4. lövhə pin(board pin) ilə uyğun olan string təyin etmək.
5. CPU port/pin ilə uyğun olan string təyin etmək.

Xüsusi obyektlərin pinə necə qoşulduğu haqda bəzi debug informasiyalar almaq üçün siz ``pyb.Pin.debug(True)`` edə bilərsiniz.

Pin ``Pin.PULL_UP`` və ya ``Pin.PULL_DOWN`` pull-mode aktiv olanda,
o pin effektiv olaraq 40k Ohm müqavimətə malik olur və onu 3V3 -a və ya GND-yə müvafiq olaraq qaldırır.

Konsruktorlar
------------

.. class:: pyb.Pin(id, ...)

   id ilə bağlı yeni pin obyekt yaradır.  Əgər əlavə arqumentlər verilərsə onlar pini başlatmaqda istifadə olunur.
   Bax: :meth:`pin.init`


sinfi(class) metodlar
-------------

.. method:: Pin.af_list()

   bu pin üçün mövcud olan alternative funksiyalar sırasına qayıdır.

.. method:: Pin.debug([state])

   Debugging vəziyyətini təyin edir (``True`` və ya ``False`` aktiv və ya deaktiv etmək üçün).

.. method:: Pin.dict([dict])

   pin mapper lüğətini təyin və ya əldə et.

.. method:: Pin.mapper([fun])

   pin mapper funksiyasını təyin və ya əldə et.


Metodlar
-------

.. method:: pin.init(mode, pull=Pin.PULL_NONE, af=-1)

   Pinin başladılması:
   
     - ``mode`` aşağıdakılardan biri ola bilər:
       - ``Pin.IN`` - pini giriş(input) üçün tənzimləyir;
       - ``Pin.OUT_PP`` - pini çıxış(output) üçün tənzimləyir, push-pull control ilə;
       - ``Pin.OUT_OD`` - pini çıxış üçün tənzimləyir, with open-drain control metodu ilə;
       - ``Pin.AF_PP`` - pini alternativ funksiya üçün tənzimləyir, pull-pull;
       - ``Pin.AF_OD`` - pini alternativ funksiya üçün tənzimləyir, open-drain;
       - ``Pin.ANALOG`` - pini analog üçün tənzimləyir.
     - ``pull`` aşağıdakılardan biri ola bilər:
       - ``Pin.PULL_NONE`` - qalxan və ya enən müqavimət aktiv deyil.
       - ``Pin.PULL_UP`` - müqavimətin qalxmasını aktiv etmək.
       - ``Pin.PULL_DOWN`` - müqavimətin enməsini aktiv etmək.
     - Rejim Pin.AF_PP or Pin.AF_OD, olarsa onda af index və ya pin ilə bağlı olan alternativ funksiyanın adı ola bilər.
   
   Göstərir(Returns): ``None``.

.. method:: pin.high()

   Pini yüksək məntiq səviyyəsinə təyin etmək.

.. method:: pin.low()

   Pini aşağı məntiq səviyyəsinə təyin etmək.

.. method:: pin.value([value])

   Pinin rəqəmsal məntiq səviyyəsini təyin etmək və ya əldə etmək:

     - heç bir arqument verilmədikdə məntiq səviyyəsindən asılı olaraq 0 və ya 1 göstərir.
     - ``value`` pinin məntiq səviyyəsini təyin etmək verilən dəyərdir.
      ``value`` booleana çevrilən hər şey ola bilər
       Əgər ``True`` -ya çevrilirsə onda pin yüksək məntiqə tənzimlənir,
       digər hallarda aşağı məntiqə tənzimlənir.

.. method:: pin.__str__()

  Pin obyekti izah edən string göstərir.

.. method:: pin.af()

   Pinin hazırki tənzimlənmiş alternativ funksiyanı göstərir.
   Göstərilən rəqəm init funksiyasında af arqument üçün icazə verilən vahidlərdən birinə uyğun olmalıdır.

.. method:: pin.gpio()

   GPIO blokunun pinlə bağlı olan əsas ünvanını göstərir.

.. method:: pin.mode()

   Pinin hazırki tənzimlənmiş rejimini göstərir.
   Göstərilən rəqəm init funksiyasında mode arqument üçün icazə verilən vahidlərdən birinə uyğun olmalıdır.

.. method:: pin.name()

   Pinin adını əldə et

.. method:: pin.names()

   Pin üçün cpu və board-ın adını göstərir.

.. method:: pin.pin()

   Pinin nömrəsini əldə et.

.. method:: pin.port()

   Pinin portunu əldə et

.. method:: pin.pull()

   Pinin hazırki tənzimlənmiş pull dəyərini göstərir.
   Göstərilən rəqəm init funksiyasında pull arqument üçün icazə verilən vahidlərdən birinə uyğun olmalıdır.


Sabitlər(Constants)
---------

.. data:: Pin.AF_OD

   pini alternativ funksiya ilə open-drain drive rejimində başladır

.. data:: Pin.AF_PP

   Pini alternativ funksiya ilə open-drain drive rejimində başladır

.. data:: Pin.ANALOG

   pini analoq rejimə tənzimləyir

.. data:: Pin.IN

   pini giriş(input) rejiminə tənzimləyir

.. data:: Pin.OUT_OD

   pini open-drain drive ilə çıxış(output)rejiminə tənzimləyir.

.. data:: Pin.OUT_PP

   Pini push-pull drive ilə çıxış(output) rejiminə tənzimləyir

.. data:: Pin.PULL_DOWN

   Pində müqavimətin aşağı düşməsini aktiv edir

.. data:: Pin.PULL_NONE

   Pində müqavimətin qalxmasını və ya enməsini deaktiv edir

.. data:: Pin.PULL_UP

   Pində müqavimətin qalxmasını aktiv edir


class PinAF -- Alternativ Pin Funksiyaları
======================================

Pin mikroprosessorda fiziki pini təmsil edir. Hər pinin öz funksiyalarında müxtəliflik ola bilər(GPIO, I2C SDA, və.s).
Hər PinAF obyekt pin üçün xüsusi funksiyaları təmsil edir.

Istifadə örnəyi::

    x3 = pyb.Pin.board.X3
    x3_af = x3.af_list()

x3_af burada PinAF obyektlər üçün sıraya(array) malikdir hansı ki onlar pin X3-də mövcuddurlar.

Pyboard üçün x3_af özündə aşağıdakıları əks etdirəcək:
    [Pin.AF1_TIM2, Pin.AF2_TIM5, Pin.AF3_TIM9, Pin.AF7_USART2]

Normalda, hər periferik(peripheral, çevresel) af-ni avtomatik tənzimləyəcək ama bəzən eyni funksiya bir
çox pində mövcud olur və daha dəqiq idarəetmə arzuolunandır.


TIM2_CH3-ü göstərmək(görünən etmək,expose) üçün siz aşağıdakı kodlardan istifadə edə bilərsiniz::

   pin = pyb.Pin(pyb.Pin.board.X3, mode=pyb.Pin.AF_PP, af=pyb.Pin.AF1_TIM2)

və ya::

   pin = pyb.Pin(pyb.Pin.board.X3, mode=pyb.Pin.AF_PP, af=1)


Metodlar
-------

.. method:: pinaf.__str__()

   Alternativ funksiayanın sırasını(string,dizi) göstərir.

.. method:: pinaf.index()

   alternativ funksiyanın indexini göstərir.

.. method:: pinaf.name()

   Alternativ funksiyanın adını göstərir.

.. method:: pinaf.reg()

   alternativ funksiyaya təyin olunmuş periferik reestri göstərir.
   Məsələn əgər alternative funksiya TIM2_CH3 olarsa onda bu funksiya bizə
   stm.TIM2 göstərəcək(return)
