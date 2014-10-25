Taymerlər
==========

Pyboard-ın 14 taymeri var.
Hər bir taymer müstəqil sayğaca malikdir və bu sayğac istifadəçinin müəyyən etdiyi tezlikdə çalışır.
Taymerlər, müəyyən aralıqlarda, müəyyən funksiyanı çağırıb işlədə bilərlər.

14 Taymer 1-dən 14-ə qədər nömrələnib.
3-cü taymer daxili istifadə üçün ayrılıb.
5 və 6-cılar isə servo və ADC/DAC kontrol üçün rezerv olunub.
Mümkün mərtəbə bu rezerv olunmuş taymerlərdən istifadə etməyin.

Gəlin taymer obyekti yaradaq ::

    >>> tim = pyb.Timer(4)

İndicə nə yaratdığımıza baxaq ::

    >>> tim
    Timer(4)

Pyborad(çip) deyir ki, ``tim`` 4 nömrəli taymerə uyğundur, lakin o hələ başladılmayıb(initialised,инициализировать).
Gəlin, taymeri 10 Hz tezlikdə (hər saniyədə 10 dəfə) başladaq.
 #### Kömək lazım olan hissə ###
 So let's initialise it to trigger at 10 Hz
(that's 10 times per second)::
#### Kömək lazım olan hissə ###

    >>> tim.init(freq=10)

İndi taymer başladılıb və biz onun haqqında bəzi məlumatlar əldə edə bilərik. ::

    >>> tim
    Timer(4, prescaler=255, period=32811, mode=0, div=0)

Yuxarıdakı məlumatı bir az izah edək.
prescaler=255 -> Periferik saat sürətinin 255-ə bölünməsindən alınan qiymət.
Bu o deməkdir ki, bu taymer məhz yuxarıdakı qiymətə bərabər olan sürətlə işləyir.
period=32811 -> Sayğac maksimum bu qiymətə qədər sayacaq.
32811-ə çatanda yenidən 0-dan saymağa başlayacaq.
Bu rəqəmlər 10 Hz-də taymerin başalamasını(trigger) təmin etmək üçün verilmişdir.

Taymer sayğacı
-------------

Bəs biz taymerimizlə nələr edə bilərik?
Ən sadəsi biz istədiyimiz vaxtda sayğacın qiymətini götürə bilərik. ::

    >>> tim.counter()
    21504

Təbii ki, siz hər dəfə fərqli rəqəmlər alacaqsınız, çünki sayğac davamlı olaraq sayımı artırır.

Timer callbacks
---------------

Edə biləcəyimiz digər işlərdən biri budur ki, biz taymer üçün callback funskiyası yerləşdirə bilərik.
Qısa olaraq desək, taymer işlədikcə (trigger) bu funksiya çağırılacaq
(daha ətraflı oxuyun [switch tutorial](tut-switch) callback funksiyalarına giriş)::

    >>> tim.callback(lambda t:pyb.LED(1).toggle())


Dərhal qırmızı LED yanıb-sönməyə başlamalıdır.
Bu 5 Hz tezlikdə yanıb sönməyə başlayacaq
(1 yanıb sönmə üçün 2 toggle lazımdır, 2 toggle hərəsi 5 Hz tezliklə işləyərsə ümumilikdə 10 Hz-lik fəaliyyət baş tutur).
Burdan belə nəticə çıxır ki, 10 Hz-in 5 Hz-i işığı yandırır, digər 5 Hz-i işığı söndürür.
Ümumilikdə 5 Hz-lik yanıb sönmə(flashing) baş tutur.

Tezliyi dəyişmək üçün, taymeri re-initialize etmək lazımdır. ::

    >>> tim.init(freq=20)

Callback-i söndürmək üçün , ``None`` dəyərini ötürmək lazımdır. ::

    >>> tim.callback(None)

Callback-ə yalnız 1 arqumentli funksiya ötürməlisiniz.
Hansı ki, trigger olunan taymer obyektidir.
Bu, callback funksiyası daxilindən taymeri idarə etməyə imkan verir.

Biz 2 müstəqil işləyən taymer yarada bilərik ::

    >>> tim4 = pyb.Timer(4, freq=10)
    >>> tim7 = pyb.Timer(7, freq=20)
    >>> tim4.callback(lambda t: pyb.LED(1).toggle())
    >>> tim7.callback(lambda t: pyb.LED(2).toggle())

Callback müvafiq(proper) hardware interrupt olması səbəbi ilə,
taymerlər işlədiyi müddətdə biz pyboard-ı digər məqsədlər üçün də istifadə edə bilərik.


Mikro saniyə sayğacının yaradılması
----------------------------

Siz taymerdən mikro saniyə sayğacı yarada bilərsiniz.
Bu dəqiq zamanlama tələb olunan işlərdə sizə yardımçı ola bilər.
Biz 2 nömrəli taymeri bu məqsədlə istifadə edəcik.
Taymer 2-nin 32-bit sayğacı var(bu baxımdan taymer 5 ilə eynidir,
lakin taymer 5 Servo drayver-ə ayrıldığı üçün, Servo drayverlə paralel işlədilə bilməz).

Taymer 2-nin setup-ı aşağıdakı kimidir ::

    >>> micros = pyb.Timer(2, prescaler=83, period=0x3fffffff)

Prescaler 83-ə bərabər edilmişdir, bu taymeri 1 Mhz-də saymağa məcbur edir.
CPU saatı 168 Mhz-lə çalışır, onu 2-yə bölürük daha sonra prescaler+1-ə bölürük.
Bu da nəticədə 1 Mhz verir (168 MHz/2/(83+1)=1 MHz).
Period çox böyük ədədə bərabər etmişik.
Bu səbəblə taymer çox böyük rəqəmə qədər saya biləcək.
Bu halda sayğacın artımı edib daha sonra sıfırlanması 17 dəqiqə vaxt alacaq.

Bu taymeri istifadə etmək üçün, ilk öncə onu sıfırlayaq ::

    >>> micros.counter(0)

Daha sonra isə timing-i başlatın :: 

    >>> start_micros = micros.counter()

    ... Hər hansı işi burda görün ...

    >>> end_micros = micros.counter()