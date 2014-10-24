

Note that for some angles, the returned angle is not exactly the same as
the angle you set, due to rounding errors in setting the pulse width.

You can pass a second parameter to the ``angle`` method, which specifies how
long to take (in milliseconds) to reach the desired angle.  For example, to
take 1 second (1000 milliseconds) to go from the current position to 50 degrees,
use ::

     >>> servo1.angle(50, 1000)

This command will return straight away and the servo will continue to move
to the desired angle, and stop when it gets there.  You can use this feature
as a speed control, or to synchronise 2 or more servo motors.  If we have
another servo motor (``servo2 = pyb.Servo(2)``) then we can do ::

    >>> servo1.angle(-45, 2000); servo2.angle(60, 2000)

This will move the servos together, making them both take 2 seconds to
reach their final angles.

Note: the semicolon between the 2 expressions above is used so that they
are executed one after the other when you press enter at the REPL prompt.
In a script you don't need to do this, you can just write them one line
after the other.

Continuous rotation servos
--------------------------

So far we have been using standard servos that move to a specific angle
and stay at that angle.  These servo motors are useful to create joints
of a robot, or things like pan-tilt mechanisms.  Internally, the motor
has a variable resistor (potentiometer) which measures the current angle
and applies power to the motor proportional to how far it is from the
desired angle.  The desired angle is set by the width of a high-pulse on
the servo signal wire.  A pulse width of 1500 microsecond corresponds
to the centre position (0 degrees).  The pulses are sent at 50 Hz, ie
50 pulses per second.

You can also get **continuous rotation** servo motors which turn
continuously clockwise or counterclockwise.  The direction and speed of
rotation is set by the pulse width on the signal wire.  A pulse width
of 1500 microseconds corresponds to a stopped motor.  A pulse width
smaller or larger than this means rotate one way or the other, at a
given speed.

On the pyboard, the servo object for a continuous rotation motor is
the same as before.  In fact, using ``angle`` you can set the speed.  But
to make it easier to understand what is intended, there is another method
called ``speed`` which sets the speed::

    >>> servo1.speed(30)

``speed`` has the same functionality as ``angle``: you can get the speed,
set it, and set it with a time to reach the final speed. ::

    >>> servo1.speed()
    30
    >>> servo1.speed(-20)
    >>> servo1.speed(0, 2000)

The final command above will set the motor to stop, but take 2 seconds
to do it.  This is essentially a control over the acceleration of the
continuous servo.

A servo speed of 100 (or -100) is considered maximum speed, but actually
you can go a bit faster than that, depending on the particular motor.

The only difference between the ``angle`` and ``speed`` methods (apart from
the name) is the way the input numbers (angle or speed) are converted to
a pulse width.

Calibration
-----------

The conversion from angle or speed to pulse width is done by the servo
object using its calibration values.  To get the current calibration,
use ::

    >>> servo1.calibration()
    (640, 2420, 1500, 2470, 2200)

There are 5 numbers here, which have meaning:

1. Minimum pulse width; the smallest pulse width that the servo accepts.
2. Maximum pulse width; the largest pulse width that the servo accepts.
3. Centre pulse width; the pulse width that puts the servo at 0 degrees
   or 0 speed.
4. The pulse width corresponding to 90 degrees.  This sets the conversion
   in the method ``angle`` of angle to pulse width.
5. The pulse width corresponding to a speed of 100.  This sets the conversion
   in the method ``speed`` of speed to pulse width.

You can recalibrate the servo (change its default values) by using::

    >>> servo1.calibration(700, 2400, 1510, 2500, 2000)

Of course, you would change the above values to suit your particular
servo motor.



###################################################################################################



Həvəskar servo (avto) motorların idarə edilməsi

