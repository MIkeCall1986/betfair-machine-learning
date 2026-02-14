# Betfair
## Collect odds
To begin collecting odds you must run the `betfair.py` script using the following command:
```
python3 betfair.py appKey sessionToken &
```
### Create Application Key
To create an Application Key visit: <br/> http://docs.developer.betfair.com/docs/display/1smk3cen4v3lu3yomq5qye0ni/Application+Keys#ApplicationKeys-HowtoCreateAnApplicationKey <br/>
### Obtain Session Token 
* Login to betfair.com.
* Go to https://developer.betfair.com/exchange-api/accounts-api-demo/ and copy your session token.

## Clean data
To clean the data (remove noise and anomalous results), run the `cleanData.py` script using the following command:
```
python cleanData.py directory
``` 
where `directory` is the directory that contains the `.csv` files to be cleaned. This will create a new directory `CLEANED_FILES` within `directory` which will contain the cleaned `.csv` files.

## Plot match odds
To plot collected odds for a particular match run the `plotMatchOdds.py` script using the following command:
```
python plotMatchOdds.py example.csv
```
You can produce a more detailed plot which includes shaded regions for when the match is inplay, and dashed lines for goals using the following command:
```
python plotMatchOdds.py example.csv extraMinsFirstHalf extraMinsSecondHalf minOfGoal1 minOfGoal2 ...
```
14.02.26
Ось результати аналізу та стратегія трансформації для проекту **betfair-machine-learning**, підготовлені у форматі для Notion.

---

# 📑 Звіт AI-консультанта: Проект "betfair-machine-learning"

## 🧬 Частина 1: "ДНК" Проекту

Проект **betfair-machine-learning** являє собою комплексний інструментарій для повного циклу роботи з даними спортивного беттінгу: від збору сирих даних до побудови прогнозних моделей. Його логіку можна розбити на такі атомарні функції:

*   **Авторизація та збір (Data Acquisition):** Скрипт `betfair.py` забезпечує доступ до API Betfair за допомогою ключів додатку та токенів сесії.
*   **Парсинг контекстних даних (Scraping):** Модулі `matchEventsScraper.py` та `rankingsScraper.py` відповідають за збір додаткової інформації про події матчів та рейтинги команд.
*   **Очищення та препроцесинг (Data Cleaning):** Скрипт `cleanData.py` видаляє шуми та аномальні результати із зібраних CSV-файлів.
*   **Формування датасетів:** `generateDataset.py` об'єднує розрізнені дані у структуровані набори для навчання.
*   **Машинне навчання та регресія (Modeling):** Використання бібліотек **Scikit-learn** (`sklearnModel.py`) та **TensorFlow** (`tensorflowModel.py`) для створення моделей прогнозування.
*   **Візуалізація (Visualization):** Модуль `plotMatchOdds.py` дозволяє будувати графіки руху коефіцієнтів з прив'язкою до ігрових подій (голів, пауз).

### 💎 Головна технічна цінність
Головна цінність проекту — у створенні **наскрізного конвеєра даних (data pipeline)** для ринку футбольних ставок. Проект дозволяє не просто збирати цифри, а візуалізувати динаміку ринку в реальному часі та застосовувати глибоке навчання для пошуку закономірностей у рухах коефіцієнтів.

---

## 🚀 Частина 2: "Трансформація" (Інтеграція з Gemini LLM)

Інтеграція Gemini перетворює цей набір скриптів з "калькулятора імовірностей" на **інтелектуального спортивного аналітика**.

### Як зміниться функціонал?
1.  **Пояснювальний AI (XAI):** Замість сухої видачі результату моделі `tensorflowModel.py`, Gemini зможе інтерпретувати графіки з `plotMatchOdds.py` та пояснювати: *"Коефіцієнт на господарів різко впав на 15-й хвилині через червону картку, що підтверджується історичними даними з вашої бази"*.
2.  **Синтез новин та статистики:** Gemini може автоматично зчитувати результати з `matchEventsScraper.py` і зіставляти їх із ринковими аномаліями для виявлення "валуйних" ставок.
3.  **Автоматизація тюнінгу:** LLM може аналізувати логи з `evaluation.py` та пропонувати зміни параметрів для скриптів `tuningTensorflowModel.py`.

### Сценарій сервісу (betfair-machine-learning + ID_001 + Gemini)

Ваші базові скрипти (**ID_001**) керують клієнтською частиною сайту (дашборди, сповіщення, Telegram-бот).

**Алгоритм роботи сервісу:**
1.  **Двигун даних (Betfair ML):** Скрипти `betfair.py` та `cleanData.py` у фоновому режимі збирають та чистять дані про live-матчі.
2.  **Аналітичний шар (Gemini):** LLM отримує очищені дані та вихідні сигнали від моделей TensorFlow. Вона генерує текстовий звіт про те, чому модель вважає певну ставку вигідною.
3.  **Доставка (ID_001):** Ваш базовий скрипт забирає цей текстовий звіт і графік, згенерований `plotMatchOdds.py`, та публікує "Розумний Прогноз" на фронтенді вашого сайту.

---

## 📋 План дій для Notion
| Етап | Дія | Результат |
| :--- | :--- | :--- |
| **Data Flow** | Налаштування регулярного запуску `betfair.py` через cron | Постійний потік свіжих даних |
| **AI Layer** | Створення промпта для Gemini, що приймає CSV з `cleanData.py` | Перетворення таблиць на аналітичні висновки |
| **Integration** | Зв'язка виводу `plotMatchOdds.py` з веб-інтерфейсом сайту | Візуальний дашборд для користувачів |
| **ML Feedback** | Використання Gemini для генерації нових фіч у `generateDataset.py` | Постійне покращення точності моделей |

> **Технічна замітка:** Весь проект написаний на **Python 100%**, що робить інтеграцію з Gemini API максимально простою через бібліотеку `google-generativeai`. Код має високу модульність, тому ви можете замінити блок навчання на Gemini для швидкої перевірки гіпотез без довгого тренування TensorFlow-моделей.
