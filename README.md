*Проект был выполнен в рамках обучения в Яндекс Практикуме на курсе "Специалист по Data Science расширенный".*
*Дата выполнения: февраль 2024 года.*

# Анализ сервиса проката самокатов GoFast

## Введение

GoFast - сервис аренды самокатов, работающий в нескольких городах России. Для совершения поездки пользователь арендует самокат через собственное мобильное приложение сервиса. Возможны два варианта аренды: с подпиской Ultra и без подписки. Подробные условия приведены в таблице: 

|Условия|Без подписки|С подпиской Ultra|
|-|--------|---|
|Абонентская плата|отсутствует|199 рублей|
|Стоимость одной минуты поездки|8 рублей|6 рублей|
|Стоимость старта|50 рублей|бесплатно|



### Цели анализа 
1. Установить, кто пользуется сервисом. 
2. Установить, какие факторы влияют на выручку сервиса.  

Результаты работы могут быть использованы для повышение рентабельности бизнеса.

### Задачи, решаемые в ходе анализа
1. Предварительная обработка данных.  
Под обработкой подразумевается:
- приведение типа данных к нужному типу,
- обработка пропусков,
- обработка дубликатов.
2. Выполнение визуализации данных:
- распределение пользователей по городам,
- соотношение пользователей с подпиской и без неё,
- распределение пользователей по возрасту,
- распределение продолжительности поездок,
- распределение дистанции поездок.
3. Подсчет помесячной выручки от пользователей с подпиской и без подписки.
4. Проверка гипотез касательно сервиса:
- тратят ли пользователи с подпиской больше времени на поездки,
- среднее расстояние, которое проезжают пользователи с подпиской за одну поездку, не превышает 3130 метров,
- помесячная выручка от пользователей с подпиской по месяцам выше, чем выручка от пользователей без подписки.

### Описание начальных данных   


Для анализа предоставлены три таблицы с данными.
1. Таблица с информацией о пользователях (файл "users_go.csv").

|Столбец|Описание|
|-|--------|
|user_id|уникальный идентификатор пользователя|
|name|имя пользователя|
|age|возраст|
|subscription_type|тип подписки (free, ultra)|

2. Таблица с информацией о поездках (файл "rides_go.csv").

|Столбец|Описание|
|-|--------|
|user_id|уникальный идентификатор пользователя|
|distance|расстояние, которое проехали в текущей сессии (в метрах)|
|duration|продолжительность сессии (в минутах)|
|date|дата совершения поездки|


3. Таблица с информацией о подписках (файл "subscriptions_go.csv").

|Столбец|Описание|
|-|--------|
|subscription_type|тип подписки|
|minute_price|стоимость одной минуты поездки|
|start_ride_price|стоимость начала поездки|
|subscription_fee|стоимость ежемесячного платежа|

### План работы
1. Загрузка данных.
2. Предварительная обработка.  
3. Выполнение визуализации в соответствии с задачами.
4. Подсчет помесячной выручки от пользователей с подпиской и без подписки.
5. Проверка ряда гипотез.

### Использованные библиотеки в работе
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
from scipy import stats as st
from scipy.stats import binom, norm
from math import sqrt

## Заключение и общие выводы

### Обработка данных  

В рамках работы были обработаны данные из трех файлов: 
- сведения о пользователях, 
- сведения о поездках, 
- сведения о подписках.  

В начальных данных пропусков не было обнаружено. Вместе с тем, в файле о поездках были найдены аномальные значения. Так, в файле было 95 строк, в которых расстояние поездки составляло более 4 километров, но время поездки было всего 0,5 минуты.

### Анализ данных и проверка гипотез.  

Были построены визуализации и определено следующее:
- больше всего пользователей сервиса из Пятигорска. Всего сервис представлен в 8 городах.
- среди пользователей сервиса большинство не пользуется подпиской (54%).
- сервисом пользуются люди от 12 до 43 лет.
- средняя дистанция, на которую совершают поездки, около 3 километров. При этом средняя скорость движения составляет 10 км/ч.  

Сформулированы и проверены следующие гипотезы:
- пользователи с подпиской тратят больше времени на поездки. Вывод: скорее всего, так и есть. Проверка гипотезы были осуществлена с помощью двухвыборочного t-теста для независимых выборок.
- пользователи с подпиской ездят меньшее, чем 3130 метров за одну поездку. Вывод: не удалось опровергнуть. Скорее всего, так и есть. Проверка гипотезы была осуществлена одновыборочным t-тестом.
- пользователи с подпиской приносят больше выручки, чем пользователи без подписки. Вывод: не удалось опровергнуть. Вероятно, пользователи с подпиской приносят больше выручки. Проверка гипотезы была осуществлена с помощью двухвыборочного t-теста для независимых выборок.  

Решены задачи: 
- установлено необходимое количество промокодов: минимум 1173 промокода.
- установлена вероятность, что уведомления откроют менее 399 500 раз: 15,4%.

### Рекомендации по улучшению и уточнению анализа  


В целях уточнения результатов анализа рекомендуется:
- предоставить данные за другие временные промежутки.
- предоставить данные после модернизации приложения сервиса. О модернизации приложения говорилось в п. 7.4.
- проверить алгоритм сбора и выгрузки данных на возможные ошибки. Указанные выше 95 строк с аномально большим расстоянием при аномально малом времени возможно связаны с ошибкой в алгоритмах сбора и выгрузки данных.