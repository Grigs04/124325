# Задания по дисциплине «Программирование и ИТ в научно-исследовательской работе»

**Тема ВКР:** Разработка модуля CRM-системы для автоматизации финансового и управленческого учёта предприятия сферы услуг

---

## Структура репозитория

```
├── README.md
├── data/
│   ├── clients_clustering.csv        ← датасет для кластеризации (300 клиентов)
│   └── orders_classification.csv    ← датасет для классификации (500 заказов)
├── notebooks/
    ├── clustering_mafia_time.ipynb
    └── classification_mafia_time.ipynb

```

---

## Задание 1. Аннотированный список источников (5+)

### Источник 1

**Das, S., Nayak, J.** Customer Segmentation via Data Mining Techniques: State-of-the-Art Review // *Computational Intelligence in Data Mining. Smart Innovation, Systems and Technologies*, vol. 281. — Singapore: Springer, 2022. — P. 489–507.  
DOI: [10.1007/978-981-16-9447-9_38](https://doi.org/10.1007/978-981-16-9447-9_38)

**Аннотация.** Систематический обзор 57 статей: K-Means, RFM и иерархическая кластеризация наиболее распространены для сегментации клиентов в CRM-системах. Служит теоретической основой для аналитической подсистемы и практических заданий по кластеризации клиентской базы «Mafia Time».

---

### Источник 2

**Mahadik, D., Shinde, A., Konale, A., Kadam, D., Auti, O.** To Study Webhooks for an Event-Driven Integration Mechanism // *ALOCHANA Journal*. — 2025. — Vol. 14, Issue 11.  
URL: [https://alochana.org/wp-content/uploads/6-AJ3851.pdf](https://alochana.org/wp-content/uploads/6-AJ3851.pdf)

**Аннотация.** Сравнение вебхуков с polling-подходами; анализ валидации payload и надёжности доставки событий. Непосредственно применимо к модулю обработки вебхуков amoCRM в дипломной работе.

---

### Источник 3

**WPI Team.** Webhooks-as-a-Service: A Custom API Design // *Worcester Polytechnic Institute Digital WPI*. — 2022.  
URL: [https://digital.wpi.edu/downloads/2n49t521z](https://digital.wpi.edu/downloads/2n49t521z)

**Аннотация.** Архитектура Webhook Dispatch System: Router Lambda (валидация payload) + Child Lambda (маршрутизация), хранилище на AWS DynamoDB. Решения учтены при проектировании backend-модуля обработки событий amoCRM.

---

### Источник 4

**Rahmatulloh, A., Nugraha, F., Gunawan, R., Darmawan, I.** Event-Driven Architecture to Improve Performance and Scalability in Microservices-Based Systems // *ICADEIS*. — IEEE, 2022. — P. 01–06.  
URL: [https://ieeexplore.ieee.org/document/9848872](https://ieeexplore.ieee.org/document/9848872)

**Аннотация.** Экспериментальное сравнение синхронных REST и асинхронной EDA при реальных нагрузках. Результаты подтверждают преимущество событийной модели по времени отклика — обосновывает выбор вебхуков для архитектуры модуля «Mafia Time».

---

### Источник 5

**Ledro, C., Nosella, A., Vinelli, A.** Artificial intelligence in customer relationship management: literature review and future research directions // *Journal of Business & Industrial Marketing*. — 2022. — Vol. 37, No. 13. — P. 48–63.  
DOI: [10.1108/JBIM-07-2021-0332](https://doi.org/10.1108/JBIM-07-2021-0332)

**Аннотация.** Библиометрическое исследование на базе 212 рецензируемых статей за 1989–2020 гг. Авторы систематизируют литературу о взаимосвязи ИИ и CRM, выделяя три направления: большие данные как база CRM, машинное обучение и предиктивная аналитика, автоматизация коммуникаций. Предложена трёхступенчатая концептуальная модель внедрения ИИ в CRM. Актуальна как теоретическая основа для обоснования автоматизации CRM-процессов применительно к малому бизнесу.

---

## Задание 2. Кластеризация в Orange Data Mining

### Данные

Файл: **`data/clients_clustering.csv`** (300 строк)

| Столбец | Описание |
|---|---|
| `orders_count` | Количество заказов клиента |
| `avg_check` | Средний чек (руб.) |
| `days_since_last_order` | Дней с последнего заказа |
| `response_time_hours` | Среднее время ответа менеджера (ч.) |
| `total_revenue` | Суммарная выручка по клиенту (руб.) |
| `segment` | Истинный сегмент — **не используется** в кластеризации, пометить как meta |


### Скриншоты

**Скрин 1 — Общая схема на холсте**

![Схема кластеризации в Orange](screenshots/orange_clustering_schema.png)

*Рисунок 1 — Схема потока данных для кластеризации в Orange Data Mining*

---

**Скрин 2 — Scatter Plot: K-Means**

![K-Means](screenshots/orange_clustering_kmeans.png)

*Рисунок 2 — Визуализация кластеров K-Means (k=3)*

---

**Скрин 3 — Scatter Plot: DBSCAN**

![DBSCAN](screenshots/orange_clustering_dbscan.png)

*Рисунок 3 — Кластеры DBSCAN (eps=1.5, min\_samples=8), серые точки — шум*

---

**Скрин 4 — Дендрограмма Hierarchical Clustering**

![Дендрограмма](screenshots/orange_clustering_hier.png)

*Рисунок 4 — Дендрограмма (Ward, 3 кластера)*

---

### Ожидаемые результаты

| Метод | Кластеров | Силуэт | Особенность |
|---|---|---|---|
| K-Means | 3 | ~0.45–0.55 | VIP / корпоративные / частные клиенты |
| DBSCAN | 2–3 + шум | ~0.40–0.50 | ~7% аномальных клиентов помечаются как шум |
| Hierarchical | 3 | ~0.43–0.53 | Иерархия сегментов видна на дендрограмме |

---

## Задание 3. Классификация в Orange Data Mining

### Данные

Файл: **`data/orders_classification.csv`** (500 строк)

| Столбец | Описание |
|---|---|
| `order_amount` | Сумма заказа (руб.) |
| `days_to_event` | Дней до мероприятия на момент выставления счёта |
| `client_orders_history` | Количество предыдущих заказов клиента |
| `response_time_hours` | Время ответа клиента на счёт (часов) |
| `is_corporate` | Корпоративный клиент (0/1) |
| `has_contract` | Подписан договор (0/1) |
| `paid` | **Целевая переменная**: оплачен заказ (1=да, 0=нет) |

---

### Скриншоты

**Скрин 5 — Общая схема классификации на холсте**

![Схема классификации](screenshots/orange_classif_schema.png)

*Рисунок 5 — Схема потока данных для классификации в Orange Data Mining*

---

**Скрин 6 — Test and Score (сводные метрики)**

![Test and Score](screenshots/orange_classif_scores.png)

*Рисунок 6 — Сравнение Accuracy, F1, AUC-ROC трёх классификаторов*

---

**Скрин 7 — ROC Analysis**

![ROC кривые](screenshots/orange_classif_roc.png)

*Рисунок 7 — ROC-кривые Decision Tree, Random Forest, kNN*

---

**Скрин 8 — Confusion Matrix (Random Forest)**

![Confusion Matrix](screenshots/orange_classif_matrix.png)

*Рисунок 8 — Матрица ошибок Random Forest*

---

### Ожидаемые метрики

| Метод | Accuracy | F1 | AUC-ROC |
|---|---|---|---|
| Decision Tree | ~0.78 | ~0.80 | ~0.84 |
| Random Forest | ~0.83 | ~0.85 | ~0.90 |
| kNN | ~0.76 | ~0.78 | ~0.83 |

---

## Задания 4–5. JupyterLab — Python / scikit-learn

### Запуск


Открыть файлы из папки `notebooks/`.

### `clustering_mafia_time.ipynb`

K-Means · DBSCAN · Agglomerative Clustering на синтетическом датасете клиентов (300 записей).  
Включает: метод локтя, силуэт, k-distance graph, дендрограмму, сравнительный subplot.

### `classification_mafia_time.ipynb`

Decision Tree · Random Forest · KNN на датасете заказов (500 записей, целевая — `paid`).  
Включает: EDA, cross-val подбор k, важность признаков, ROC-кривые, матрицы ошибок.
