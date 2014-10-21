Pyboard-a giriş
===========================

Pyboard-ı mükəmməl şəkildə başa düşmək və onunla işləməyi bacarmaq üçün,
onun iş prinsipi ilə bağlı xırda lakin, vacib məlumatları əldə etməkdə fayda var.

Pyboard-la diqqətli olaq
-----------------------

Pyboard-ın qoruyucu korpusu olmadığı üçün, onunla diqqətli davranmaq lazımdır:

  - USB kabeli pyboard-a taxanda və çıxardanda ehtiyatla davranın.
    Baxmayaraq ki, USB konnektor board-a bərkidilib(lehimlənib), ehtiyatsızlıq ucbatından qırılsa, düzəldilməsi çox çətindir.
    
  - Statik elektrik cərəyanı pyboard-ın hissələrini şok edə bilər, hətta onları sıradan çıxara bilər.
    Əgər sizin ərazidə uzun müddətli statik elektirk cərəyanı varsa (məsələn, quru və soyuq iqlim),
    o zaman siz Pyboard-ınızı qorumaq üçün əlavə səylər göstərməlisiniz.
    Əgər board-ınız qara plastik qutuda sizə təqdim olunubsa, o zaman pyboard-ı daşıyarkən və saxlayarkən bu qutudan istifadə edin
    (Qutu ötürücülü plastikdən hazırlanmışdır və daxilində ötürücü köpük yerləşdirilmişdir)
    

Fiziki şəkildə pyboard-la diqqətli olduğunuz halda, heç bir çətinliyiniz olmayacaq.
Pyboard-da proqram təminatını qırmaq demək olar ki, mümkün deyil.
Bu səbəbdən istədiyiniz qədər, rahat şəkildə kod yazıb sınaya bilərsiniz.
Əgər filesystem zədələnərsə, aşağıda göstərilmiş üsullardan istifadə edib onu reset edə bilərsiniz.
Ən pis halda siz Micro Python proqram təminatını reflash edəcəksiz.
Narahat olmayın bunu siz USB ilə edə bilərsiniz.



Pyboard-ın sxemi(layout)
---------------------
Mikro USB konnektor üst tərəfdən sağda yerləşir.
Mikro SD kart slot isə üst tərəfdən solda yerləşir.
4 LED işıqlar isə SD slot ilə  USB konnektorun arasında yerləşir.
İşıqların rəngləri, aşağıdan yuxarıya qırmızı,yaşıl, narıncı və ən üstdə mavidir.
Pyboard-da 2 switch yerləşir: sağdakı resetləmək, soldakı isə "istifadəçi switch-idir"


Qoşulma və Qidalanma
---------------------------

Pyboard USB vasitəsilə kompüterə qoşulur və işə salınır(qidalanır).
Kabel PC-yə qoşulduqdan dərhal sonra, yaşıl LED yanıb sönməyə başlayacaq.


Xarici vasitə ilə qidalanma
------------------------------------
Pyboard xarici qida mənbəyi ilə(məs. batareya) işlədilə bilər


**Be sure to connect the positive lead of the power supply to VIN, and
ground to GND.  There is no polarity protection on the pyboard so you
must be careful when connecting anything to VIN.**

**The input voltage must be between 3.6V and 10V.**
