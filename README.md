TON Predictor Pro (High-Load Async Engine)
Высоконагруженный бэкенд Telegram Mini App (модель пари-мутуэль на TON). Проект спроектирован с упором на отказоустойчивость, финансовую точность и защиту от фрода.
> **Note:** В открытом доступе представлена архитектурная спецификация и структура модулей. Ядро расчетов и антифрод скрыты.
---
🛠 Стек
* **Язык:** Python 3.11+, aiogram 3.x
* **БД & Кэш:** PostgreSQL, SQLAlchemy 2.0 (async), asyncpg, Redis
* **Инфраструктура:** Alembic, Docker
---
📐 Архитектура и ключевые решения
1. **Точность расчетов:** Использование `Numeric(20, 8)` исключает ошибки округления с плавающей точкой.
2. **Идемпотентность:** Защита от повторных списаний и дублирования транзакций (`idem_key`).
3. **Изоляция балансов:** Разделение пользовательских средств и операционных счетов с фоновой сверкой через воркеры.
4. **Контур безопасности:** Модуль поведенческого анализа (`AnomalyDetector`) для отсечения аномальных ставок.
---
📂 Структура проекта
```text
├── bot/
│   ├── handlers/        # Контроллеры и FSM-состояния (aiogram 3)
│   ├── middlewares/     # Лимиты, защита и авторизация
│   └── database/        # SQLAlchemy 2.0 модели и пул asyncpg
├── services/            # Бизнес-логика, раунды, TON API, воркеры
├── security/            # Аварийные режимы и антифрод
├── alembic/             # Миграции базы данных
├── tests/               # PyTest и нагрузочные симуляции
└── main.py              # Точка входа
⚙️ Быстрый старт (dev)
git clone [https://github.com/rimskii15/ton-predictor-pro.git](https://github.com/rimskii15/ton-predictor-pro.git)
cd ton-predictor-pro
cp .env.example .env
pip install -r requirements.txt
alembic upgrade head
python main.py