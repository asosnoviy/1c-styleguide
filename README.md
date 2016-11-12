# 1С: Руководство по стилю оформления

> Почти все убеждены, что любой стиль кроме их собственного ужасен и нечитаем. Уберите отсюда "кроме их собственного" — и они будут, наверное, правы... 
> 
> -- Джерри Коффин (Jerry Coffin) об отступах

## Оформление модулей

- Длина строки в общем случае не должна превышать 120 символов.
- Для отступов необходимо использовать символы табуляции.
- Одна строка кода - одна управляющая конструкция.
 
```
// Плохо:
Если ЭтоБрак Тогда Продолжить; КонецЕсли;

// Хорошо:
Если ЭтоБрак Тогда
  Продолжить;
КонецЕсли;
```
- Следует отделять друг от друга пробелами ключевые слова, вызовы процедур и функций, параметры процедур и функций внутри скобок, операторы.

```
// Плохо:
Сообщить(“Сумма: “+Сумма);

// Хорошо:
Сообщить(“Сумма: “ + Сумма);
```

- Для разделения на логические части внутри модуля следует использовать пустые строки
- При вызове функции с несколькими параметрами при переносе строк необходимо выравнивать параметры по первому

```
// Начальное состояние (строка слишком длинная):
НалоговыйУчет.ОстаткиВременныхРазниц(СтрокаВидАктиваОбязательства, СписокОрганизаций, Реквизиты.НачалоГода, Реквизиты.КонДата);

// Плохо:
НалоговыйУчет.ОстаткиВременныхРазниц(
    СтрокаВидАктиваОбязательства, СписокОрганизаций, Реквизиты.НачалоГода, Реквизиты.КонДата);

// Лучше:
НалоговыйУчет.ОстаткиВременныхРазниц(СтрокаВидАктиваОбязательства,
                                     СписокОрганизаций,
                                     Реквизиты.НачалоГода,
                                     Реквизиты.КонДата);

// Хорошо:
НалоговыйУчет.ОстаткиВременныхРазниц(
    СтрокаВидАктиваОбязательства,
    СписокОрганизаций,
    Реквизиты.НачалоГода,
    Реквизиты.КонДата);
    
```

- Выравнивание однотипных операторов. При следовании друг за другом нескольких однотипных операторов допускается их выравнивание. Выравнивание следует выполнять с помощью пробелов

```
// Хорошо:
НоваяСтрока = ВидыОпераций.Добавить();
НоваяСтрока.ВидОперации         = ВидОперации;
НоваяСтрока.НомерГруппы         = ГруппаПоВидуОперации(ВидОперации);
НоваяСтрока.ПоОрганизацииВЦелом = ГруппаПоОрганизации(НоваяСтрок);
```

## Условия
- Предпочтительней использовать тернарный оператор для простых конструкций.
```
// Плохо:
Если НДС0 Тогда
    Возврат 0;
Иначе
    Возврат 18;
КонецЕсли;

// Хорошо:
Возврат ?(НДС0, 0, 18);
```

- Не допускайте использования вложенных тернарных операторов.
- Сложные условия (содержащие 3  конструкции и более) необходимо выносить в отдельные методы.

```
// Плохо:
Если ИдентификаторОбъекта = "АнализСубконто"
    ИЛИ ИдентификаторОбъекта = "АнализСчета"
    ИЛИ ИдентификаторОбъекта = "ОборотноСальдоваяВедомость"
    ИЛИ ИдентификаторОбъекта = "ОборотноСальдоваяВедомостьПоСчету"
    ИЛИ ИдентификаторОбъекта = "ОборотыМеждуСубконто"
    ИЛИ ИдентификаторОбъекта = "ОборотыСчета"
    ИЛИ ИдентификаторОбъекта = "СводныеПроводки" 
    ИЛИ ИдентификаторОбъекта = "ГлавнаяКнига"
    ИЛИ ИдентификаторОбъекта = "ШахматнаяВедомость" Тогда
    ПараметрыРасшифровки.Вставить("ОткрытьОбъект", Ложь);
		
    ЕстьПоказатель  = Ложь;
    ЕстьКорЗначение = Ложь;
    ЕстьСчет        = Истина;
    Счет            = Неопределено;
    ПервыйЭлемент   = Неопределено;
КонецЕсли;

// Хорошо:
Если ОткрыватьОбъектПриИдентификаторе(ИдентификаторОбъекта) Тогда
    ПараметрыРасшифровки.Вставить("ОткрытьОбъект", Ложь);
		
    ЕстьПоказатель  = Ложь;
    ЕстьКорЗначение = Ложь;
    ЕстьСчет        = Истина;
    Счет            = Неопределено;
    ПервыйЭлемент   = Неопределено;
КонецЕсли;

Функция ОткрыватьОбъектПриИдентификаторе(ИдентификаторОбъекта)
   
    ДопустимыеИдентификаторы = Новый Структура("
    |АнализСубконто,
    |АнализСчета,
    |ОборотноСальдоваяВедомость,
    |ОборотноСальдоваяВедомостьПоСчету,
    |ОборотыМеждуСубконто,
    |ОборотыСчета,
    |СводныеПроводки,
    |ГлавнаяКнига,
    |ШахматнаяВедомость");
    
    Возврат ДопустимыеИдентификаторы.Свойство(ИдентификаторОбъекта);
   
КонецФункции
```

## Методы

- Параметр функции не должен возвращать значение
```
// Плохо:
Если Не ПроверкаПройдена() Тогда
    ОбщегоНазначения.СообщитьОбОшибке(ТекстОшибки, Отказ);
КонецЕсли;

// Хорошо:
Отказ = Не ПроверкаПройдена();
Если Отказ Тогда
    ОбщегоНазначения.СообщитьОбОшибке(ТекстОшибки);
КонецЕсли;
```

## Имена процедур, функций, переменных

- Следуюйте общему подходу именования
```
// Плохо:
этобрак, ЭТОБРАК, этоБрак

// Хорошо:
ЭтоБрак
```

- Не используйте отрицание в именах переменных и методов
```
// Плохо:
Функция ПроверкаНеПройдена()
...

Если Не (Условие И Не ПроверкаНеПройдена()) Тогда

// Хорошо:
Функция ПроверкаПройдена()
...

Если Не Условие И Не ПроверкаПройдена() Тогда
```

## Комментарии
- Если код требует комментария для пояснения работы - в первую очередь необходимо рассмотреть варианты рефакторинга, чтобы код не требовал комментария.
- К комментариям также относятся ограничения на длину строки в 120 символов.