Həvəskar servo motorlarla əlaqənin yaradılması üçün çipdə (mikrosxemdə) 4 əlaqə çıxışı var.
(nümunə: [Wikipedia](http://en.wikipedia.org/wiki/Servo_%28radio_control%29))
Bu motorlarda 3 kabel var. 0 (yüksüz), faza və siqnal kabelləri.
Bu kabelləri siz çipin üzərində sağ aşağı küncdən siqnal mərkəzinə (pin) qoşa bilərsiniz.
X1, X2, X3 və X4 deyə 4 əlaqədar siqnal pin mövcuddur. 

<img src="/static/doc/pyboard-servo.jpg" alt="pyboard with servo motors" style="width:250px; border:1px solid black; display:inline-block;"/>

Şəkildə  avto motorları çipdə yerləşən əsas siqnal pininə birləşdirən iki keçirici (adaptor, perexodnik) görürsüz. 

0 – yüksüz kabel adətən qara və ya tünd qəhvəyi rənglərdə olur. Faza xətti isə daha çox qırmızı. 

Avto motorlardakı faza çıxışı ( faza pini) ( üzərində VİN yazılır) çipdəki daxil olan faza qaynağı – faza mərkəzinə bir başa qoşulur.
USB kabeli vasitəsilə enerji qaynağına qoşularkən VİN kabeli diod vazitəsilə 5 voltluq USB kabelindən qidalanır.
USB vasitəsiylə 4 kiçik və orta ölçülü avto motorlar lazımi enerjini təmin edə bilir.

Əgər avto motorları başqa üsulla ( yəni USB kabeldən başqa ) enerji qaynağına qoşursunuzsa 6 voltdan artıq olmamasına diqqət edin.
Əksər servo – motorların götürə biləcəyi ən yüksək voltaj da adətən 6V olur. ( Bəzi motorlar isə maksimum 4.8V götürür. Odur ki, istifadə etdiyiniz servo motora diqqət edin)

Servo obyektlərin yaradılması
-----------------------

Servonu birinci pozisiyaya – X1 pininə qoşun və servo obyektin işləmə prinsipini yaratmağa başlayın. ::

    >>> servo1 = pyb.Servo(1)

Servonun bucaq dərəcəsini dəyişmək üçün angle – bucaq metodundan istifadə edin: :: 

    >>> servo1.angle(45)
    >>> servo1.angle(-60)
    
Burda əyilmə bucağı dərəcələndirilib və motordan asılı olaraq -90 –dan +90-a qədər olan intervalda dəyişə bilir.
Parametrlər daxil edilmədən ``angle`` metoduna müraciət etsəniz aşağıdakı bucaq dərəcəsi geri gələcəkdir: :: 

    >>> servo1.angle()
    -60


Note that for some angles, the returned angle is not exactly the same as the angle you set,
due to rounding errors in setting the pulse width. (Tərcümə olunması mümkün olmayan hissə)

Bundan əlavə siz ``angel`` metoduna ikinci parameer daxil edə bilərsiniz.
Bu parametr müəyyən dərəcəyə nə qədər vaxtda çatmasını müəyyənləşdirir(millisaniyə)
Məsələn, indiki vəziyyətindən 50 dərəcəlik bucaq vəziyyətinə keçməsinin 1 saniyədə ( 1000 milli saniyə)
baş tutmasını istəyirsinizsə aşağıdakı koddan istifadə edin. ::

    >>> servo1.angle(50, 1000)

Bu komanda bir başa ötürüləcək və servo müəyyən edilmiş bucaq dərəcəsinə gələnə qədər hərəkət edib,dayanacaq.
Siz bu özəllikdən sürətin idarə edilməsində
və ya 2 və daha artıq servo motorların sinxronlaşdırılmasında da istifadə edə bilərsiz.
Əgər bizim ikinci servo motorumuz varsa(servo2 = pyb.Servo(2)) o zaman ::

    >>> servo1.angle(-45, 2000); servo2.angle(60, 2000)
    
Bu hər iki servo motorun eyni anda işləməsini və hər iki motorun müəyyənləşdirilmiş bucağa eyni vaxtda – 2 saniyədə çatmasını təmin edəcəkdir. 

Qeyd: Yuxarıdakı misalda iki əmr arasında nöqtə vergüldən istifadə olunub.
Odur ki, əmrlər siz REPL –də ENTER düyməsini basan kimi dərhal ard-arda həyata keçiriləcəkdir.
Ancaq, siz əmrləri yazarkən buna ehtiyac yoxdur. Əmrləri sadəcə ard-arda yaza bilərsiniz. 



Servoların davamlı fırlanması
--------------------------

İndiyə kimi biz servonu müəyyən bucaq altında hərəkət etdirdik və həmin bucaq dərəcəsində qala bilməsini təyin etdik.
Bu servolar robotların birləşmə yerləri və yaxud pan-tilt mexanizmlər üçün idealdır. 
Motorda resistor ( potentiometer) dəyişəni var.
Hansı ki, hal-hazırda olduğu bucaq dərəcəsi ilə verilmiş edilmiş bucaq arasındakı nisbəti(uzaqlığı) ölçür.
Verilmiş(istənilmiş) bucaq dərəcəsi servonun siqnal kabeli vasitəsilə həyata keçirilir.
1500 mikro saniyə genişliyindəki impuls mərkəz pozisiyasına (0 dərəcəyə) tuş gəlir.
Siqnallar 50 hers sürətlə göndərilir digər sözlə Saniyəyə 50 siqnal.

Servo motorlar saat istiqamətində və satın əks istiqamətində davamlı hərəkət edə bilir.
Motorların hərəkət sürəti və istiqaməti siqnal kabeli vasitəsilə müəyyənləşdirilir.
1500 mikro saniyəlik impuls motorun dayandırılması impulsudur.
Bu impulsdan daha geniş və daha kiçik siqnallar hərəkətin veilmiş sürətdə bu və ya digər istiqamətdə fırlanmasını müəyyənləşdirir.  

Pyboard-da,çipdə (mikro sxem) servo motorun fırlanmasının prinsipi elə əvvəlki kimidir.
``angle`` dan istifadə edərək fırlanma sürətini yaza bilərsiniz.
Ancaq, bu işi daha da asanlaşdırmaq, anlaşıqlı etmək üçün, ``speed`` metodu var.
Sürəti bu metodla müəyyən edirlər. ::

    >>> servo1.speed(30)

``speed`` metodunun da işləmə prinsipi ``angle`` da olduğu kimidir.
Siz sürətin indiki qiymətini ala , onu təyin edə bilərsiniz.
Həmçinin arzu olunan sürətə nə vaxta çatmalı olduğunu təyin edə bilərsiniz: ::

    >>> servo1.speed()
    30
    >>> servo1.speed(-20)
    >>> servo1.speed(0, 2000)
    
Yuxarıda verilmiş sonuncu əmr motorun söndürülməsi üçündür.
Ancaq bu iki saniyəlik bir müddətdə reallaşacaq.
Bu əsasən motorun sürət dəyişməsindəki istənilməyən halları tənzimləmək üçün nəzərdə tutulub. 

Servo motorlorun maksimum sürəti 100-dür ( -100 ).
Ancaq, xüsusi motorlarla adətən daha sürətli hərəkət etmək olur. 

The only difference between the ``angle`` and ``speed`` methods (apart from
the name) is the way the input numbers (angle or speed) are converted to
a pulse width. (Tərcüməyə ehtiyac duyulmayan yer)

Tənzimləmələr
Əmrlərin ``angle`` və ya ``speed`` metodundan impuls müstəvisinə keçməsi tənzimləmə dəyərləri ilə reallaşır. Belə ki, cari tənzimləmələr üçün aşağıdakı dəyərlərdən istifadə edə bilərsiniz:
>>> servo1.calibration()
(640, 2420, 1500, 2470, 2200)
Bu beş ədəd aşağıdakı mənalara gəlir: 

1. Minimum impuls: Servo motorun qəbul etdiyi ən kiçik impulsdur
2. Maksimum impuls: Servo motorunun qəbul etdiyi ən geniş impulsdur
3. Mərkəz impulse: Servo motoru üçün bu siqnal 0 dərəcə və ya 0 sürət mənasına gəlir
4. Bu impuls angle metodunda 90 dərəcə mənasında işlədilir
5. Bu impuls speed metodunda 100 sürət anlamında işlədilir 
Siz servo matorunu aşağıdakı dəyərlərdən istifadə edərək yenidən tənzimləməyə bilərsiz:
>>> servo1.calibration(700, 2400, 1510, 2500, 2000)
Təbii ki, yuxarıda göstərilmiş ölçülərə uyğun tənzimləmələri, dəyişiklikləri servo motorunuz buna uyğundursa edə bilərsiniz. 
