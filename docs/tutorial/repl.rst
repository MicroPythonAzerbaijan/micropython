Micro Python REPL prompt-la tanışlıq
==================================
REPL = Read Evaluate Print Loop, pyboard daxilində olan interaktiv Micro Python prompt-dur.  
REPL-dən istifadə etmək, kodları test etməyin ən asan yoludur. 
``main.py``-a kod yazmaqdan əlavə REPL-dan istifadə edə bilərsiniz.

REPL-dən istifadə üçün, pyboard-ı USB ilə PC-yə qoşmalısınız.
Bunu necə edəcəyiniz isə, sizin əməliyyat sisteminizdən asılıdır.

Windows
-------
USB device-dan istifadə etmək üçün, pyboard driver-i install etməlisiniz.
Driver pyboard-ın USB flash drayvında yerləşir və onun adı ``pybcdc.inf``-dır.

Driver-i qurmaq üçün, Device Manager-ə gedib, pyboard-ı həmin listdən tapmaq lazımdır.
Pyboard olan yerdə warning işarəsi görəcəksiniz (çünki hələ işləmir),
sağ klik edərək -> Properties -> Install Driver seçin.
Daha sonra, manual olaraq driver-in olduğu yerə gedin və pyboard-ın USB drayvını seçin.
Seçdikdən sonra, yazılmalıdır.  
Drayveri qurduqdan sonra yenidən Device Manager-e gedib install olunmuş pyboard-u tapın
və hansı COM port olduğunu müəyyən edin (məs. COM4).

İndi siz terminal proqramınızdan istifadə etməlisiniz.
HyperTerminal buna nümunə ola bilər. Pulsuz olaraq isə PuTTY-dən istifadə edə bilərsiniz:
[`putty.exe`](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).
Using your serial program you must connect to the COM port that you found in the
previous step.  With PuTTY, click on "Session" in the left-hand panel, then click
the "Serial" radio button on the right, then enter you COM port (eg COM4) in the
"Serial Line" box.  Finally, click the "Open" button.

Mac OS X
--------

Open a terminal and run::

    screen /dev/tty.usbmodem*
    
When you are finishend and want to exit screen, type CTRL-A CTRL-\\.

Linux
-----

Open a terminal and run::

    screen /dev/ttyACM0
    
You can also try ``picocom`` or ``minicom`` instead of screen.  You may have to
use ``/dev/ttyACM1`` or a higher number for ``ttyACM``.  And, you may need to give
yourself the correct permissions to access this devices (eg group ``uucp`` or ``dialout``,
or use sudo).

Using the REPL prompt
---------------------

Now let's try running some Micro Python code directly on the pyboard.

With your serial program open (PuTTY, screen, picocom, etc) you may see a blank
screen with a flashing cursor.  Press Enter and you should be presented with a
Micro Python prompt, i.e. ``>>>``.  Let's make sure it is working with the obligatory test::

    >>> print("hello pyboard!")
    hello pyboard!

In the above, you should not type in the ``>>>`` characters.  They are there to
indicate that you should type the text after it at the prompt.  In the end, once
you have entered the text ``print("hello pyboard!")`` and pressed Enter, the output
on your screen should look like it does above.

If you already know some python you can now try some basic commands here. 

If any of this is not working you can try either a hard reset or a soft reset;
see below.

Go ahead and try typing in some other commands.  For example::

    >>> pyb.LED(1).on()
    >>> pyb.LED(2).on()
    >>> 1 + 2
    3
    >>> 1 / 2
    0.5
    >>> 20 * 'py'
    'pypypypypypypypypypypypypypypypypypypypy'

Resetting the board
-------------------

If something goes wrong, you can reset the board in two ways. The first is to press CTRL-D
at the Micro Python prompt, which performs a soft reset.  You will see a message something like ::

    >>> 
    PYB: sync filesystems
    PYB: soft reboot
    Micro Python v1.0 on 2014-05-03; PYBv1.0 with STM32F405RG
    Type "help()" for more information.
    >>>

If that isn't working you can perform a hard reset (turn-it-off-and-on-again) by pressing the RST
switch (the small black button closest to the micro-USB socket on the board). This will end your
session, disconnecting whatever program (PuTTY, screen, etc) that you used to connect to the pyboard.

If you are going to do a hard-reset, it's recommended to first close your serial program and eject/unmount
the pyboard drive.
