AMP audio örtüyü
==================

AMP audio örtüyünün qaynaq edilməsi və istifadəsi.

.. image:: img/skin_amp_1.jpg
    :alt: AMP skin
    :width: 250px

.. image:: img/skin_amp_2.jpg
    :alt: AMP skin
    :width: 250px

Növbəti video başlıqlarının, mikrofon və ucadandanışanın AMP örtüyünə necə qaynaq edildiyini göstərir.

.. raw:: html

    <iframe style="margin-left:3em;" width="560" height="315" src="http://www.youtube.com/embed/fjB1DuZRveo?rel=0" frameborder="0" allowfullscreen></iframe>

Kod nümunəsi
------------

AMP örtüyünün kiçik gücləndirici vasitəsilə ``DAC(1)``-ə qoşulmuş ucadandanışanı vardır.
Gücləndiricinin səviyyəsi rəqəmsal potensiometer vasitəsi idarə edilir, hansı ki,
``IC2(1)`` şinində ünvanı 46 olan I2C cihazdır.

Səviyyəni təyin etmək üçün aşağıdakı funksiyanı yazın::

    import pyb
    def volume(val):
        pyb.I2C(1, pyb.I2C.MASTER).mem_write(val, 46, 0)

Sonra bunları edə bilərsiniz::

    >>> volume(0)   # minimum volume
    >>> volume(127) # maximum volume

Səsləndirmək üçün ``DAC`` obyektinin ``write_timed`` metodundan istifadə edin.
Məsələn::

    import math
    from pyb import DAC

    # create a buffer containing a sine-wave
    buf = bytearray(100)
    for i in range(len(buf)):
        buf[i] = 128 + int(127 * math.sin(2 * math.pi * i / len(buf)))

    # output the sine-wave at 400Hz
    dac = DAC(1)
    dac.write_timed(buf, 400 * len(buf), mode=DAC.CIRCULAR)

Siz həmçinin Python-un ``wave`` modulunun köməyi ilə WAV faylları da oxuda bilərsiniz.
wave modulunu `buradan <http://micropython.org/resources/examples/wave.py>`_ əldə edə bilərsiniz, həmçinin `buradan <http://micropython.org/resources/examples/chunk.py>`_ əldə edə biləcəyiniz chunk moduluna ehtiyacınız olacaqdır. Bunları pyboard-da (fləş və ya SD kartda baş qovluğa) yerləşdirin.
Oxutmaq üçün `belə <http://micropython.org/resources/examples/test.wav>`_ bir 8-bitlik WAV faylına ehtiyacınız olacaqdır. Sonra aşağıdakılar edə bilərsiniz.

    >>> import wave
    >>> from pyb import DAC
    >>> dac = DAC(1)
    >>> f = wave.open('test.wav')
    >>> dac.write_timed(f.readframes(f.getnframes()), f.getframerate())

Bu WAV faylını oxutmalıdır.
