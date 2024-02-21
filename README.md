# Расчет доставки за МКАД с автозаполнием полей формы для tilda.

Скрипт написан на js для CMS Tilda Publishing. Реализован следующий функционал:

- отображение автоподсказки для полей формы: город, улица, дом
- расчет расстояния до МКАД:
  - обработаны случаи внутри МКАД
  - зона до и после 200 км от МКАД
- вывод сообщения внизу формы с информацией о доставке
- добавление товара “Доставка” в корзину, с подсчетом суммы товаров в корзине и возможностью безналичной оплаты

## Вставка и подключение кода к сайту

В коде используется сервис dadata и API Яндекс.Карт. Для подключения сервисов нужно в HTML-КОД ДЛЯ ВСТАВКИ ВНУТРЬ HEAD вставить следующий код, [подробнее в разделе документации к Tilda](https://help-ru.tilda.cc/html#:~:text=%D0%9A%D0%B0%D0%BA%20%D0%B4%D0%BE%D0%B1%D0%B0%D0%B2%D0%B8%D1%82%D1%8C%20HTML%2D%D0%BA%D0%BE%D0%B4%20%D0%B2,%D0%BA%D0%BE%D0%B4%20%D0%B4%D0%BB%D1%8F%20%D0%B2%D1%81%D1%82%D0%B0%D0%B2%D0%BA%D0%B8%20%D0%B2%D0%BD%D1%83%D1%82%D1%80%D1%8C%20head%C2%BB):

```
<script src="https://api-maps.yandex.ru/2.1/?apikey=ваш API-ключ&lang=ru_RU"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
<link href="http://cdn.jsdelivr.net/npm/suggestions-jquery@22.6.0/dist/css/suggestions.min.css" rel="stylesheet"/>
<script src="http://cdn.jsdelivr.net/npm/suggestions-jquery@22.6.0/dist/js/jquery.suggestions.min.js"></script>

```

В верхней части скрипта добавлены переменные для быстрой настройки скрипта под другую форму.

Для настройки автозаполнения полей с помощью сервиса dadata нужно заполнить следующие переменные, [подробнее в документации к dadata](https://dadata.ru/suggestions/usage/address/): 

```
//suggest settings
      const CITY_INPUT_ID = "input_1586189295013";
      const STREET_INPUT_ID = "input_1708237620456";
      const HOUSE_INPUT_ID = "input_1586517665571";
      const suggestToken = "e94b27193053163c8724e9b2cf1548a086eb95be";
```

Для настройки maps-api нужно заполнить следующие переменные, [подробнее в документации к maps-api](https://yandex.ru/dev/jsapi-v2-1/doc/ru/): 
```
const result = document.querySelector(
        "div[data-input-lid='1586265419159']"
      ).firstElementChild.firstElementChild.firstElementChild;  //Блок для вывода сообщения о доставке

      let point = [55.547281, 44.438658];   // Точка по умолчанию, в данном случае задан центр Москвы. 
      const deliveryRate = 30;             // Стоимость доставки за км в рублях
      const IS_IN_MKAD = "Доставка в пределах МКАД бесплатная.";     // Текст сообщения для блока result
      const deliveryLimitKM = 200;   // Ограничение по зоне доствки от МКАД в км.
      const IS_TO_FAR = `Доставка более ${deliveryLimitKM}км от МКАД не осуществляется.`;   // Текст сообщения для блока result
```

Весь код из файла index.html нужно вставить в блок T123, [подробнее в докуметации к tilda](https://help-ru.tilda.cc/html):

```
<div id="map"></div>
<script>
...
//Весь код из тега script
...
</script>
```
