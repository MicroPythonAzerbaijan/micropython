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


Pyboard-da Disco keyfi
-----------------------
Bu ana qədər biz yalnız 1 LED-dən istifadə etmişik lakin, pyboard-da 4-ü var. Hər bir LED-i idarə etmək üçün müvafiq obyekt yaratmaq lazımdır.  Biz buna LED-lərin listini yaratmaqla nail oluruq. ::

    leds = [pyb.LED(i) for i in range(1,5)]

Əgər siz pyb.LED()-i 1,2,3,4-dən başqa rəqəmlərdən başqa qiymətlərlə çağırsanız, o zaman error mesajı görmüş olarsınız.
Daha sonra biz, sonsuz döngü yaradırıq. Kod bütün LED-lərdən dövr edərək onları yandırıb söndürür. ::

    n = 0
    while True:
      n = (n + 1) % 4
      leds[n].toggle()
      pyb.delay(50)

Burda n hal-hazırkı LED-i qeydə alır və hər dövrdə növbəti n-ə keçir(% işarəsi modullu bölmə əməliyyatıdır, bununla n 0 ilə 4 arasında qalır). Daha sonra da növbəti n-ə(LED-ə) keçirik və işıqları dəyişirik.Bu kodu işlətsəniz, görərsiniz ki, LED işıqlar növbə ilə yanıb sönür. 

Bu kodla bağlı problem ondadır ki, biz scripti sonlandırıb yenidən işlətsək, LED-lər ilişəcək və bizim çox maraqlı diskomuzu pozacaq. Bunu aradan qaldırmaq üçün, hər dəfə script çalışmağa başladıqda, LED-ləri söndürəcik və ən sonda da try/finally blokdan istifadə edəcik. CTRL+C-ni sıxdıqda Micro Python VCPInterrupt exception qaytaracaq. Bir kodun Exception qaytarması, onun hər bir halda düzgün işləməməsinin göstəricisidir. Bu məqsədlə try:-dan istifadə edərək exception-ları catch edə bilərsiniz. İndiki halda, CTRL+C user tərəfindən kodun dayandırılmasıdır, bu səbəbdən hər hansl try-catch-ə ehtiyac yoxdur. Bunun əvəzinə scriptimizə işini dayandırdıqda nə etməli olduğunu deyəcik.
Hər hansı interrupt zamanı kodumuz bütün LED-ləri söndürəcək.
Kodun ən son halı aşağıdakıdır. ::

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


4-cü LED xüsusidir
----------------------
Mavi LED xüsusidir. LED-i söndürüb yandırmaqdan əlavə, onun instensivliyini intensity() funksiyası ilə idarə edə bilərsiniz. 0-dan 255-ə qədər rəqəmləri qəbul edir və bununla da parlaqlığı müəyyən edir. Aşağıdakı kod mavi LED-i tədricən parlaqlaşdırır və ən sonda da söndürür. ::

    led = pyb.LED(4)
    intensity = 0
    while True:
        intensity = (intensity + 1) % 255
        led.intensity(intensity)
        pyb.delay(20)

Digər LED-lərdə də intensity() metodunu istifadə edə bilərsiniz. Lakin unutmayın ki digər LED-lərdə 0 həmin LED-i söndürəcək, istənilən digər rəqəm isə onu yandıracaq.
