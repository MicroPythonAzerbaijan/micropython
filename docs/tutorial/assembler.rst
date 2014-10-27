Sətiriçi assembler
================

Burada Micro Python-da sətriçi assembleri yazmağı öyrənəcəksiniz.

**Qeyd**: bu təkmil dərslikdir, mikrokontrollerlər və assembly dili haqqında 
bir qədər biliyi olanlar üçün nəzərə tutulmuşdur.

Micro Python-da sətriçi assembler vardır. Bu sizə assembly instruksiyalarını
Python funksiyası kimi yazmağa imkan verir və siz onları normal Python funksiyası
kimi çağıra bilərsiniz.

Qiymət qaytarılması
-----------------

Sətiriçi assembly funksiyaları xüsusi funksiya dekoratoru ilə təmin edilir.
Ən sadə misaldan başlayaq::

    @micropython.asm_thumb
    def fun():
        movw(r0, 42)

Bunu skript içində və ya REPL-da daxil edə bilərsiniz. Bu funksiya heç bir
arqument qəbul etmir və 42 ədədini qaytarır. ``r0`` registerdir və funksiyanın
qaytardığı qiymət registerdəki qiymətdir. Micro Python həmişə ``r0``-ı tam ədəd
kimi qəbul edir və onu çağıran üçün integer obyektinə çevirir.

Əgər siz ``print(fun())`` icra etsəniz 42 çap edildiyini görəcəksiniz.

Periferiyaya müraciət
---------------------

Bir qədər daha mürəkkəb misal üçün, LED yandıraq::

    @micropython.asm_thumb
    def led_on():
        movwt(r0, stm.GPIOA)
        movw(r1, 1 << 13)
        strh(r1, [r0, stm.GPIO_BSRRL])

Bu kod bəzi yeni anıayışlar istifadə edir:

  - ``stm`` pyboard-un mikrokontrollerinin registerlərinə asan müraciət 
    etmək üçün sabitlə yığımı ilə təmin edən moduldur. REPL-da ``import stm``
    sonra isə ``help(stm)`` icra etməyə çalışın. Bu sizə bütün mümkün sabitlərin
    siyahısını verəcəkdir.

  - ``stm.GPIOA`` GPIOA periferiyasının yaddaşdakı ünvanıdır.
    Pyboard-da qırmızı LED A portunda, PA13 sancağındadır.

  - ``movwt`` 32 bitlik ədədi registerə daxil edir. Bu iki instruksiyaya - ``movw``
    sonra ``movt`` - çevrilən uyğun funksiyadır. ``movt`` həmçinin alınan qiyməti
    16 bit sağa sürüşdürür.

  - ``strh`` yarım-sözü (16 bit) saxlayır. Yuxarıdakı instruksiya ``r1``-in
    aşağı 16 bitini ``r0 + stm.GPIO_BSRRL`` yaddaş ünvanında saxlayır. Nəticədə
    ``r0`` bitlərinə uyğun şəkildə A portunun sancaqları yüksək rejimə keçirilir.
    Yuxarıdakı nümunəmizdə ``r0``-ın 13-cü biti təyin edilib, beləliklə PA13 yüksək
    rejimə keçirilib. Bu qırmızı LED-i yandırır.

Arqumentlərin qəbulu
-------------------

Inline assembler functions can accept up to 3 arguments.  If they are
used, they must be named ``r0``, ``r1`` and ``r2`` to reflect the registers
and the calling conventions.

Sətriçi assembler funksiyalar 3-ə qədər arqument qəbul edə bilərlər. Əgər
onlar istifadə olunursa onda çağrılma konvensiyalarını və registerləri
göstərmək üçün ``r0``, ``r1`` və ``r2`` kimi adlandırılmalıdırlar.

Bu arqumentlərini əlavə edən funksiyadır::

    @micropython.asm_thumb
    def asm_add(r0, r1):
        add(r0, r0, r1)

Bu ``r0 = r0 + r1`` hesablamasını aparır. Nəticə ``r0``-ə verildiyindən qaytarılan
qiymət də budur. ``asm_add(1, 2)`` yoxlayın, bu 3 qaytarmalıdır.

Dövrlər
-----

Sərlövhələri ``label(my_label)`` vasitəsilə təyin edə bilərik və ``b(my_label)``
ilə ünvanlana və ya ``bgt(my_label)`` kimi şərti ünvanlana bilərik.


Növbəti nümunə yaşıl LED-i yandırıb söndürür, bunu ``r0`` dəfə edir. ::

    @micropython.asm_thumb
    def flash_led(r0):
        # get the GPIOA address in r1
        movwt(r1, stm.GPIOA)

        # get the bit mask for PA14 (the pin LED #2 is on)
        movw(r2, 1 << 14)

        b(loop_entry)

        label(loop1)

        # turn LED on
        strh(r2, [r1, stm.GPIO_BSRRL])

        # delay for a bit
        movwt(r4, 5599900)
        label(delay_on)
        sub(r4, r4, 1)
        cmp(r4, 0)
        bgt(delay_on)

        # turn LED off
        strh(r2, [r1, stm.GPIO_BSRRH])

        # delay for a bit
        movwt(r4, 5599900)
        label(delay_off)
        sub(r4, r4, 1)
        cmp(r4, 0)
        bgt(delay_off)

        # loop r0 times
        sub(r0, r0, 1)
        label(loop_entry)
        cmp(r0, 0)
        bgt(loop1)
