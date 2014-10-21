LED-ləri yandırmaq və ilkin Python anlayışları
=========================================

Pyboard-da edə biləcəyiniz sadə işlərdən biri LED işıqları yandırmaq və söndürməkdir. Board-ı PC-yə qoşun, tutorial 1-də göstərildiyi kimi log in olun. Biz işıqları interpreter-dən yandırmağı göstərməklə başlayırıq: ::
    >>> myled = pyb.LED(1)
    >>> myled.on()
    >>> myled.off()

Bu komandalar LED-i yandırıb söndürür.

Bu üsul çox gözəl olsa da, təbii ki, biz bu prosesi avtomatlaşdırmağa çalışmalıyıq. MAIN.PY faylını sevdiyiniz text editor-la açın. Fayl daxilinə aşağıdakı kod parçasını yazın. Əgər siz Python-da yenisinizsə boşluqlara (indentation) mütləq şəkildə fikir verin: ::

    led = pyb.LED(2)
    while True:
        led.toggle()
        pyb.delay(1000)

Faylı yaddaşa verdikdən sonra, pyboard-dakı qırmızı işıq 1 saniyəlik yanmalıdır. Script-i run etmək üçün "soft reset" edin, yəni CTRL+D. Bundan sonra, pyboard restart olacaq ve siz görəcəksiniz ki, yaşıl işıq dayanmadan yanıb sönür. Təbriklər, dəhşətli robotlar düzəltmək yolunda ilk addımılarınızı artıq atdınız. Əgər yanan sönən işıqlardan bezdinizsə sadəcə terminalda CTRL+C edin.
Sözsüz ki maraqlıdır,axı bu kod parçası nə edir? Bunun üçün biraz terminalogiyaya baş vuraq. Python obyekt-yönümlü(OOP) proqramlaşdırma dilidir, Python-da demək olar ki hər şey *class*-dır və siz class-ın instance-ını yaratdıqda nəticədə əlinizdə *object* olur. Klaslarda həmçinin onların öz *metod*-ları olur. Metod(member function) *object* -i idarə etməyə həmçinin onunla işləməyə icazə verir.
Kodun birinci sətri, LED obyektini yaradır ki, biz onun adını led qoymuşuq. LED Obyekt yaratdıqda, 1-dən 4-ə qədər parameter ötürə bilərik, əgər xatırlayırsınızsa bizim board-ın üzərində 4 LED gəlib və bu rəqəmlər həmin LED-lərə uyğundur. pyb.LED klasının 3 mühüm funksiyası(member function) var: on(), off() və toggle(). Bizim istifadə etdiyimiz digər funksiya pyb.delay() adından da görüldüyü kimi, verilmiş vaxt(millisaniyə) ərzində gözləməyi təmin etmək üçündür. LED obyektini yaratdıqdan sonra, verilmiş while True: sonsuz döngü yaradır və bu döngü daxilində, 1 saniyəlik aralıqlarla LED yanıb sönür.

**Tapşırıq: Fərqli bir LED üzərində daha uzun müddətli toggle olunmağı təmin edin.**

**Tapşırıq: pyboard-da birbaşa qoşulun, pyb.LED object-ini yaradın və LED-lərdən hər hansı birini on() metodu ilə yandırın.**

A Disco on your pyboard
-----------------------

So far we have only used a single LED but the pyboard has 4 available. Let's start by creating an object for each LED so we can control each of them. We do that by creating a list of LEDS with a list comprehension. ::

    leds = [pyb.LED(i) for i in range(1,5)]

If you call pyb.LED() with a number that isn't 1,2,3,4 you will get an error message.
Next we will set up an infinite loop that cycles through each of the LEDs turning them on and off. ::

    n = 0
    while True:
      n = (n + 1) % 4
      leds[n].toggle()
      pyb.delay(50)

Here, n keeps track of the current LED and every time the loop is executed we cycle to the next n (the % sign is a modulus operator that keeps n between 0 and 4.) Then we access the nth LED and toggle it. If you run this you should see each of the LEDs turning on then all turning off again in sequence.

One problem you might find is that if you stop the script and then start it again that the LEDs are stuck on from the previous run, ruining our carefully choreographed disco. We can fix this by turning all the LEDs off when we initialise the script and then using a try/finally block. When you press CTRL-C, Micro Python generates a VCPInterrupt exception. Exceptions normally mean something has gone wrong and you can use a try: command to "catch" an exception. In this case it is just the user interrupting the script, so we don't need to catch the error but just tell Micro Python what to do when we exit. The finally block does this, and we use it to make sure all the LEDs are off. The full code is::

    leds = [pyb.LED(i) for i in range(1,5)]
    for l in leds: 
        l.off()

    n = 0
    try:
       while True:
          n = (n + 1) % 4
          leds[n].toggle()
          pyb.delay(50)
    finally:
        for l in leds:
            l.off()

The Fourth Special LED
----------------------

The blue LED is special. As well as turning it on and off, you can control the intensity using the intensity() method. This takes a number between 0 and 255 that determines how bright it is. The following script makes the blue LED gradually brighter then turns it off again. ::

    led = pyb.LED(4)
    intensity = 0
    while True:
        intensity = (intensity + 1) % 255
        led.intensity(intensity)
        pyb.delay(20)

You can call intensity() on the other LEDs but they can only be off or on. 0 sets them off and any other number up to 255 turns them on.
