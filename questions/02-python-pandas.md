# Python і pandas / Python and pandas

## Базові питання

**1. 🟢 list / tuple / set / dict.** list — змінюваний, tuple — незмінний, set — унікальні елементи, dict — пари ключ→значення (пошук O(1) у середньому).

**2. 🟢 Зрізи й comprehension.**
```python
squares = [x**2 for x in range(10) if x % 2 == 0]
by_len = {w: len(w) for w in ["sql", "python"]}
```

**3. 🟡 Mutable default argument.** `def f(x, acc=[])` зберігає список між викликами. Правильно: `acc=None` і створювати всередині.

**4. 🟡 `is` vs `==`.** `==` порівнює значення, `is` — ідентичність об'єктів.

**5. 🟡 Генератори.** `yield` повертає елементи ліниво, економить пам'ять на великих файлах.

## pandas

**6. 🟢 Читання та огляд.**
```python
import pandas as pd
df = pd.read_csv("data.csv", parse_dates=["date"])
df.info(); df.describe(include="all"); df.isna().mean()
```

**7. 🟡 groupby + agg.**
```python
out = (df.groupby("segment")
         .agg(n=("id","count"), spend=("amount","sum"), avg=("amount","mean"))
         .reset_index())
```

**8. 🟡 merge і перевірка розмноження рядків.**
```python
m = a.merge(b, on="id", how="left", validate="many_to_one")
assert len(m) == len(a)
```
Параметр `validate` одразу ловить дублікати ключа.

**9. 🟡 Пропуски.** `df.fillna({"income": df.income.median()})`, `df.dropna(subset=[...])`; фіксуйте причини пропусків.

**10. 🟡 Віконні операції.**
```python
df = df.sort_values(["card_id","month"])
df["prev_balance"] = df.groupby("card_id")["balance"].shift(1)
df["ma3"] = df.groupby("card_id")["balance"].transform(lambda s: s.rolling(3).mean())
```

**11. 🔴 Продуктивність.** Векторизація замість `apply`/циклів, типи `category`, `float32`, читання по частинах (`chunksize`), Parquet замість CSV, `pyarrow`/`polars` на великих обсягах.

## Задачі

**A. Когортний retention.**
```python
df["cohort"] = df.groupby("user_id")["date"].transform("min").dt.to_period("M")
df["period"] = df["date"].dt.to_period("M")
df["age"] = (df["period"] - df["cohort"]).apply(lambda x: x.n)
ret = (df.groupby(["cohort","age"])["user_id"].nunique()
         .unstack(fill_value=0))
ret = ret.div(ret[0], axis=0)
```

**B. Виявлення викидів за IQR.**
```python
q1, q3 = df.amount.quantile([.25, .75]); iqr = q3 - q1
outliers = df[(df.amount < q1 - 1.5*iqr) | (df.amount > q3 + 1.5*iqr)]
```

**C. Тест двох пропорцій (A/B).**
```python
from statsmodels.stats.proportion import proportions_ztest
stat, p = proportions_ztest([conv_a, conv_b], [n_a, n_b])
```

## Чекліст чистого аналізу

1. Фіксуйте seed і версію даних.
2. Додавайте assert-перевірки (розмір, унікальність, діапазони).
3. Пояснюйте кожен крок коментарем «чому», а не «що».
4. Зберігайте результат у відтворюваному вигляді (ноутбук + `requirements.txt`).
