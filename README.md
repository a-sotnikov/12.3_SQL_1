# Домашнее задание к занятию "SQL - часть 1" - `Андрей Сотников`

---

### Задание 1

> Получите уникальные названия районов из таблицы с адресами, которые
начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

``` sql
SELECT DISTINCT district FROM address 
WHERE district like 'K%a' AND NOT district LIKE '% %'
```

### Задание 2

> Получите из таблицы платежей за прокат фильмов информацию по платежам, которые
выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно**
и стоимость которых превышает 10.00.

``` sql
SELECT * FROM payment
WHERE (CAST(payment_date AS DATE) BETWEEN '2005-06-15' AND '2005-06-18') AND amount > 10
```

### Задание 3

> Получите последние пять аренд фильмов.

```sql
SELECT * FROM rental 
ORDER BY rental_date DESC LIMIT 5
```

### Задание 4

> Одним запросом получите активных покупателей, имена которых Kelly или Willie.
>
> Сформируйте вывод в результат таким образом:
>
> - все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
> - замените буквы 'll' в именах на 'pp'.

```sql
SELECT REPLACE(LOWER(first_name), 'll', 'pp'), LOWER(last_name) 
FROM customer 
WHERE active=1 AND first_name LIKE 'Kelly' OR first_name LIKE 'Willie'
```

### Задание 5*

> Выведите Email каждого покупателя, разделив значение Email на две отдельных
колонки: в первой колонке должно быть значение, указанное до @,
во второй — значение, указанное после @.

```sql
SELECT email, 
LEFT(email, POSITION('@' IN email)-1) AS first_part,
SUBSTR(email, POSITION('@' IN email)+1) AS second_part
FROM customer
```

### Задание 6*

> Доработайте запрос из предыдущего задания, скорректируйте значения в
новых колонках: первая буква должна быть заглавной, остальные — строчными.

```sql
SELECT email, 
INSERT(LOWER(LEFT(email, POSITION('@' IN email)-1)), 1, 1, UPPER(LEFT(email, 1))) AS first_part,
INSERT(LOWER(SUBSTR(email, POSITION('@' IN email)+1)), 1, 1, UPPER(SUBSTR(email, POSITION('@' IN email)+1, 1))) AS second_part
FROM customer
```
