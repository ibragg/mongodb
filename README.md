# mongodb

## Установка, заполнение данными

Устанавливаем MongoDB Compass с официального [сайта](https://www.mongodb.com/try/download/community) . Далее для удобства я создал бесплатный кластер также на [сайте](https://www.mongodb.com/basics/clusters), к которому подключаемся через ранее устанвленный клиент. Создаем новую базу данных. Я выбрал Daily Financial News for 6000+ Stocks из [source](https://www.kaggle.com/miguelaenlle/massive-stock-news-analysis-db-for-nlpbacktests)

![](upload%20data.png)


После того как данные загружены напишем несколько запросов на выборку и изменение данных.

## Запросы

Запросы на выборку:
1) Какие новости публиковало издание GuruFocus 1-го июня по акциям класса А?
```javascript
db.bd.find({date:"2020-06-01 00:00:00", publisher:"GuruFocus",stock:"A"})
```
![result1](query1.png)

2) Сколько всего новостей было опубликовано 1-го июня?
```javascript
db.bd.find({date:"2020-06-01 00:00:00"}).count()
```
![result2](query2.png)

3) Сколько всего было новостей про акции класса А за период с 2009 по 2020 год у издания Zacks?
```javascript
db.bd.find({publisher:"Zacks",stock:"A"}).count()
```
![result3](query3.png)


Запросы на изменение:
1) Переименуем все новости где издатель Zucks на GuruInvest:
```javascript
db.bd.updateMany({publisher:"Zacks",},{$set: {publisher: "GuruInvest"}})
```
После чего проверим количество записей с издателем Zacks:

![result4](query4.png)

2) Поменяем все класс акций А у издателя GuruFocus на класс B
```javascript
db.bd.updateMany({publisher:"GuruFocus",stock:"A"},{$set: {stock:"B"}})
```
После чего делаем проверку

![result5](query5.png)


## Индексы

Сравним производительность до и после индексов.

До(когда индексы только по id, стандартные):

![before](before%20indexes.png)

Создаем индексы по столбцу дат:
```javascript
db.bd.createIndex({date:1})
```
![created](added.png)

Сравниваем производительность после создания индексов:
![after](after%20indexes.png)

Видим ускорение в два раза


# Выводы
MongoDB удобный инструмент для работы с базами данных, который позволяет легко заполнить данные без создания таблиц и сложных схем, однако из-за этого нет джоинов как в sql, что для меня скорее минус. Но это покрывается высокой производительностью и масштабируемостью объема данных. Удобно что можно развернуть базу данных на облаке. Индексы кратно повышают производительность запросов, что довольно obvious.
