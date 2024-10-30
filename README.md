# Техническая документация для telegram_bot_framework

## Введение

`telegram_bot_framework` — это фреймворк для разработки и управления Telegram-ботами на языке Python. Он упрощает работу с Telegram API, предоставляя структурированную основу для создания, настройки и поддержки ботов. С его помощью можно быстро разворачивать ботов, управлять конфигурацией и использовать общие модули для кода, что делает проект масштабируемым и легким в сопровождении.

## Системные Требования

### Операционная система
- **macOS**: Версия 10.14 (Mojave) или выше. Рекомендуется macOS 11 Big Sur и выше.

### Программное обеспечение
- **Python**: Версия 3.6 или выше.
  - Библиотеки:
    - `python-telegram-bot` — для взаимодействия с Telegram API.
    - `requests` — для выполнения HTTP-запросов.
    - `logging` — стандартная библиотека для логирования.
- **Git**: Для клонирования репозитория и управления версиями.

### Аппаратные требования
- **Процессор**: Минимум двухъядерный, рекомендуется четырехъядерный и выше.
- **Оперативная память**: Минимум 4 ГБ, рекомендуется 8 ГБ и выше.
- **Свободное место на диске**: Не менее 200 МБ.

### Сетевые требования
- **Интернет-соединение**: Постоянное подключение для работы с Telegram API.
- **Открытые порты**: Порты 80 и 443 для HTTP и HTTPS запросов.

