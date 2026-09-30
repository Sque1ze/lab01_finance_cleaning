# Лабораторна робота №1 — Варіант 005 (Фінанси)

Готовий Python-проєкт для VS Code. Основний файл: `notebooks/lab01_cleaning.ipynb`.

## Структура

```text
lab01_finance_cleaning/
├─ data/
│  ├─ raw/generated_dirty.csv
│  └─ clean/dataset_clean.csv
├─ notebooks/lab01_cleaning.ipynb
├─ outputs/
│  ├─ figures/
│  │  ├─ 01_fx_deviation_before_after.png
│  │  └─ 02_channel_share_before_after.png
│  ├─ kpi_summary.csv
│  └─ validation_results.csv
├─ change_log.csv
├─ requirements.txt
└─ README.md
```

## Запуск у VS Code на Windows

1. Розпакуй ZIP і відкрий папку `lab01_finance_cleaning` через **File → Open Folder**.
2. Переконайся, що встановлено Python 3.10+ та розширення VS Code **Python** і **Jupyter**.
3. Відкрий Terminal → New Terminal та створи віртуальне середовище:

```powershell
python -m venv .venv
```

4. Активуй його:

```powershell
.\.venv\Scripts\Activate.ps1
```

Якщо PowerShell блокує активацію, лише для поточного вікна виконай:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

5. Встанови залежності:

```powershell
python -m pip install --upgrade pip
pip install -r requirements.txt
```

6. Відкрий `notebooks/lab01_cleaning.ipynb`.
7. У правому верхньому куті натисни **Select Kernel** → **Python Environments** → вибери `.venv`.
8. Натисни **Run All**.

Після запуску ноутбук заново генерує `data/raw/generated_dirty.csv`, очищує дані, зберігає `data/clean/dataset_clean.csv`, `change_log.csv`, результати валідації, KPI та два PNG-графіки.

## Що показати на захисті

- першу клітинку з `RANDOM_SEED = 2026005`;
- контроль параметрів генерації і статистику «до»;
- таблицю `cleaning_plan`;
- `change_log.csv`;
- таблицю з 14 автоматичними валідаціями;
- 2 графіки «до/після»;
- `kpi_table` з 10 KPI;
- порівняння 3 KPI «до/після».

## Важливі рішення для пояснення

- `DD.MM.YYYY HH:MM` у синтетичному наборі трактується як `Europe/Kyiv`, потім переводиться в UTC.
- Для UAH курс дорівнює 1.
- Якщо `amount_uah` пропущено, його відновлюємо як `amount * fx_rate`.
- Якщо foreign `fx_rate` пропущено, спочатку пробуємо `abs(amount_uah / amount)`, інакше — медіану по валюті.
- Якщо FX-невідповідність >3%, довіряємо `amount` і `fx_rate`, а `amount_uah` перераховуємо.
- Невалідний MCC не вигадуємо: переводимо в `NA`.
- `city` не імпутуємо без зовнішнього джерела.
- Повні дублікати видаляються першими; near-duplicate визначається за повторним `txn_id` після нормалізації тексту.
