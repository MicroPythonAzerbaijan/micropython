Safe mode and factory reset
===========================
Əgər sizin Pyboard-la nəsə yolunda deyilsə, həyəcanlanmayın.
Səhv kodlaşdırmaq nəticəsinda pyboard-ı xarab etmək demək olar ki, mümkün deyil.

İlk öncə safe mode-a keçməyi sınamalısınız: bu, müvəqqəti olaraq ``boot.py`` və ``main.py``
fayllarının işlənməsini dayandırır və default USB ayarlarını sizə verir.

Əgər sizin fayl sistemlə bağlı probleminiz varsa fabrika tənzimləmələrinə geri dönmək üçün,
factory reset edə bilərsiniz. Factory reset faylsistemi onun ilk günkü orijinal vəziyyətinə geri qaytarır.


Safe mode
---------

Safe mode-a keçmək üçün aşağıdakı addımları atın:

1. Pyboard-ı USB ilə PC-yə qoşun
2. USR switch-i basılı saxlayın.
3. USR basılı ola-ola, RST switch-i basıb buraxın.
4. LED daha sonra yaşıldan narıncıya keçəcək daha sonra da yaşıl+narıncı olacaq və əvvəlki vəziyyətinə qayıdacaq.
5. USR-i basılı saxlamaqda davam edin *yalnız narıncı LED yanılı qalana kimi*, daha sonra da USR switch-i buraxın.
6. Narıncı LED tez-tez 4 dəfə yanıb-sönəcək, daha sonra isə dayanacaq.
7. Siz artıq Safe mode-dasınız.

Safe Mode-da ``boot.py`` və ``main.py`` faylları çalışmır(run olmur), beləliklə də pyboard default ayarlarla qalxır.
Bu o deməkdir ki, indi siz asan şəkildə ``boot.py`` və ``main.py`` fayllarını dəyişə bilərsiniz.
Beləliklə də hər hansı səhvliklər varsa, onları düzəltmə şansını əldə etmiş olursunuz.

Safe Mode-a daxil olmaq müvəqqətidir, bu o deməkdir ki, bu ziyansız əməliyyatdır.

Faylsistemi fabrika tənzimləmələrinə geri qaytarmaq (factory reset)
----------------------------
Əgər pyboard-ın faylsistemi zədələnibsə (məs. eject/unmount etməyi unutmusunuzsa),
və yaxud ``boot.py`` və ya ``main.py`` fayllarında hər hansı kod var ki, onu dayandıra bilmirsiniz,
o zaman siz faylsistemi reset edə bilərsiniz.

Faylsistemi resetləsəniz, daxili puboard yaddaşında olan (SD kart buna daxil deyil),
bütün fayllar silinəcək və ``boot.py``, ``main.py``, ``README.txt``
və ``pybcdc.inf`` faylları özlərinin orijinal vəzziyyətlərinə qayıdacaq.

Factory reset etmək, Safe Mode-a keçmək kimidir, sadəcə USR-i yaşıl+narıncı olanda buraxmaq lazımdır:

1. Pyboard-ı USB ilə PC-yə qoşun.
2. USR switch-i basılı saxlayın.
3. USR basılı ola-ola, RST switch-i basıb buraxın.
4. LED daha sonra yaşıldan narıncıya keçəcək daha sonra da yaşıl+narıncı olacaq və əvvəlki vəziyyətinə qayıdacaq.
5. USR-i basılı saxlamaqda davam edin *yaşıl və narıncı LED yanılı qalana kimi*, daha sonra da USR switch-i buraxın.
6. Yaşıl və narıncı LED-lər 4 dəfə yanıb sönəcək.
7. Qırmızı LED yanacaq (beləliklə indi yaşıl, narıncı və qırmızı işıqlar yanır).
8. İndi pyboard faylsistemi reset edir(bu bir neçə saniyə çəkə bilər).
9. Bütün LED-lər indi sönülüdür.
10. Bəli artıq siz fayl sistemi resetlədiniz və hal-hazırda Safe Mode-da çalışırsınız.
11. Normal şəkildə boot olmaq üçün,RST switch-i basıb buraxın.