### Дополнительно
- **IDE или текстовый редактор**: Visual Studio Code, PyCharm, или Atom.
- **Telegram Bot API Token**: Получите у [BotFather](https://core.telegram.org/bots#botfather) для каждого бота.

## Установка и Настройка

Для работы с `telegram_bot_framework` необходим Python версии 3.6 или выше.

### Установка зависимостей

После клонирования репозитория установите зависимости командой:

pip install -r requirements.txt

Основные библиотеки

Проект использует следующие основные библиотеки:

	•	python-telegram-bot — для работы с Telegram API.
	•	requests — для выполнения HTTP-запросов.
	•	logging — стандартная библиотека для ведения логов.

Пример файла requirements.txt:

Создайте файл requirements.txt в корневой папке проекта и добавьте в него следующий код:

python-telegram-bot==13.0
requests>=2.25.1

Возможные конфликты и их решение

	1.	Конфликт версии библиотеки python-telegram-bot:
	•	Если возникает ошибка несовместимости версий, убедитесь, что используется версия, совместимая с вашим Python (рекомендуется последняя стабильная версия).
	•	Для установки конкретной версии:

pip install python-telegram-bot==13.0


	2.	Ошибка при установке requests:
	•	Убедитесь, что requests не конфликтует с уже установленными библиотеками. В случае ошибки выполните:

pip install --upgrade requests

Обновим описание структуры проекта согласно новому предложению:

## Структура Проекта

telegram_bot_framework/
├── bots/                   # Папка с отдельными ботами
│   ├── bot1/               # Папка первого бота
│   │   ├── config.py       # Конфигурация бота
│   │   ├── bot.py          # Основной файл логики бота
│   │   ├── main_handler.py # Обработчики сообщений
│   │   └── ai_handler.py   # Интерфейс OpenAI или аналогичного сервиса
│   └── bot2/               # Папка второго бота
├── modules/                # Папка с общими модулями
│   ├── translator_client.py# Модуль переводчика
│   ├── logger.py           # Логирование
│   ├── db_client.py        # Подключение к базе данных
│   └── helpers.py          # Дополнительные функции
└── README.md               # Документация

Описание компонентов

	•	bots/ — папка, содержащая отдельные папки для каждого бота. В каждой из этих папок находятся файлы конфигурации (config.py) и основная логика (bot.py) для конкретного бота, а также обработчики сообщений и логика для использования AI.
	•	config.py — файл, содержащий ключевые настройки, такие как токен API, параметры логирования и другие уникальные параметры бота.
	•	bot.py — основной файл логики бота, который инициализирует компоненты бота, такие как переводчик, OpenAI, и запускает обработчик сообщений.
	•	main_handler.py — модуль, отвечающий за обработку входящих сообщений. В нем можно определить правила обработки команд, формирование ответов и событий.
	•	ai_handler.py — интерфейс для взаимодействия с OpenAI API или аналогичными сервисами. Этот файл изолирует логику AI, что упрощает интеграцию и замену моделей.
	•	modules/ — папка с общими модулями и утилитами, которые могут быть использованы разными ботами.
	•	translator_client.py — модуль для работы с сервисами перевода, например, Google Translate или другой API. В этом модуле можно легко изменять или обновлять логику перевода.
	•	logger.py — модуль для централизованного логирования. Этот файл позволяет устанавливать единые правила и форматы логирования для всех компонентов проекта.
	•	db_client.py — модуль для подключения к базе данных, если требуется сохранять данные ботов.
	•	helpers.py — вспомогательные функции, которые могут понадобиться для обработки данных, форматирования или других операций, часто используемых в проекте.
	•	README.md — файл с документацией проекта. Содержит информацию о структуре, установке, настройке и использовании telegram_bot_framework. Включает описание всех основных компонентов и инструкцию по их использованию.
Вот расширенный раздел о логах с полным кодом для копирования:

## Логи

Логи являются важным компонентом для мониторинга работы ботов и отладки. В проекте telegram_bot_framework логи сохраняются в папке logs/, что позволяет отслеживать события, ошибки и другие действия, выполняемые ботами.

Структура папки логов

logs/
├── bot1.log              # Лог файл для первого бота
└── bot2.log              # Лог файл для второго бота

Каждый бот имеет свой собственный лог-файл, что позволяет удобно отделять логи разных ботов и упрощает их анализ.

Настройки логирования

В файле config.py каждого бота можно настроить уровень логирования и другие параметры. Пример настройки логирования:

import logging

# Настройка логирования
LOG_LEVEL = 'INFO'  # Уровень логирования: DEBUG, INFO, WARNING, ERROR, CRITICAL
LOG_FILE = 'logs/bot1.log'  # Путь к файлу лога

# Конфигурация логирования
logging.basicConfig(
    filename=LOG_FILE,
    level=getattr(logging, LOG_LEVEL),  # Установка уровня логирования
    format='%(asctime)s - %(levelname)s - %(message)s'  # Формат логирования
)

Уровни логирования

	•	DEBUG: Подробная информация, полезная для отладки.
	•	INFO: Информация о нормальной работе бота.
	•	WARNING: Уведомления о потенциальных проблемах.
	•	ERROR: Ошибки, возникающие в процессе выполнения.
	•	CRITICAL: Критические ошибки, требующие немедленного внимания.

Примеры использования логирования в bot.py

Для логирования различных событий в коде бота можно использовать следующие команды:

import logging

# Логирование информации о запуске бота
logging.info("Бот запущен и слушает сообщения.")

# Логирование обработки входящего сообщения
def handle_message(update, context):
    logging.info(f"Получено сообщение: {update.message.text}")
    # Ваш код для обработки сообщения

# Логирование ошибок
try:
    # Код, который может вызвать ошибку
    risky_operation()
except Exception as e:
    logging.error(f"Произошла ошибка: {str(e)}")

Проверка логов

Для проверки логов вы можете открыть соответствующий файл в текстовом редакторе или использовать команду tail в терминале, чтобы наблюдать за логами в реальном времени:

tail -f logs/bot1.log

Это позволяет видеть новые записи в логе сразу после их добавления, что удобно для мониторинга работы бота во время его выполнения.

## Запуск и Настройка Ботов

### Настройка ботов

1. **Переход в папку бота**: Перейдите в папку `bots/`, где хранятся отдельные папки для каждого бота.
   
2. **Открытие конфигурации**:
   - В каждой папке с ботом есть файл `config.py`, который содержит ключевые настройки.
   - Откройте `config.py` для редактирования (вы можете использовать текстовый редактор, например, Visual Studio Code или Sublime Text).

3. **Настройки в `config.py`**:
   - **API Token**: Убедитесь, что в конфигурации указан корректный токен Telegram API, полученный у [BotFather](https://core.telegram.org/bots#botfather). Например:
     ```python
     API_TOKEN = 'YOUR_TELEGRAM_BOT_API_TOKEN'
     ```
   - **Настройки логирования**:
     - Уровень логирования можно настроить для отслеживания событий. Рекомендуется установить уровень `INFO` или `ERROR` для начальной настройки. Пример:
       ```python
       LOG_LEVEL = 'INFO'
       ```
     - Логи по умолчанию сохраняются в папке `logs/`. Убедитесь, что у бота есть доступ к записи в эту папку.
   - **Другие параметры**: Настройки могут включать уникальные параметры для вашего бота, такие как приветственное сообщение, тайм-ауты, и ограничения на частоту запросов.

4. **Проверка и сохранение**: Убедитесь, что все параметры указаны корректно, и сохраните файл `config.py`.

### Запуск ботов

1. **Открытие терминала**:
   - Запустите **Терминал** и перейдите в корневую папку проекта `telegram_bot_framework`:
     ```bash
     cd ~/Desktop/telegram_bot_framework
     ```

2. **Запуск бота**:
   - Выполните команду для запуска бота, указав путь к файлу `bot.py` конкретного бота:
     ```bash
     python bots/your_bot_name/bot.py
     ```
     Замените `your_bot_name` на имя папки с ботом, которого вы хотите запустить. 

3. **Проверка работы бота**:
   - После запуска бота в терминале должны отображаться сообщения о его работе, например, "Bot started" или "Listening for messages".
   - Откройте Telegram и отправьте сообщение боту, чтобы проверить, правильно ли он реагирует на команды.
   - При ошибках проверьте логи, которые сохраняются в папке `logs/`, чтобы найти причину проблемы.

### Запуск нескольких ботов

Если необходимо запустить несколько ботов одновременно:
- Откройте новую вкладку терминала для каждого бота.
- Запустите каждого бота в своей вкладке, следуя тем же шагам, как описано выше.

### Запуск нескольких ботов с уникальными конфигурациями

Чтобы запустить несколько ботов одновременно, рекомендуется каждому боту назначить собственные конфигурационные файлы и пути для логов. Это позволит избежать конфликтов при работе с общими ресурсами, такими как токены, логи и уникальные параметры каждого бота.

Структура проекта для нескольких ботов

Предположим, у нас есть проект со следующей структурой:

telegram_bot_framework/
├── bots/
│   ├── bot1/
│   │   ├── config.py
│   │   └── bot.py
│   ├── bot2/
│   │   ├── config.py
│   │   └── bot.py
├── logs/
│   ├── bot1.log
│   └── bot2.log
└── README.md

Пример конфигурации config.py для каждого бота

Для каждого бота необходимо настроить конфигурационный файл config.py с уникальными параметрами. Например:

config.py для bot1:

API_TOKEN = 'TOKEN_BOT1'
LOG_FILE = 'logs/bot1.log'
LOG_LEVEL = 'INFO'
WELCOME_MESSAGE = "Привет от бота 1!"

config.py для bot2:

API_TOKEN = 'TOKEN_BOT2'
LOG_FILE = 'logs/bot2.log'
LOG_LEVEL = 'DEBUG'
WELCOME_MESSAGE = "Привет от бота 2!"

Запуск каждого бота в отдельных терминалах

Чтобы запустить каждого бота, откройте отдельный терминал и выполните команду для каждого бота:

	1.	Для bot1:

python bots/bot1/bot.py


	2.	Для bot2:

python bots/bot2/bot.py



Таким образом, каждый бот будет работать с отдельной конфигурацией и вести свои логи в выделенный файл, что упрощает их анализ и отладку.

Обновление зависимостей

Регулярное обновление зависимостей важно для обеспечения безопасности и функциональности вашего проекта. Чтобы обновить зависимости до последних версий, выполните следующую команду:

pip install -U -r requirements.txt

Эта команда обновит пакеты, перечисленные в файле requirements.txt, до последних доступных версий, совместимых с вашими установленными ограничениями.

Эти инструкции помогут вам поддерживать конфигурации и зависимостями ботов в актуальном состоянии и без конфликтов.


### Завершение работы бота

Для остановки работы бота:
1. В терминале, где запущен бот, нажмите **Ctrl + C**. Это остановит процесс выполнения.
2. Убедитесь, что бот корректно завершил работу, и его сессия закрыта.

## Обработка ошибок

В процессе работы Telegram-бота могут возникать различные ошибки, такие как ошибки сети, проблемы с доступом к API, или ошибки, вызванные некорректными данными. Для того чтобы ваш бот был более устойчивым к сбоям, мы рекомендуем добавить обработку ошибок в ключевые места кода.

### Основные типы ошибок и их обработка

1. **Ошибка подключения (ConnectionError)**:
   - Возникает, когда бот не может подключиться к Telegram API, например, при отсутствии интернета или недоступности API.
   
   ```python
   import logging
   import requests
   from telegram import Bot
   from telegram.error import NetworkError, TelegramError

   def start_bot(api_token):
       bot = Bot(token=api_token)
       try:
           bot.send_message(chat_id='YOUR_CHAT_ID', text="Бот успешно запущен!")
       except NetworkError:
           logging.error("Ошибка сети: проверьте интернет-соединение.")
       except TelegramError as e:
           logging.error(f"Ошибка Telegram API: {e}")

	2.	Ошибка обработки сообщения:
	•	Ошибки могут возникнуть при обработке входящих сообщений от пользователей. Чтобы бот продолжал работать, необходимо перехватывать исключения в обработчиках сообщений.

def handle_message(update, context):
    try:
        message = update.message.text
        logging.info(f"Получено сообщение: {message}")
        # Логика обработки сообщения
    except Exception as e:
        logging.error(f"Ошибка при обработке сообщения: {e}")
        context.bot.send_message(chat_id=update.effective_chat.id, text="Произошла ошибка при обработке сообщения.")


	3.	Повторные попытки при ошибке (Retry):
	•	При временных сбоях соединения можно использовать повторные попытки для выполнения запросов.

import time

def send_message_with_retry(bot, chat_id, text, retries=3):
    for attempt in range(retries):
        try:
            bot.send_message(chat_id=chat_id, text=text)
            break
        except NetworkError as e:
            logging.warning(f"Ошибка сети. Повторная попытка {attempt + 1} из {retries}.")
            time.sleep(2)
        except TelegramError as e:
            logging.error(f"Ошибка Telegram API: {e}")
            break


	4.	Логирование всех исключений:
	•	Логирование помогает понять, когда и где произошла ошибка, и является важным инструментом для отладки. Убедитесь, что у вас настроен лог-файл для записи всех ошибок.

import logging

logging.basicConfig(
    filename="logs/bot_errors.log",
    level=logging.ERROR,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

def example_function():
    try:
        # Ваш код
        pass
    except Exception as e:
        logging.error(f"Произошла ошибка: {e}")



Эти подходы помогут вашему боту стабильно работать даже в условиях нестабильного интернет-соединения или при временных ошибках на стороне API. Такой подход к обработке ошибок сделает код более устойчивым и обеспечит высокую надежность бота.


