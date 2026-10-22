# SQL: питання та задачі / SQL questions

Позначки: 🟢 Junior · 🟡 Middle · 🔴 Senior. Практикуйте в [SQL-тренажері](https://antonemec.github.io/analyst-html-tools/sql-trainer.html).

## Теорія

**1. 🟢 WHERE vs HAVING.**
WHERE фільтрує рядки до групування, HAVING — групи після агрегації.

**2. 🟢 Види JOIN.**
INNER — лише збіги; LEFT — усе ліве + збіги; RIGHT — навпаки; FULL — усе з обох; CROSS — декартів добуток. SELF JOIN — таблиця сама з собою (ієрархії, «попередній запис»).

**3. 🟢 COUNT(\*) / COUNT(col) / COUNT(DISTINCT col).**
Усі рядки / не-NULL / унікальні не-NULL.

**4. 🟢 NULL.**
`NULL = NULL` дає невідомо (не TRUE). Використовуйте `IS NULL`, `COALESCE(col, 0)`. Агрегати ігнорують NULL (крім COUNT(\*)).

**5. 🟡 UNION vs UNION ALL.**
UNION видаляє дублікати (сортування/хеш → повільніше), UNION ALL — ні.

**6. 🟡 Підзапит vs CTE vs тимчасова таблиця.**
CTE — читабельність, одноразове використання в запиті; temp table — матеріалізація, індекси, повторне використання; підзапит — короткі випадки. В залежності від СУБД CTE може оптимізуватись по-різному.

**7. 🟡 Віконні функції: ROW_NUMBER, RANK, DENSE_RANK, LAG/LEAD, SUM OVER.**
Дивіться тренажер, задачі 13–18. Рамка: `ROWS BETWEEN 2 PRECEDING AND CURRENT ROW`.

**8. 🟡 Порядок виконання запиту.**
FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT.

**9. 🔴 Індекси: коли допомагають, коли шкодять.**
Допомагають у фільтрах/JOIN/сортуванні за вибірковими колонками. Шкодять при частих записах, низькій селективності (стать), функціях над колонкою у WHERE (`WHERE DATE(col)=...`).

**10. 🔴 Нормалізація та денормалізація.**
Нормалізація зменшує дублювання, денормалізація (зірка/сніжинка) прискорює аналітичні запити. Факти й виміри у вітринах.

## Практичні задачі

Схема: `customers`, `cards`, `transactions`, `monthly_snapshots` (як у тренажері).

**A. 🟢 Топ-5 клієнтів за обігом.**
```sql
SELECT cu.customer_id, ROUND(SUM(t.amount),2) AS spend
FROM transactions t
JOIN cards c ON c.card_id=t.card_id
JOIN customers cu ON cu.customer_id=c.customer_id
WHERE t.status='approved'
GROUP BY cu.customer_id ORDER BY spend DESC LIMIT 5;
```

**B. 🟡 Клієнти, які не мали жодної транзакції за останній місяць.**
```sql
SELECT cu.customer_id
FROM customers cu
WHERE NOT EXISTS (
  SELECT 1 FROM cards c JOIN transactions t ON t.card_id=c.card_id
  WHERE c.customer_id=cu.customer_id AND t.txn_date >= '2026-09-01');
```

**C. 🟡 Медіана суми транзакції (без MEDIAN()).**
```sql
WITH o AS (SELECT amount, ROW_NUMBER() OVER (ORDER BY amount) rn, COUNT(*) OVER () n FROM transactions WHERE status='approved')
SELECT AVG(amount) FROM o WHERE rn IN ((n+1)/2, (n+2)/2);
```

**D. 🔴 Друга за величиною транзакція кожного клієнта.**
```sql
WITH r AS (
  SELECT c.customer_id, t.amount,
         DENSE_RANK() OVER (PARTITION BY c.customer_id ORDER BY t.amount DESC) AS rk
  FROM transactions t JOIN cards c ON c.card_id=t.card_id WHERE t.status='approved')
SELECT customer_id, amount FROM r WHERE rk=2 GROUP BY customer_id, amount;
```

**E. 🔴 Сесії/ланцюги: послідовні місяці прострочки** — див. тренажер, задача 17.

## Як відповідати на SQL-завдання вголос

1. Повторіть умову й уточніть: що таке «клієнт»? що робити з NULL? які межі періоду?
2. Назвіть таблиці, ключі й грануляцію результату («один рядок на що?»).
3. Напишіть простий варіант, потім ускладніть.
4. Перевірте граничні випадки: порожній результат, дублікати, NULL.
5. Згадайте продуктивність: індекси, розмір таблиць.
