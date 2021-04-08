---
layout: post
title: "Google BigQuery DateTime функции"
crawlertitle: "This is our first post"
summary: "BigQuery Date и Time"
date:   2021-04-01 23:09:47 +0700
categories: posts
tags: ['Продуктовая аналитика']
#author: Felipe
---


Анализируя маркетинговые данные нам довольно часто приходится отслеживать изменения, которые происходят в определенные промежутки времени. Для тех кто плотно работает с облачной базой данных Google BigQuery и анализирует в ней данные из сервисов аналитики Google Analytics или Firebase, а сейчас уже и Google Analytics 4. Не секрет, что для работы с датами и временем Google BigQuery предоставляет
довольно большой набор функций. Конечно много функций — это хорошо, но когда начинаешь разбираться, как они работают периодически, встречаются определенные сложности. Поискав на просторах интернета хороший мануал по работе с этими функциями ничего нормального мне так и не удалось найти в связи с этим решил немного исправить данную ситуацию. В данном мануале постараюсь внести ясность, как работать с этими функциями на примере работы с базами данных Google Analytics и Firebase. Также постараюсь ответить на вопрос в чем основная сложность при работе с этими функциями и дам небольшую шпаргалку для работы.


# Функции для работы с датами и временем 

BigQuery поддерживает 4 основных типа данных даты и времени:

* DATE: YYYY-MM-DD  (2021-01-01)
* DATETIME: YYYY-MM-DD HH:MM:SS (2020-01-01 13:04:11)
* TIME: HH:MM:SS (13:04:11)
* TIMESTAMP: YYYY-MM-DD HH:MM:SS [timezone] (UTC 2020-01-01 13:04:11-5:00)

### Функции для типа Date:
* CURRENT_DATE 
* EXTRACT (part FROM date)
* DATE 
* DATE_ADD (date, INTERVAL … )
* DATE_SUB
* DATE_DIFF
* DATE_TRUNC
* DATE_FROM_UNIX_DATE
* FORMAT_DATE
* LAST_DAY
* PARSE_DATE
* UNIX_DATE

#### CURRENT_DATE - Получаем текущую дату

> ``CURRENT_DATE([time_zone])``

{% highlight sql %}
select current_date; -- date
{% endhighlight %}

##### Return Data Type

DATE

#### EXTRACT (part FROM date) - Возвращает день, месяц, год. 
Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``EXTRACT(part FROM date_expression)``

{% highlight sql %}
SELECT
  EXTRACT(DAY FROM today ) AS the_day,
  EXTRACT(MONTH FROM today ) AS the_month,
  EXTRACT(YEAR FROM today ) AS the_year
