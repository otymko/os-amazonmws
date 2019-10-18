# Взаимодействие с API Amazon MWS

Библиотека помогает интегрироваться с [Amazon MWS](https://developer.amazonservices.com/). Реализует базовый функционал по взаимодействую с площадками на сервисе.

## Установка

### OPM

В консоли выполняем:
```
opm install os-amazonmws
```
### Из файла

Качаем последний релиз со страницы Релизы. Затем из командной строки:
```
opm install -f os-amazonmws*.ospx
```

## Примеры

### Подготовительный период

Для авторизации требуется следущие данные:
* SellerID
* AWSKey
* SecretKey

Их можно получить через личном кабинете в разделе **UserPermissions** -> **Amazon MWS Developer Permissions**.

Подготовительный этап:
```bsl
#Использовать os-amazonmws

// Инициализируем с нужными данными
ИдентификаторПродавца = "";
СекретныйКлюч = "";
КлючAWS = "";

// класс для авторизации
КлиентAmazon = Новый КлиентAmazonMWS(ИдентификаторПродавца, СекретныйКлюч, КлючAWS);

// выполняем какие-то действия
```

## Получение списка товаров

Для примера можем получить список всех товаров на какой-то конкретной площадки. Для этого запрашиваем отчет в Amazon и работаем с ответом.

```bsl
// подготовительный этап
Клиент = Новый КлиентAmazonMWS(ИдентификаторПродавца, СекретныйКлюч, КлючAWS);
// ...

// нужная площадка
Площадка = ПлощадкиAmazon.Германия;
КодОтчета = Клиент.ЗапроситьСписокТоваровПоПлощадке(Площадка, ТипыОтчетовAmazon.ТоварыВесьСписок);

// Пауза 15 секунд. Отчеты не сразу формируются
МодульОбщегоНазначения.СделатьПаузу(15);

// Идентификатор отчета
ИдентификаторОтчета = Клиент.ПолучитьИдентификаторОтчета(Площадка, КодОтчета);

// Конечный результат
ДанныеОтчета = МодульОбщегоНазначения.ТаблицаЗначенийТзТекста(СодержимоеОтчета);
```

Далее можно все преобразовать в НоменклатураAmazon:

```bsl
КоллекцияТоваров = Новый Массив;
Для Каждого СтрокаОтчета Из ДанныеОтчета Цикл
    Номенклатура = Новый НоменклатураAmazon();
    Номенклатура.Заполнить(СтрокаОтчета);
    КоллекцияТоваров.Добавить(Номенклатура);
КонецЦикла;
```

### Общее описание использования

Класс `КлиентAmazonMWS` имеет следующие возможности:
* ЗапроситьОтчет() - запросить формирование отчета;
* ПолучитьИдентификаторОтчета() - получить идентификатор отчета по его коду;
* ПолучитьСодержимоеОтчета() - получить конечный результат по отчету;
* ЗапроситьСписокТоваровПоПлощадке() - запросить формирование отчета `ТипыОтчетовAmazon.ТоварыАктивныйСписок` (`_GET_MERCHANT_LISTINGS_DATA_`).

Класс `НоменклатураAmazon` реализует общее представление о товаре Амазон.
Доступные поля:
* `Наименование` - item-name
* `Описание` - item-description
* `Артикул` - seller-sku
* `Цена` - price
* `Остаток` - quantity
* `ДатаРазмещения` - open-date
* `ТипТовара` - product-id-type
* `ASIN` - asin1
* `Штрихкод` - asin2

Класс `ПлощадкаАмазон` реализует представление площадки в Амазон.

Модуль `ТипыОтчетовAmazon` хранит и позволяет хранить типы отчетов Амазон.

## Контрибьютинг

Все просто:
1. Заводим issue
2. Обсуждаем issue
3. Отправляем PR с решением
4. Изменения в репе. **Profit!**