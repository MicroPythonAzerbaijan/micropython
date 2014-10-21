Pyboard-a giriş
===========================

Pyboard-ı mükəmməl şəkildə başa düşmək və onunla işləməyi bacarmaq üçün,
onun iş prinsipi ilə bağlı xırda lakin vacib məlumatları əldə etməkdə fayda var.

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
    

As long as you take care of the hardware, you should be okay.  It's almost
impossible to break the software on the pyboard, so feel free to play around
with writing code as much as you like.  If the filesystem gets corrupt, see
below on how to reset it.  In the worst case you might need to reflash the
Micro Python software, but that can be done over USB.

Layout of the pyboard
---------------------

The micro USB connector is on the top right, the micro SD card slot on
the top left of the board.  There are 4 LEDs between the SD slot and
USB connector.  The colours are: red on the bottom, then green, orange,
and blue on the top.  There are 2 switches: the right one is the reset
switch, the left is the user switch.

Plugging in and powering on
---------------------------

The pyboard can be powered via USB.  Connect it to your PC via a micro USB
cable.  There is only one way that the cable will fit.  Once connected,
the green LED on the board should flash quickly.

Powering by an external power source
------------------------------------

The pyboard can be powered by a battery or other external power source.

**Be sure to connect the positive lead of the power supply to VIN, and
ground to GND.  There is no polarity protection on the pyboard so you
must be careful when connecting anything to VIN.**

**The input voltage must be between 3.6V and 10V.**
