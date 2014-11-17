:mod:`gc` -- tullantı kollektorunu idarə edin
=============================================

.. module:: gc
   :synopsis: tullantı kollektorunu idarə edin

Funksiyalar
---------

.. function:: enable()

   Avtomatik tullantı kollektorunu işə salın.

.. function:: disable()

   Avtomatik tullantı kollectorunu söndürün.  Yaddaş kütləsi hələ də istifadə oluna bilər,
   tullantı kollektoru :meth:`gc.collect` vasitəsilə istifadəçi tərəfindən qoşula bilər.

.. function:: collect()

   Tullantı toplusunu işə salın.

.. function:: mem_alloc()

   Ayrılmış RAM kütləsinin baytlarla sayını qaytarın.

.. function:: mem_free()

   İstifadə olunmamış RAM kütləsinin baytlarla sayını qaytarın.