FROM (
  SELECT
    CURRENT_DATE() AS today,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
{% endhighlight %}

##### Return Data Type

INT64

#### DATE - Создает ДАТУ из значений INT64, год, месяц и день.

> ``1. DATE(year, month, day)`` 
> ``2. DATE(timestamp_expression[, timezone])`` 
> ``3. DATE(datetime_expression)`` 


{% highlight sql %}
SELECT DATE(2021, 12, 25) as date_ymd,
{% endhighlight %}

##### Return Data Type

DATE

#### DATE_ADD - Добавляет указанный интервал времени к ДАТЕ.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK, DAY

> ``DATE_ADD(date_expression, INTERVAL int64_expression date_part)``

{% highlight sql %}
SELECT
    today,
    DATE_ADD( today, INTERVAL 1 DAY ) AS plus_one_day,
    DATE_ADD( today, INTERVAL 1 WEEK ) AS plus_one_week,
    DATE_ADD( today, INTERVAL 1 MONTH ) AS plus_one_month,
    DATE_ADD( today, INTERVAL 1 QUARTER ) AS plus_one_quarter,
    DATE_ADD( today, INTERVAL 1 YEAR ) AS plus_one_year,
FROM (
  SELECT
     CURRENT_DATE() AS today,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### DATE_SUB - Вычитает указанный временной интервал из ДАТЫ.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK, DAY

> ``DATE_SUB(date_expression, INTERVAL int64_expression date_part)``

{% highlight sql %}
SELECT
    today,
    DATE_SUB( today, INTERVAL 1 DAY ) AS minus_one_day,
    DATE_SUB( today, INTERVAL 1 WEEK ) AS minus_one_week,
    DATE_SUB( today, INTERVAL 1 MONTH ) AS minus_one_month,
    DATE_SUB( today, INTERVAL 1 QUARTER ) AS minus_one_quarter,
    DATE_SUB( today, INTERVAL 1 YEAR ) AS minus_one_year,
FROM (
  SELECT
     CURRENT_DATE() AS today,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### DATE_DIFF - Возвращает разницу между двумя датами timestamp1 и timestamp2.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``DATE_DIFF(date_expression_a, date_expression_b, date_part)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    DATE_DIFF( today, UTC_Time, DAY) AS difference_between_day,
    DATE_DIFF( today, UTC_Time, WEEK ) AS difference_between_week,
    DATE_DIFF( today, UTC_Time, MONTH ) AS difference_between_month,
    DATE_DIFF( today, UTC_Time, QUARTER ) AS difference_between_quarter,
    DATE_DIFF( today, UTC_Time, YEAR ) AS difference_between_year,
    DATE_DIFF( today, UTC_Time, WEEK(SUNDAY)) AS difference_between_week_sunday,
    DATE_DIFF( today, UTC_Time, WEEK(MONDAY)) AS difference_between_week_monday,
    DATE_DIFF( today, UTC_Time, WEEK(TUESDAY)) AS difference_between_week_tuesday,
    DATE_DIFF( today, UTC_Time, WEEK(WEDNESDAY)) AS difference_between_week_wednesday,
    DATE_DIFF( today, UTC_Time, WEEK(THURSDAY)) AS difference_between_week_thusday,
    DATE_DIFF( today, UTC_Time, WEEK(FRIDAY)) AS difference_between_week_friday,
    DATE_DIFF( today, UTC_Time, WEEK(SATURDAY)) AS difference_between_week_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     DATE(CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME)) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

INT64

#### DATE_TRUNC - Усекает дату до указанной степени детализации.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``DATE_TRUNC(date_expression, date_part)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    DATE_TRUNC( UTC_Time, DAY) AS trunc_day,
    DATE_TRUNC( UTC_Time, WEEK ) AS trunc_week,
    DATE_TRUNC( UTC_Time, MONTH ) AS trunc_month,
    DATE_TRUNC( UTC_Time, QUARTER ) AS trunc_quarter,
    DATE_TRUNC( UTC_Time, YEAR ) AS trunc_year,
    DATE_TRUNC( UTC_Time, WEEK(SUNDAY)) AS trunc_sunday,
    DATE_TRUNC( UTC_Time, WEEK(MONDAY)) AS trunc_monday,
    DATE_TRUNC( UTC_Time, WEEK(TUESDAY)) AS trunc_tuesday,
    DATE_TRUNC( UTC_Time, WEEK(WEDNESDAY)) AS trunc_wednesday,
    DATE_TRUNC( UTC_Time, WEEK(THURSDAY)) AS trunc_thusday,
    DATE_TRUNC( UTC_Time, WEEK(FRIDAY)) AS trunc_friday,
    DATE_TRUNC( UTC_Time, WEEK(SATURDAY)) AS trunc_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     DATE(CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME)) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### DATE_FROM_UNIX_DATE - интерпретирует целое число как количество дней с 01.01.1970.

> ``DATE_FROM_UNIX_DATE(int64_expression)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
--     DATE_FROM_UNIX_DATE() Example #1
    DATE_FROM_UNIX_DATE( 1 ) AS one_utc_day,
    DATE_FROM_UNIX_DATE( 2 ) AS two_utc_day,
    DATE_FROM_UNIX_DATE( 3 ) AS three_utc_day,
    DATE_FROM_UNIX_DATE( 4 ) AS four_utc_day,
    DATE_FROM_UNIX_DATE( 5 ) AS five_utc_day,
--     DATE_FROM_UNIX_DATE() Example #2
    DATE_FROM_UNIX_DATE( 18500 ) AS one_utc_day,
    DATE_FROM_UNIX_DATE( 18501 ) AS two_utc_day,
    DATE_FROM_UNIX_DATE( 18502 ) AS three_utc_day,
    DATE_FROM_UNIX_DATE( 18503 ) AS four_utc_day,
    DATE_FROM_UNIX_DATE( 18504 ) AS five_utc_day,
--     DATE_FROM_UNIX_DATE() Example #3
    DATE_FROM_UNIX_DATE( -1 ) AS one_utc_day,
    DATE_FROM_UNIX_DATE( -2 ) AS two_utc_day,
    DATE_FROM_UNIX_DATE( -3 ) AS three_utc_day,
    DATE_FROM_UNIX_DATE( -4 ) AS four_utc_day,
    DATE_FROM_UNIX_DATE( -5 ) AS five_utc_day,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### FORMAT_DATE - функция конвертирует дату в форматированный текст.

[Поддерживаемые элементы в format_string](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#supported_format_elements_for_date)

> ``FORMAT_DATE(format_string, date_expr)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
--     FORMAT_DATE() Example #1
    FORMAT_DATE('%B', UTC_Time) AS date_to_month_name,
--     FORMAT_DATE() Example #2
    FORMAT_DATE('%r', UTC_Time) AS date_to_am_pm,
--     FORMAT_DATE() Example #3
    FORMAT_DATE('%m', UTC_Time) AS date_to_month_decimal,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

STRING

#### LAST_DAY - Возвращает последний день из даты.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``LAST_DAY(date_expression[, date_part])``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    LAST_DAY( UTC_Time, WEEK ) AS last_day_week,
    LAST_DAY( UTC_Time, MONTH ) AS last_day_month,
    LAST_DAY( UTC_Time, QUARTER ) AS last_day_quarter,
    LAST_DAY( UTC_Time, YEAR ) AS last_day_year,
    LAST_DAY( UTC_Time, WEEK(SUNDAY)) AS last_day_sunday,
    LAST_DAY( UTC_Time, WEEK(MONDAY)) AS last_day_monday,
    LAST_DAY( UTC_Time, WEEK(TUESDAY)) AS last_day_tuesday,
    LAST_DAY( UTC_Time, WEEK(WEDNESDAY)) AS last_day_wednesday,
    LAST_DAY( UTC_Time, WEEK(THURSDAY)) AS last_day_thusday,
    LAST_DAY( UTC_Time, WEEK(FRIDAY)) AS last_day_friday,
    LAST_DAY( UTC_Time, WEEK(SATURDAY)) AS last_day_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     DATE(CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME)) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### PARSE_DATE - Преобразует строковое представление даты в объект DATE.
[Поддерживаемые элементы в format_string](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#supported_format_elements_for_date)

> ``PARSE_DATE(format_string, date_string)``

{% highlight sql %}
SELECT
    today,
--     PARSE_DATE() Example #1
    PARSE_DATE('%Y-%m-%d', today) AS parse_date_today,
    event_date,
--     PARSE_DATE() Example #2
    PARSE_DATE('%Y%m%d', event_date) AS parse_date_event_date,
FROM (
  SELECT
     STRING(CURRENT_DATE()) AS today,
     event_date
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### UNIX_DATE - Возвращает количество дней с 01.01.1970.

> ``UNIX_DATE(date_expression)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    event_date,
--     UNIX_DATE() Example #1
    UNIX_DATE( today ) AS one_utc_today,
--     UNIX_DATE() Example #2
    UNIX_DATE( UTC_Time ) AS one_utc_UTC_Time,
--     UNIX_DATE() Example #3
    UNIX_DATE( event_date ) AS one_utc_event_date,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     DATE(CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME)) As UTC_Time,
     PARSE_DATE('%Y%m%d', event_date) as event_date
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

INT64


### Функции для типа Datetime:

* CURRENT_DATETIME
* DATETIME
* DATETIME_ADD
* DATETIME_SUB
* DATETIME_DIFF
* DATETIME_TRUNC
* FORMAT_DATETIME
* LAST_DAY
* PARSE_DATETIME

#### CURRENT_DATETIME - Получаем текущую дату и время

> ``CURRENT_DATE([time_zone])``

{% highlight sql %}
select current_datetime; -- datetime
{% endhighlight %}

##### Return Data Type

DATE

#### DATETIME - Создает объект DATETIME, используя значения INT64 год, месяц, день, час, минуту и ​​секунду.

> ``1. DATETIME(year, month, day, hour, minute, second)`` 
> ``2. DATETIME(date_expression[, time_expression])`` 
> ``3. DATETIME(timestamp_expression [, timezone])`` 

{% highlight sql %}
SELECT DATETIME(2020, 1, 1, 5, 30, 00) as DATETIME
{% endhighlight %}

##### Return Data Type

DATETIME

#### EXTRACT (part FROM date) - Возвращает день, месяц, год, часы, минуты, секунды.

Принимает следующие значения — MICROSECOND, MILLISECOND, SECOND, MINUTE, HOUR,
DAYOFWEEK, DAYOFYEAR, YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

{% highlight sql %}
SELECT
  EXTRACT(MICROSECOND FROM UTC_Time ) AS the_microsecond,
  EXTRACT(MILLISECOND FROM UTC_Time ) AS the_millisecond,
  EXTRACT(SECOND FROM UTC_Time ) AS the_second,
  EXTRACT(MINUTE FROM UTC_Time ) AS the_minute,
  EXTRACT(HOUR FROM UTC_Time ) AS the_hour,
  EXTRACT(DAY FROM UTC_Time ) AS the_day,
  EXTRACT(MONTH FROM UTC_Time ) AS the_month,
  EXTRACT(YEAR FROM UTC_Time ) AS the_year
FROM (
  SELECT
    CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
  {% endhighlight %}

##### Return Data Type

INT64, за исключением следующих случаев:

* Если часть - ДАТА, возвращает объект ДАТА. 
* Если часть - ВРЕМЯ, возвращает объект ВРЕМЯ.

#### DATETIME_ADD - добавляет указанный интервал времени к дате или дате и времени. 

Принимает следующие значения — MICROSECOND, MILLISECOND, SECOND, MINUTE, HOUR, YEAR, QUARTER, MONTH, WEEK, DAY

> ``DATETIME_ADD(datetime_expression, INTERVAL int64_expression part)``

{% highlight sql %}
SELECT
    UTC_Time,
    DATETIME_ADD( UTC_Time, INTERVAL 1 MICROSECOND ) AS plus_one_microsecond,
    DATETIME_ADD( UTC_Time, INTERVAL 1 MILLISECOND ) AS plus_one_millisecond,
    DATETIME_ADD( UTC_Time, INTERVAL 1 SECOND ) AS plus_one_second,
    DATETIME_ADD( UTC_Time, INTERVAL 1 MINUTE ) AS plus_one_minute,
    DATETIME_ADD( UTC_Time, INTERVAL 1 HOUR ) AS plus_one_hour,
    DATETIME_ADD( UTC_Time, INTERVAL 1 DAY ) AS plus_one_day,
    DATETIME_ADD( UTC_Time, INTERVAL 1 WEEK ) AS plus_one_week,
    DATETIME_ADD( UTC_Time, INTERVAL 1 MONTH ) AS plus_one_month,
    DATETIME_ADD( UTC_Time, INTERVAL 1 QUARTER ) AS plus_one_quarter,
    DATETIME_ADD( UTC_Time, INTERVAL 1 YEAR ) AS plus_one_year,
FROM (
  SELECT
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type 

DATETIME

#### DATETIME_SUB - Вычитает указанный временной интервал из ДАТЫ.

Принимает следующие значения — MICROSECOND, MILLISECOND, SECOND, MINUTE, HOUR, YEAR, QUARTER, MONTH, WEEK, DAY

> ``DATETIME_SUB(datetime_expression, INTERVAL int64_expression part)``

{% highlight sql %}
SELECT
    UTC_Time,
    DATETIME_SUB( UTC_Time, INTERVAL 1 MICROSECOND ) AS minus_one_microsecond,
    DATETIME_SUB( UTC_Time, INTERVAL 1 MILLISECOND ) AS minus_one_millisecond,
    DATETIME_SUB( UTC_Time, INTERVAL 1 SECOND ) AS minus_one_second,
    DATETIME_SUB( UTC_Time, INTERVAL 1 MINUTE ) AS minus_one_minute,
    DATETIME_SUB( UTC_Time, INTERVAL 1 HOUR ) AS minus_one_hour,
    DATETIME_SUB( UTC_Time, INTERVAL 1 DAY ) AS minus_one_day,
    DATETIME_SUB( UTC_Time, INTERVAL 1 WEEK ) AS minus_one_week,
    DATETIME_SUB( UTC_Time, INTERVAL 1 MONTH ) AS minus_one_month,
    DATETIME_SUB( UTC_Time, INTERVAL 1 QUARTER ) AS minus_one_quarter,
    DATETIME_SUB( UTC_Time, INTERVAL 1 YEAR ) AS minus_one_year,
FROM (
  SELECT
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATETIME

#### DATETIME_DIFF - Возвращает разницу между двумя датами timestamp1 и timestamp2.

Принимает следующие значения — MICROSECOND, MILLISECOND, SECOND, MINUTE, HOUR, YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``DATETIME_DIFF(datetime_expression_a, datetime_expression_b, part)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    DATETIME_DIFF( today, UTC_Time, MICROSECOND) AS difference_between_microsecond,
    DATETIME_DIFF( today, UTC_Time, MILLISECOND ) AS difference_between_millisecond,
    DATETIME_DIFF( today, UTC_Time, SECOND ) AS difference_between_second,
    DATETIME_DIFF( today, UTC_Time, MINUTE ) AS difference_between_minute,
    DATETIME_DIFF( today, UTC_Time, HOUR ) AS difference_between_hour,
    DATETIME_DIFF( today, UTC_Time, DAY) AS difference_between_day,
    DATETIME_DIFF( today, UTC_Time, WEEK ) AS difference_between_week,
    DATETIME_DIFF( today, UTC_Time, MONTH ) AS difference_between_month,
    DATETIME_DIFF( today, UTC_Time, QUARTER ) AS difference_between_quarter,
    DATETIME_DIFF( today, UTC_Time, YEAR ) AS difference_between_year,
    DATETIME_DIFF( today, UTC_Time, WEEK(SUNDAY)) AS difference_between_week_sunday,
    DATETIME_DIFF( today, UTC_Time, WEEK(MONDAY)) AS difference_between_week_monday,
    DATETIME_DIFF( today, UTC_Time, WEEK(TUESDAY)) AS difference_between_week_tuesday,
    DATETIME_DIFF( today, UTC_Time, WEEK(WEDNESDAY)) AS difference_between_week_wednesday,
    DATETIME_DIFF( today, UTC_Time, WEEK(THURSDAY)) AS difference_between_week_thusday,
    DATETIME_DIFF( today, UTC_Time, WEEK(FRIDAY)) AS difference_between_week_friday,
    DATETIME_DIFF( today, UTC_Time, WEEK(SATURDAY)) AS difference_between_week_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

INT64

#### DATETIME_TRUNC - Усекает дату до указанной степени детализации.

Принимает следующие значения — MICROSECOND, MILLISECOND, SECOND, MINUTE, HOUR, YEAR, QUARTER, MONTH, WEEK, DAY
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``DATETIME_TRUNC(date_expression, date_part)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    DATETIME_TRUNC( UTC_Time, MICROSECOND) AS trunc_microsecond,
    DATETIME_TRUNC( UTC_Time, MILLISECOND ) AS trunc_millisecond,
    DATETIME_TRUNC( UTC_Time, SECOND ) AS trunc_second,
    DATETIME_TRUNC( UTC_Time, MINUTE ) AS trunc_minute,
    DATETIME_TRUNC( UTC_Time, HOUR ) AS trunc_hour,
    DATETIME_TRUNC( UTC_Time, DAY) AS trunc_day,
    DATETIME_TRUNC( UTC_Time, WEEK ) AS trunc_week,
    DATETIME_TRUNC( UTC_Time, MONTH ) AS trunc_month,
    DATETIME_TRUNC( UTC_Time, QUARTER ) AS trunc_quarter,
    DATETIME_TRUNC( UTC_Time, YEAR ) AS trunc_year,
    DATETIME_TRUNC( UTC_Time, WEEK(SUNDAY)) AS trunc_sunday,
    DATETIME_TRUNC( UTC_Time, WEEK(MONDAY)) AS trunc_monday,
    DATETIME_TRUNC( UTC_Time, WEEK(TUESDAY)) AS trunc_tuesday,
    DATETIME_TRUNC( UTC_Time, WEEK(WEDNESDAY)) AS trunc_wednesday,
    DATETIME_TRUNC( UTC_Time, WEEK(THURSDAY)) AS trunc_thusday,
    DATETIME_TRUNC( UTC_Time, WEEK(FRIDAY)) AS trunc_friday,
    DATETIME_TRUNC( UTC_Time, WEEK(SATURDAY)) AS trunc_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATETIME

#### FORMAT_DATETIME - функция конвертирует дату в форматированный текст.

[Поддерживаемые элементы в format_string](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#supported_format_elements_for_date)

> ``FORMAT_DATETIME(format_string, date_expr)``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
--     FORMAT_DATETIME() Example #1
    FORMAT_DATETIME('%B', UTC_Time) AS datetime_to_month_name,
--     FORMAT_DATETIME() Example #2
    FORMAT_DATETIME('%r', UTC_Time) AS datetime_to_am_pm,
--     FORMAT_DATETIME() Example #3
    FORMAT_DATETIME('%m', UTC_Time) AS datetime_to_month,
--     FORMAT_DATETIME() Example #4
    FORMAT_DATETIME('%M', UTC_Time) AS datetime_to_minut,
--     FORMAT_DATETIME() Example #4
    FORMAT_DATETIME('%H', UTC_Time) AS datetime_to_hour,
--     FORMAT_DATETIME() Example #4
    FORMAT_DATETIME('%S', UTC_Time) AS datetime_to_second,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

STRING

#### LAST_DAY - Возвращает последний день из даты.

Принимает следующие значения — YEAR, QUARTER, MONTH, WEEK
WEEK(<WEEKDAY>): SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY

> ``LAST_DAY(datetime_expression[, date_part])``

{% highlight sql %}
SELECT
    today,
    UTC_Time,
    LAST_DAY( UTC_Time, WEEK ) AS last_day_week,
    LAST_DAY( UTC_Time, MONTH ) AS last_day_month,
    LAST_DAY( UTC_Time, QUARTER ) AS last_day_quarter,
    LAST_DAY( UTC_Time, YEAR ) AS last_day_year,
    LAST_DAY( UTC_Time, WEEK(SUNDAY)) AS last_day_sunday,
    LAST_DAY( UTC_Time, WEEK(MONDAY)) AS last_day_monday,
    LAST_DAY( UTC_Time, WEEK(TUESDAY)) AS last_day_tuesday,
    LAST_DAY( UTC_Time, WEEK(WEDNESDAY)) AS last_day_wednesday,
    LAST_DAY( UTC_Time, WEEK(THURSDAY)) AS last_day_thusday,
    LAST_DAY( UTC_Time, WEEK(FRIDAY)) AS last_day_friday,
    LAST_DAY( UTC_Time, WEEK(SATURDAY)) AS last_day_saturday,
FROM (
  SELECT
     CURRENT_DATE() AS today,
     CAST(TIMESTAMP_MICROS(event_timestamp) AS DATETIME) As UTC_Time,
  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type

DATE

#### PARSE_DATETIME - Преобразует строковое представление даты в объект DATE.
[Поддерживаемые элементы в format_string](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#supported_format_elements_for_date)

> ``PARSE_DATE(format_string, date_string)``

{% highlight sql %}
SELECT
    today,
--     PARSE_DATETIME() Example #1
    PARSE_DATETIME('%Y-%m-%d', today) AS parse_date_today,
    event_date,
--     PARSE_DATETIME() Example #2
    PARSE_DATETIME('%Y%m%d', event_date) AS parse_date_event_date,
--     PARSE_DATETIME() Example #3    
    UTC_Time,
    PARSE_DATETIME('%Y-%m-%d %H:%M:%S', UTC_Time) AS parse_date_UTC_Time,
FROM (
  SELECT
     STRING(CURRENT_DATE()) AS today,
     event_date,
     FORMAT_DATETIME('%Y-%m-%d %H:%M:%S', DATETIME(TIMESTAMP_MICROS(event_timestamp))) As UTC_Time,

  FROM
    `firebase-public-project.analytics_153293282.events_20181003`)
LIMIT 1
{% endhighlight %}

##### Return Data Type 

DATETIME




































### Функции для типа Time:

* CURRENT_TIME
* TIME
* TIME_ADD
* TIME_SUB
* TIME_DIFF
* TIME_TRUNC
* FORMAT_TIME
* PARSE_TIME

### Функции для типа Timestamp

* CURRENT_TIMESTAMP
* EXTRACT
* STRING
* TIMESTAMP
* TIMESTAMP_ADD
* TIMESTAMP_SUB
* TIMESTAMP_DIFF
* TIMESTAMP_TRUNC
* FORMAT_TIMESTAMP
* PARSE_TIMESTAMP
* TIMESTAMP_SECONDS
* TIMESTAMP_MILLIS
* TIMESTAMP_MICROS
* UNIX_SECONDS
* UNIX_MILLIS
* UNIX_MICROS










{% highlight sql %}
select current_date;
{% endhighlight %}

































