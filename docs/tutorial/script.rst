İlk scriptimizi işlədək
=========================

Gəlin biliklərimizi daha da dərinləşdirək və Python script-i pyboard-da calışdıraq.
Bunu etdikdən sonra görəcəksiniz ki, bəli bütün əsas iş elə bundan ibarətdir.

Pyboar-da qoşulmaq
-----------------------

Pyboard-ı USB kabel vasitəsilə PC-yə qoşaq(Windows, Mac və Linux).
Bunun yalnız bir üsulu var, ona görə də səhv etmə ehtimalınız yoxdur.

<img src="/static/doc/pyboard-usb-micro.jpg" alt="pyboard with USB micro cable" style="width:200px; border:1px solid black; display:inline-block;"/>

Pyboard PC-yə qoşulduqdan sonra boot olacaq. Yarım saniyə və ya daha az bir müddətə yaşıl LED yanacaq.
Yaşıl işıq söndükdən sonra, bilin ki boot proses artıq sonlanıb.

Pyboard USB drayvla işləmək
-----------------------------

Pyboard PC-yə qoşulan kimi, sizin PC onu dərhal tanımalıdır.
Təbii ki, bu sizin əmliyyat sisteminizdən asılıdır. ::

  - **Windows**: Sizin Pyboard USB flash drive kimi görsənəcək.
    Windows avtomatik olaraq pəncərə açacaq və siz ora Explorer-lə gedə bilərsiniz.

    Windows həmçinin görəcək ki, pyboard-ın serial device-ı var və dərhal onu tənzimləməyə çalışacaq.
    Əgər bu baş verərsə, bu prosesi dayandırın.
    Biz serial device-la işləməyi növbəti dərsdə göstərəcik.

  - **Mac**: Pyboard desktop-da removable disk kimi görsənəcək.
    Çox böyük ehtimal onun adı "NONAME" olacaq. Onun üzərinə tıklayın və qovluğu açın.

  - **Linux**: Pyboard removable medium kimi görsənəcək.
    Ubuntu-da avtomatik olaraq, pyboard qovluğu pop-up kimi açılacaq.
    Digər Linux distributivlərində, pyboard avtomatik olaraq mount ola bilər.
    Əgər avtomatik olaraq mount olmursa, o zaman bunu əllə etməli olacaqsınız.
    Terminalda, ``lsblk`` yazıb qoşulmuş drayverlərin siyahısını görə bilərsiniz.
    Daha sonra ``mount /dev/sdb1`` yazmaqla (``sdb1`` -ni müvafiq devays adı ilə dəyişin)
    Bunları edə bilmək üçün root olmaq tələb oluna bilər.

İndi Pyboard PC-yə USB flash drive kimi qoşulub və siz artıq pyboard-ın faylları olan qovluğu görürsünüz.

Sizin hal-hazırda olduğunuz drayv ``/flash``-dir.
Daxilində 4 fayl olmalıdır:


  - [``boot.py``](/static/doc/fresh-pyboard/boot.py) -- Bu script pyboard boot olanda çalışır.
    Script müxtəlif konfiqurasiya opsiyalarını tətbiq edir.

  - [``main.py``](/static/doc/fresh-pyboard/main.py) -- Bu sizin Python kodlarınızı özündə saxlayan əsas scriptdir.
    Bu script ``boot.py`` -dan sonra çalışır.

  - [``README.txt``](/static/doc/fresh-pyboard/README.txt) -- Adından da göründüyü kimi, işə başlamanız üçün lazım olan,
    ilkin məlumatları bu faylda tapa bilərsiniz.
    
  - [``pybcdc.inf``](/static/doc/fresh-pyboard/pybcdc.inf) -- Bu fayl isə Windows drayverdir.
    Daha ətraflı bu barədə növbəti dərsdə danışacıq.


``main.py`` -da dəyişikliklər.
-------------------

İndi biz Python proqram yazmağa çalışacıq. ``main.py`` faylını text editorla açın.
Windows-da notepad və ya digər editoru istifadə edə bilərsiniz.
Mac və Linux -da isə sevimli editorunuzdan istifadə edə bilərsiniz.
Faylı açdıqda cəmi 1 sətr kod olduğunu görəcəksiniz: ::

    # main.py -- put your code here!

Bu sətr #-lə başlayır, Python-da bu *comment*-dir.
Əgər sizin proqramlaşdırma təcrübəniz varsa, artıq bilirsiniz ki,
*comment* heç bir şey etmir, bu kodda əlavə məlumatların qeyd olunması üçün istifadə olunur.

Gəlin bu ``main.py`` faylına aşağıdakı 2 sətri əlavə edək: ::

    # main.py -- put your code here!
    import pyb
    pyb.LED(4).on()

2-ci sətr ``pyb`` modulundan istifadə etmək istədiyimiz göstərir.
Yəni bu modulu biz import edirik.
Bu modul, pyboard-ın bütün imkanlarını özündə saxlayan class və funksiyaları özündə saxlayır.

3-cü sətrdə isə, biz 4-cü LED-i (mavi işıq) on() funskiyası ilə yandırırıq.
``LED`` class-ını ``pyb`` modulundan götürüb, parametr olaraq ona 4 ötürüb,
daha sonra da onu yandırır(on())


Resetting the pyboard
---------------------

Bu sadə scripti işlətmək üçün, ``main.py`` faylını yaddaşa verin və bağlayın.
Daha sonra USB-ni eject(unmount, çıxarmaq) edin.
Adi usb flash-də etdiyiniz kimi.

Drayv təhlükəsiz şəkildə, eject(unmount, çıxarmaq) olunduqdan sonra:
Reset etmək və scripti run etmək üçün,RST switch-i basın.
RST switch USB konnektorun altında,sağ kənarda olan kiçik qara düyməcikdir.

RST switch-i basdıqdan sonra yaşıl LED tez-tez yanıb sönməyə başlayacaq.
Daha sonra da mavi LED yanacaq (kodumuzda qeyd etdiyimiz kimi)

Təbriklər! Siz öz ilk Micro Python proqramınızı yazdınız!