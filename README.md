# Домашнее задание к занятию 12.5 «Индексы» -  Баранов Сергей

---


### Задание 1

### Напишите запрос к учебной базе данных, который вернёт процентное отношение общего размера всех индексов к общему размеру всех таблиц.

![monitoring](https://github.com/12sergey12/12.5_Indexes/blob/main/images/12.5-1.png)

---


### Задание 2

### Выполните explain analyze следующего запроса:
```sql
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id, f.title)
from payment p, rental r, customer c, inventory i, film f
where date(p.payment_date) = '2005-07-30' and p.payment_date = r.rental_date and r.customer_id = c.customer_id and i.inventory_id = r.inventory_id
```

- перечислите узкие места;
- оптимизируйте запрос: внесите корректировки по использованию операторов, при необходимости добавьте индексы.


![monitoring](https://github.com/12sergey12/12.5_Indexes/blob/main/images/12.5-21.png)

---

- distinct может отключать индексы, убираем его из запроса
- группируем по customer_id, также убираем OVER с partitions
- Убираем из оператора Over f.title и запрос данных из таблицы film f, т.к. данные запроса в селекте не используют заголовки фильмов.
- убираем обращение к таблице inventory и обращение inventory_id т.к. они тоже не учавствуют в фильтрации данных.


![monitoring](https://github.com/12sergey12/12.5_Indexes/blob/main/images/12.5-24.png)
