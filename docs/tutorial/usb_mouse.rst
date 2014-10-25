Pyboard-ı USB mouse kimi istifadə etmək
=====================================

Pyboard USB devays olduğu üçün, onu həmçinin mouse kimi istifadə edə bilərsiniz.
Buna nail olmaq üçün, USB tənzimləmələri ``boot.py`` faylından dəyişmək lazımdır.
Əgər hələ də, ``boot.py`` faylı ilə işləməməsinizsə o zaman fayl aşağıdakı kimidir ::

    # boot.py -- boot-up zamanı çalışır
    # can run arbitrary Python, but best to keep it minimal

    import pyb
    #pyb.main('main.py') # main script to run after this one
    #pyb.usb_mode('CDC+MSC') # act as a serial and a storage device
    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Mouse mode-u aktivləşdirmək üçün, ən sonuncu sətrdəki comment-i silin. ::

    pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Əgər ``boot.py`` faylını nə vaxtsa dəyişmisinizsə, o zaman minimum kod,
aşağıdakı kimi olacaq ::

    import pyb
    pyb.usb_mode('CDC+HID')

Bu kod, pyboard boot-up olanda özünü CDC (serial) və HID
(human interface device, bizim halda mouse) kimi tənzimləyir.

Pyboard Eject/unmount edin və daha sonra da RST switch ilə reset edin.
İndi sizin PC-niz pyboard-ı mouse kimi görəcək! 

Mouse event-lərin əllə ötürülməsi
----------------------------

To get the py-mouse to do anything we need to send mouse events to the PC.
We will first do this manually using the REPL prompt.  Connect to your
pyboard using your serial program and type the following::

    >>> pyb.hid((0, 10, 0, 0))

Your mouse should move 10 pixels to the right!  In the command above you
are sending 4 pieces of information: button status, x, y and scroll.  The
number 10 is telling the PC that the mouse moved 10 pixels in the x direction.

Let's make the mouse oscillate left and right::

    >>> import math
    >>> def osc(n, d):
    ...   for i in range(n):
    ...     pyb.hid((0, int(20 * math.sin(i / 10)), 0, 0))
    ...     pyb.delay(d)
    ...
    >>> osc(100, 50)

The first argument to the function ``osc`` is the number of mouse events to send,
and the second argument is the delay (in milliseconds) between events.  Try
playing around with different numbers.

**Excercise: make the mouse go around in a circle.**

Making a mouse with the accelerometer
-------------------------------------

Now lets make the mouse move based on the angle of the pyboard, using the
accelerometer.  The following code can be typed directly at the REPL prompt,
or put in the ``main.py`` file.  Here, we'll put in in ``main.py`` because to do
that we will learn how to go into safe mode.

At the moment the pyboard is acting as a serial USB device and an HID (a mouse).
So you cannot access the filesystem to edit your ``main.py`` file.

You also can't edit your ``boot.py`` to get out of HID-mode and back to normal
mode with a USB drive...

To get around this we need to go into *safe mode*.  This was described in
the [safe mode tutorial](tut-reset), but we repeat the instructions here:

1. Hold down the USR switch.
2. While still holding down USR, press and release the RST switch.
3. The LEDs will then cycle green to orange to green+orange and back again.
4. Keep holding down USR until *only the orange LED is lit*, and then let
   go of the USR switch.
5. The orange LED should flash quickly 4 times, and then turn off.  
6. You are now in safe mode.

In safe mode, the ``boot.py`` and ``main.py`` files are not executed, and so
the pyboard boots up with default settings.  This means you now have access
to the filesystem (the USB drive should appear), and you can edit ``main.py``.
(Leave ``boot.py`` as-is, because we still want to go back to HID-mode after
we finish editting ``main.py``.)

In ``main.py`` put the following code::

    import pyb

    switch = pyb.Switch()
    accel = pyb.Accel()

    while not switch():
        pyb.hid((0, accel.x(), accel.y(), 0))
        pyb.delay(20)

Save your file, eject/unmount your pyboard drive, and reset it using the RST
switch.  It should now act as a mouse, and the angle of the board will move
the mouse around.  Try it out, and see if you can make the mouse stand still!

Press the USR switch to stop the mouse motion.

You'll note that the y-axis is inverted.  That's easy to fix: just put a
minus sign in front of the y-coordinate in the ``pyb.hid()`` line above.

Restoring your pyboard to normal
--------------------------------

If you leave your pyboard as-is, it'll behave as a mouse everytime you plug
it in.  You probably want to change it back to normal.  To do this you need
to first enter safe mode (see above), and then edit the ``boot.py`` file.
In the ``boot.py`` file, comment out (put a # in front of) the line with the
``CDC+HID`` setting, so it looks like::

    #pyb.usb_mode('CDC+HID') # act as a serial device and a mouse

Save your file, eject/unmount the drive, and reset the pyboard.  It is now
back to normal operating mode.
