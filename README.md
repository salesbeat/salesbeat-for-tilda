# Интеграция Salesbeat в магазин на Tilda

[Salesbeat](https://salesbeat.pro/) — сервис, позволяющий интегрировать службы доставки России в интернет-магазины.
Salesbeat позволяет показывать точные срок и стоимость доставки в товарной карточке,
выбирать способы доставки при оформлении заказа, а также выгружать заказы в информационные
системы (личные кабинеты) служб доставки.

Вот несколько примеров магазинов на Тильда, использующих Salesbeat:
* [kitchenceremony.com](https://kitchenceremony.com/)
* [store.meilleur.ru](https://store.meilleur.ru/)
* [pekarskiy-kamen.ru](https://pekarskiy-kamen.ru/)
* [neprostokorobka.ru](https://neprostokorobka.ru)

В данном документе мы покажем, как проинтегрировать Salesbeat в интернет-магазин
на Тильде (Tilda Publishing).

[![Подробная видео-инструкция по процессу интеграции](https://img.youtube.com/vi/G1x2gP7TVx0/0.jpg)](https://www.youtube.com/watch?v=G1x2gP7TVx0).

[Актуальная версия документации находится здесь](https://salesbeat.pro/integrations/tilda)


## Устанавливаем Salesbeat

Первым шагом на страничке «шапки» или «подвала» добавляем
html-блок («Библиотека блоков» > «Другое» > «T123 HTML-код») со следующим содержанием:


```html
<!-- BEGIN SALESBEAT CONFIG FOR TILDA -->
<script>
    var salesbeatTildaConfig = {
        token: 'YOUR_API_TOKEN',
        weight: 500,  // усрединённая для магазина масса товаров (в граммах)
        x: 20, y: 20, z: 10  // усредненный размер (в см)
    }
</script>
<script src="https://app.salesbeat.pro/static/widget/js/widget.js"></script>
<!-- END SALESBEAT CONFIG FOR TILDA -->
```

Если Вы используете [каталог товаров](http://blog.tilda.cc/catalog), то данный код, как и все последующие
нужно вставлять в head страницы («Настройки сайта» –> «Ещё» –> «HTML-код для вставки внутрь HEAD»).

Отредактируем вставленный текст – зададим API-токен из личного кабинета Salesbeat (его нужно вставить
вместо `'YOUR_API_TOKEN'`) и усреднённую массу и размеры товаров (`x`, `y`, `z` – длина, высота и ширина).
Если вес и размеры товаров различны – вот [решение](https://salesbeat.pro/integrations/tilda/extended#different-weight-and-sizes).


## Добавляем Salesbeat в карточку товара

Для того, чтобы добавить Salesbeat в карточку товара нужно в html-блок (из первого шага) добавить следующее содержание:

```html
<!-- BEGIN SALESBEAT WIDGET FOR TILDA -->
<style>
    /* Тут можно стилизировать виджет */
    .salesbeat-deliveries {
        margin-top: 30px;
    }
</style>
<script src="https://app.salesbeat.pro/static/tilda/salesbeat-tilda-widget-v1.0.js"></script>
<!-- END SALESBEAT WIDGET FOR TILDA -->
```

После изменений не забудьте переопубликовать все страницы (кнопка «Опубликовать все страницы» на странице с проектом).

Если нет ни «шапки», ни «подвала» нужно добавить данный html-блок на каждой товарной страничке.


## Добавляем Salesbeat в корзину

Для того, чтобы добавить Salesbeat в корзину нужно перейти к редактированию «контента» корзины и добавить там два «поля для ввода».

Первое заполнить следующими полями (если уже есть поле с типом «Варианты доставки» – его нужно удалить или выключить):
  * Тип – «Варианты доставки»
  * Заголовок поля – «Доставка»
  * Варианты значения — две строки, первая строка `Salesbeat = 777`, вторая `Dismiss`. Посмотрите в 
[видео](https://www.youtube.com/watch?v=G1x2gP7TVx0).
  * Имя переменной – `Delivery`
  * И поставить галочку «Обязательно для заполнения»

Второе поле:
  * Тип – «Скрытое поле»
  * Имя переменной – `SalesbeatDeliveryInformation`

Далее, в html-блок (из первого шага) добавить:

```html
<!-- BEGIN SALESBEAT CART-WIDGET FOR TILDA -->
<style>
    /* Тут можно стилизировать виджет корзины */
    #sb-cart-widget .wdel2 { font-family: "Open Sans", Arial, sans-serif; }
    #sb-cart-widget .wdel2-inner {
        border: transparent;
        border-radius: 0;
    }
    .t706__cartwin_showed { z-index: 9999; }
    .t450__burger_container { z-index: 9998; }
</style>
<script src="https://app.salesbeat.pro/static/widget/js/cart_widget.js"></script>
<script src="https://app.salesbeat.pro/static/tilda/salesbeat-tilda-cart-widget-v1.0.js"></script>
<!-- END SALESBEAT CART-WIDGET FOR TILDA -->
```

После изменений не забудьте переопубликовать все страницы (кнопка «Опубликовать все страницы» на странице с проектом).


При необходимости можно отредактировать внешний вид виджета через CSS. Дополнительные CSS стили для товарного виджета можно добавить как в самой тильде (во вставленный ранее html-блок), так и в админке Salesbeat [https://app.salesbeat.pro/#/widget](https://app.salesbeat.pro/#/widget).

Если что-то не получилось – пишите скорее мне на nikita@salesbeat.pro
