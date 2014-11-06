Həvəskar servo (avto) motorların idarə edilməsi
=========================

Həvəskar servo motorlarla əlaqənin yaradılması üçün çipdə (mikrosxemdə) 4 əlaqə çıxışı var.
(nümunə: [Wikipedia](http://en.wikipedia.org/wiki/Servo_%28radio_control%29))
Bu motorlarda 3 kabel var. 0 (yüksüz), faza və siqnal kabelləri.
Bu kabelləri siz çipin üzərində sağ aşağı küncdən siqnal mərkəzinə (pin) qoşa bilərsiniz.
X1, X2, X3 və X4 deyə 4 əlaqədar siqnal pin mövcuddur. 

.. image:: img/pyboard_servo.jpg

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
due to rounding errors in setting the pulse width (Длительность импулса). (Tərcümə olunması mümkün olmayan hissə)

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

``angle`` və ``speed`` metodları arasındakı fərq (adlarından başqa),
əmrlər yazılarkən daxil edilən rəqəmlərdir.
Həmin rəqəmlər müxtəlif şəkildə impuls müddəti (pulse width,Длительность импулса)-a konvertasiya olunur.

Tənzimləmələr
-----------

Əmrlərin ``angle`` və ya ``speed`` metodundan impuls müstəvisinə keçməsi
tənzimləmə dəyərləri ilə reallaşır.
Belə ki, cari tənzimləmələr üçün aşağıdakı dəyərlərdən istifadə edə bilərsiniz: ::

    >>> servo1.calibration()
    (640, 2420, 1500, 2470, 2200)
    
Bu beş ədəd aşağıdakı mənalara gəlir: 

1. Minimum impuls: Servo motorun qəbul etdiyi ən kiçik impulsdur
2. Maksimum impuls: Servo motorunun qəbul etdiyi ən geniş impulsdur
3. Mərkəz impulse: Servo motoru üçün bu siqnal 0 dərəcə və ya 0 sürət mənasına gəlir
4. Bu impuls angle metodunda 90 dərəcə mənasında işlədilir
5. Bu impuls speed metodunda 100 sürət anlamında işlədilir 

Siz servo matorunu aşağıdakı dəyərlərdən istifadə edərək yenidən tənzimləməyə bilərsiz: ::
    
    >>> servo1.calibration(700, 2400, 1510, 2500, 2000)
    
Təbii ki, yuxarıda göstərilmiş ölçülərə uyğun tənzimləmələri,
dəyişiklikləri servo motorunuz buna uyğundursa edə bilərsiniz. 
