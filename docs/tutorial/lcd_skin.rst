LCD və touch-sensor(rus dilində: сенсорный датчик)
=============================

LCD və touch-sensor-un birləşdirilməsi və istifadəsi

.. image:: http://micropython.org/static/doc/skin-lcd-3.jpg
    :alt: pyboard with LCD skin
    :width: 250px

.. image:: http://micropython.org/static/doc/skin-lcd-1.jpg
    :alt: pyboard with LCD skin
    :width: 250px

Aşağıdakı videoda LCD-yə başlıqların qoşulması göstərilmişdir.
Videonun sonunda isə, LCD-nin pyboard-da necə düzgün şəkildə qoşulmalı olduğu göstərilmişdir.

.. raw:: html

    <iframe style="margin-left:3em;" width="560" height="315" src="http://www.youtube.com/embed/PowCzdLYbFM?rel=0" frameborder="0" allowfullscreen></iframe>


LCD-nin istifadəsi
-------------

To get started using the LCD, try the following at the Micro Python prompt.
LCD ilə işə başlamaq üçün, Micro Python prompt-da aşağıdakını sınayın.
Əmin olun ki, LCD pyboard-a məhz şəkildə göstərildiyi kimi qoşulmuşdur: ::

    >>> lcd = pyb.LCD('X')
    >>> lcd.light(True)
    >>> lcd.write('Hello uPy!\n')

Aşağıdakı koddan istifadə etməklə sadə animasiya yarada bilərsiniz: ::

    lcd = pyb.LCD('X')
    lcd.light(True)
    for x in range(-80, 128):
        lcd.fill(0)
        lcd.text('Hello uPy!', x, 10, 1)
        lcd.show()
        pyb.delay(25)

Touch sensor-un istifadəsi
----------------------

Touch-sensor məlumatları oxumaq üçün siz I2C marşrutundan(BUS) istifadə etməlisiniz.
MPR121 kapasiteli(red. ~ Türkiyə türkçəsi) touch sensor-in adresi 90-dır.

Sınamaq üçün: ::

    >>> i2c = pyb.I2C(1, pyb.I2C.MASTER)
    >>> i2c.mem_write(4, 90, 0x5e)
    >>> touch = i2c.mem_read(1, 90, 0)[0]

İlk sətr, I2C obyektini yaradır, ikinci sətr 4 touch sensoru aktivləşdirir,
üçüncü sətr "touch status"-u oxuyur
və ``touch`` dəyişəni isə 4 touch düyməciklərdən gələn məlumatı özündə saxlayır(A, B, X, Y).


Çox sadə driver artıq mövcuddur [here](/static/doc/examples/mpr121.py)
which allows you to set the threshold and debounce parameters, and
easily read the touch status and electrode voltage levels.  Copy
this script to your pyboard (either flash or SD card, in the top
directory or ``lib/`` directory) and then try::

    >>> import pyb
    >>> import mpr121
    >>> m = mpr121.MPR121(pyb.I2C(1, pyb.I2C.MASTER))
    >>> for i in range(100):
    ...   print(m.touch_status())
    ...   pyb.delay(100)
    ...

This will continuously print out the touch status of all electrodes.
Try touching each one in turn.

Note that if you put the LCD skin in the Y-position, then you need to
initialise the I2C bus using::

    >>> m = mpr121.MPR121(pyb.I2C(2, pyb.I2C.MASTER))

There is also a demo which uses the LCD and the touch sensors together,
and can be found [here](/static/doc/examples/lcddemo.py).
