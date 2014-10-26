Akselerometr
=================

Burada akselerometri oxumağı, sağa və sola meyl kimi vəziyyətləri LED- ilə işarə verməyi öyrənəcəksiniz.

Akselerometrin istifadəsi
-----------------------

Pyboard-da löhvənin və hərəkətin bucağını təyin etmək üçün istifadə oluna bilən akselerometer () vardır. x, y, z oxlarının hər biri üçün müxtəlif sensorlar vardır. Akselerometrin qiymətini əldə etmək üçün pyb.Accel() obyektini yaradın və x() metodunu çağırın ::

    >>> accel = pyb.Accel()
    >>> accel.x()
    7

Bu -30 və 30 arasında işarəli tam ədə qaytarır. Nəzərə alın ki, ölçmə çox küylüdür, bu deməkdir ki, hətta löhvəni qüsursuz sakit vəziyyətdə tutsanız belə ölçülən ədəddə müəyyən dəyişikliklər olacaqdır.Buna görə x() metodunun dəqiq qiymətini deyil, müəyyən aralıqda olub olmadığını yoxlamalısınız.

Biz akselerometerdən istifadəni löhvə düz olmadığı halda işığın yandırılmasından başlayacağıq. ::

    accel = pyb.Accel()
    light = pyb.LED(3)
    SENSITIVITY = 3

    while True:
        x = accel.x()
        if abs(x) > SENSITIVITY: 
            light.on()
        else:
            light.off()

        pyb.delay(100)

Biz Accel və LED-in obyektlərini yaradırıq, sonra akselerometrin x oxunun qiymətini əldə edirik. Əgər x-ın qiyməti ``SENSITIVITY`` müəyyən qiymətindən böyükdürsə onda LED-i yandırırıq, əks halda söndürürük.Dövrdə kiçik ``pyb.delay()`` vardır, əks halda x-ın qiyməti ``SENSITIVITY``-ə yaxınlaşan kimi LED sürətlə yanıb sönərək təngə gətirir. Bunu pyboard-da icra etməyə və LED-i yandırıb söndürmək üçün löhvəni sağa və sola meyl etdirməyə çalışın.

**Tapşırıq: Yuxarıdakı proqramı elə dəyişin ki löhvəni daha çox meyl etdirdikcə LED-in işıqlanması artsın.
İPUCU: Qiymətləri miqyaslama ehtiyacınız olacaq, intensivlik 0-255 arasında dəyişir.**

Tarazın hazırlanması
---------------------

Yuxarıdakı misal yalnız x oxu üzrə bucağa həssasdır. Əgər biz ``y()``-in qiymətindən və daha çox LED-dən istifadə etsək pyboard-u taraza çevirə bilərik. ::

    xlights = (pyb.LED(2), pyb.LED(3))
    ylights = (pyb.LED(1), pyb.LED(4))

    accel = pyb.Accel()
    SENSITIVITY = 3

    while True:
        x = accel.x()
        if x > SENSITIVITY: 
            xlights[0].on()
            xlights[1].off()
        elif x < -SENSITIVITY:
            xlights[1].on()
            xlights[0].off()
        else:
            xlights[0].off()
            xlights[1].off()

        y = accel.y()
        if y > SENSITIVITY: 
            ylights[0].on()
            ylights[1].off()
        elif y < -SENSITIVITY:
            ylights[1].on()
            ylights[0].off()
        else:
            ylights[0].off()
            ylights[1].off()

        pyb.delay(100)

Biz x və y oxları üzrə LED obyektlərinin yığımını yaratmaqla başlayırıq. Python-da
yığımlar dəyişilməz obyektlərdir, yəni, onlar yaradıldıqdan sonra dəyişilə bilməzlər.
Biz əvvəlki kimi davam edirik lakin, x-ın qiymətlərinin müsbət və mənfi qiymətlərində
müxtəlif LED-ləri yandırırıq. Sonra eyni şeyi y oxu üçün də edirik. Bu xüsusi çətin şey deyil
lakin işləyir. Bunu öz pyboard-da icra etməyə çalışın və siz lövhənin meylindən asılı
olaraq müxətlif LED-lərin yandığını görməlisiniz.
