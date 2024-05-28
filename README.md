# Домашнее задание к занятию "`Индексы`" - `Степанов Владислав`

### Задание 1

```sql
SELECT 
table_schema,
#sum(data_length),
#sum(index_length), 
concat(round(sum(data_length) / (((sum(data_length) + sum(index_length))) / 100)),'%') as 'proc_data', 
concat(round(sum(index_length) / (((sum(data_length) + sum(index_length))) / 100)),'%') as proc_index
from INFORMATION_SCHEMA.tables
group by table_schema
having table_schema = 'sakila'
```

### Задание 2  

1.![Image alt](https://github.com/vladislst/12-05.md/raw/main/img/1.jpg)

Узкое место в агрегации оконной функции

2.![Image alt](https://github.com/vladislst/12-05.md/raw/main/img/2.jpg)

оптимизирован запрос и добавлен индекс:
```sql
create index payment_date_index on payment(payment_date);
select distinct concat(c.last_name, ' ', c.first_name), sum(p.amount) over (partition by c.customer_id)
from payment p 
join rental r on r.rental_id = p.rental_id 
join customer c ON c.customer_id = p.customer_id 
join inventory i on i.inventory_id = r.inventory_id 
where date(p.payment_date) = '2005-07-30'
```

убрана неиспользуемая таблица film