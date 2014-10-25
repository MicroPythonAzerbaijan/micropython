The Switch, callbacks and interrupts
====================================

Pyboard-ın 2 kiçik switch-i var, USR və RST.
RST switch hard-reset üçündür, bunu bassanız o pyboard-ı restart edəcək və yenidən başladacaq.

USR switch ümumi istifadə üçündür və Switch obyekti ilə idarə olunur.
Switch obyekti yaratmaq üçün ::

    >>> sw = pyb.Switch()

Yadda saxlamaq lazımdır ki, ``import pyb`` qeyd etmək lazımdır.
Əks halda, error görə bilərsiniz.

Switch obyekti ilə onun statusunu götürə bilərsiniz ::

    >>> sw()
    False

Əgər Switch basılı deyilsə bu ``False`` qaytaracaq, əgər basılıdırsa ``True``.
Özünüz sınayıb baxa bilərsiniz.

Switch callbacks
----------------

Switch çox sadə bir obyekt olsa da, onun advance bir xüsusiyyəti var:
``sw.callback()`` funksiyası.
Callback funksiyası Switch basılanda nə etməli olduğunu təyin edir və interruptdan istifadə edir.
İnterrupt-ın iş prinsipini başa düşməzdən əvvəl, ən yaxşısı budur ki, kiçik kodla test edək.
Aşağıdakı kodu prompt-dan işlədin ::

    >>> sw.callback(lambda:print('press!'))

Hər dəfə switch basıldıqda ``press!`` sözü ekrana veriləcək.
Bunu dərhal sınayın. USR switch basın və PC-də ekrana ``press!`` sözünün şəklinin gəlib gəlmədiyinə baxın.

Note that this print will interrupt anything you are typing, and
is an example of an interrupt routine running asynchronously.

Başqa bir misalla sınayaq ::


    >>> sw.callback(lambda:pyb.LED(1).toggle())

Yuxarıdakı kod parçası, qırmızı işığı switch basılanda yandırır
və bu proses hətta digər kodlarla bərabər işləyəcək(digər kodların funksionallığına fikir vermədən)

Switch callback-i deaktiv etmək üçün, callback funksiyasına ``None`` parametrini ötürmək lazımdır. ::

    >>> sw.callback(None)

Switch callback-ə arqumentsiz istənilən funksiyanı parametr kimi ötürmək olar.
Yuxarıda biz, Python-nın xüsusiyyətlərindən biri olan ``lambda``-dan istifadə etdik.
Bu xüsusiyyət, anonim funksiya yaradır. 
Eyni nəticəyə aşağıdakı kodla da gəlmək olar ::

    >>> def f():
    ...   pyb.LED(1).toggle()
    ...
    >>> sw.callback(f)

Bu kod parçası, ``f`` funksiyası yaradır və switch callback-ə mənimsədir.
Bu cür üsul mürəkkəb funskiyalar üçün daha əlverişlidir.

Unutmayın ki, sizin callback funksiyalarınız memory(yaddaş) ayırmamalıdır(məs. tuple və list olmamalıdır).
Callback funksiyalarınız mümkün qədər sadə olmalıdır.
Əgər sizə list lazımdırsa, onu əvvəlcədən global dəyişən kimi yaradın(ya da lokal dəyişən kimi-funksiya daxilində)
Əgər siz, uzun və mürəkkəb hesablamalar aparmalısınızsa,
o zaman callback-dən bu əməliyyatları yerinə yetirən digər koda flag(yol, istinad) yaratmaq üçün istifadə edin. 


Technical details of interrupts
-------------------------------
Switch callback-in işləmə prinsiplərinə nəzər yetirək.
``sw.callback()`` -i çağırdıqda, switch qoşulduğu pin-də,
xarici interrupt trigger-i yaradır.
Bu o deməkdir ki, mikro kontroller hər hansı baş vermiş dəyişiklik üçün,
pin-i izləyir və aşağıdakılar baş verir :

1. Switch düyməsi sıxıldıqda, pin-də dəyişiklik baş verir(pin aşağıdan yuxarı doğru gedir)
    və mikro kontroller bu dəyişikliyi qeyd edir.
2. Mikro controller, hal-hazırkı maşın göstərişinin (machine instruction) çalışmasını başa vurur,
    onu dayandırır, onun indiki halını (current state) yadda saxlayır (pushes the registers on the stack).
    Bu sizin Python scriptinizi pause etməkdə kömək ola bilər.
3. -
The microcontroller starts executing the special interrupt handler
   associated with the switch's external trigger.  This interrupt handler
   get the function that you registered with ``sw.callback()`` and executes
   it.
4. Sizin callback funksiyanız bitənə qədər çalışır, ən sonda kontrolu,
    switch interrupt handler-ə qaytarır.

5. -
The switch interrupt handler returns, and the microcontroller is
   notified that the interrupt has been dealt with.
6. Mikro kontroller 2-ci addımda yadda saxladığı vəziyyəti geri qaytarır(bərpa edir).

7. -
Execution continues of the code that was running at the beginning.  Apart
   from the pause, this code does not notice that it was interrupted.

Yuxarıdakı ardıcıllıq, eyni anda bir neçə interrupt baş verdiyi halda daha da mürəkkəbləşir.
Belə olan halda, yüksək prioriteti olan interrupt birinci gəlir, daha sonra digərləri eyni qayda ilə davam edir.
Switch interrupt-a ən aşağı prioritet verilib.
