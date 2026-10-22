# Мапа вимог ринку України / Ukrainian job-market map

**UA:** Що реально просять у вакансіях аналітиків у банках і фінтеху України, і де в цьому репозиторії та в [SQL-тренажері](https://antonemec.github.io/analyst-html-tools/sql-trainer.html) це тренувати. Мапа складена за кількома оголошеннями та звітом DOU (жовтень 2026), це не статистично репрезентативна вибірка. Перед співбесідою звіряйте з актуальними вакансіями.
**EN:** What Ukrainian banking and fintech analyst job ads actually ask for, and where to practice it here. Built from a handful of ads and a DOU salary report (Oct 2026); not a statistically representative sample. Check against live vacancies before an interview.

## Вимоги з вакансій → де тренувати

| Вимога у вакансії / Requirement | Приклад джерела | Де тренувати |
|---|---|---|
| Просунутий SQL, складні запити (PostgreSQL / MS SQL / MySQL) | [Аналітик SQL, UKRSIBTECH](https://djinni.co/jobs/835900-analitik-sql/); [Head of Risk Analytics, E-Groshi](https://djinni.co/jobs/851220-head-of-risk-analytics/) | [questions/01-sql.md](questions/01-sql.md); тренажер, задачі 6–23 |
| Excel / Google Sheets (формули, зведені таблиці; у деяких оголошеннях VBA) | обидва оголошення вище | [resources.md](resources.md); додайте в портфоліо приклад звіту в Sheets |
| Метрики портфеля: DPD, default rate, roll rate, NPL, approval rate, cost of risk | E-Groshi | [questions/04-banking-domain.md](questions/04-banking-domain.md); тренажер: NPL 90+, approval rate, roll rate; [калькулятор](https://antonemec.github.io/analyst-html-tools/metrics-calculator.html) |
| Скорингова логіка, гіпотези, зміна критеріїв схвалення й лімітів | E-Groshi | [questions/03-metrics-product-ab.md](questions/03-metrics-product-ab.md) (A/B і гіпотези) |
| Сегментація цільових груп для маркетингових кампаній, звіти з ефективності | UKRSIBTECH | тренажер: «Цільова аудиторія для кампанії» |
| Управлінська звітність, дашборди (Power BI / Tableau / Metabase) | обидва оголошення | [resources.md](resources.md); тиждень 2 у [плані на 30 днів](study-plan-30-days.md) |
| Написання ТЗ для IT, перевірка впровадження правил | E-Groshi | [questions/06-behavioral.md](questions/06-behavioral.md) (питання 11–12 про ТЗ і перевірку правил) |
| Знання банківських продуктів і регуляторики НБУ | UKRSIBTECH | [questions/04-banking-domain.md](questions/04-banking-domain.md); [NBU-панель](https://antonemec.github.io/analyst-html-tools/nbu-market.html) |
| Якість даних перед аналізом | загальна практика | тренажер: «Якість даних: прогалини в знімках» |
| Портфельний аналіз і стрес-тестування | вакансії великих банків | тренажер: «Стрес-тест: ріст NPL» |
| Python для аналітики (плюс, не база) | E-Groshi | [questions/02-python-pandas.md](questions/02-python-pandas.md) |
| Англійська B1, українська C2 | UKRSIBTECH | розділ «Англійською» у [questions/06-behavioral.md](questions/06-behavioral.md) |

## Рівні → що підготувати

Орієнтовна кількість питань у файлах: SQL 18, Python 19, метрики й A/B 26, банківський домен 21, логіка й Фермі 11, behavioral 12 типових і 5 англійською.

| Рівень | Що перевіряють | Файли (питання з позначкою 🟢 / 🟡 / 🔴) | Тренажер |
|---|---|---|---|
| Junior | SQL-основи, уважність до даних, базова статистика | 01-sql 🟢, 03 🟢, 04 🟢 (DPD, NPL, MCC) | задачі 1–12 |
| Middle | віконні функції, метрики портфеля, A/B, самостійний аналіз | 01-sql 🟡, 03 🟡, 04 🟡 (roll rate, vintage, CoR) | задачі 13–23 |
| Senior / Lead | постановка задач, ТЗ, вплив на рішення, управління командою | 04 🔴, 06 (STAR, лідерство) | усі задачі + власний кейс |

## Орієнтир із зарплат / Salary reference

За [звітом DOU (літо 2026)](https://dou.ua/lenta/articles/salary-report-data-summer-2026/): медіана Senior Data Analyst — $3 550 net на місяць (+$350 за пів року), Senior Business Analyst — $3 400; третій квартиль для Team Lead Data Analyst — $4 725. Звіт не розбиває дані за рівнями junior/middle і за сферою (банки), тому ці цифри є верхнім орієнтиром для сильних кандидатів, а не гарантією. Оголошення банків можуть пропонувати інші діапазони.

## Обмеження

- Оголошення могли закритися, поки ви читаєте: це приклади вимог, а не відкриті позиції.
- Вимоги різняться між банком, фінтехом і мікрофінансовою компанією.
- Не копіюйте внутрішні матеріали роботодавця в публічне портфоліо.
