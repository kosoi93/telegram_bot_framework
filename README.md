Техническая Документация для Бота по Проверке Кейсов на Политическое Убежище

1. Введение

Этот документ описывает создание Telegram-бота для предоставления пользователям быстрых рекомендаций по кейсам на политическое убежище через загрузку PDF-файла и его автоматическую обработку с помощью OpenAI API. Бот предназначен для упрощения процесса анализа заявлений на убежище, обеспечивая эффективную и быструю обратную связь без необходимости сложного взаимодействия.

2. Структура проекта

asylum_case_bot/
├── bot.py                    # Основной скрипт для запуска бота
├── utils/
│   ├── config.py             # Конфигурация (API ключи)
│   └── logger.py             # Логирование
├── requirements.txt          # Зависимости
└── README.md                 # Документация

	•	bot.py: Главный скрипт запуска бота.
	•	utils/: Вспомогательные модули (конфигурация, логирование).
	•	requirements.txt: Зависимости проекта.
	•	README.md: Основная документация.

3. Конфигурация

Все настройки и ключи API хранятся в файле utils/config.py. Это упрощает управление конфигурацией и обеспечивает централизованное хранение настроек.

Пример config.py:

# utils/config.py

TELEGRAM_BOT_TOKEN = 'your_telegram_bot_token'
OPENAI_API_KEY = 'your_openai_api_key'

# Настройки логирования
LOG_LEVEL = 'INFO'
LOG_FILE = 'bot.log'

Важно: Убедитесь, что файл config.py не добавлен в систему контроля версий (например, .gitignore), чтобы защитить конфиденциальные данные.

4. Основные модули и функции

4.1. bot.py

Инициализирует и запускает Telegram-бота, обрабатывает загрузку PDF-файлов, извлекает текст, отправляет его в OpenAI API для анализа и возвращает рекомендации пользователю.

# bot.py

import logging
from telegram import Update, Bot
from telegram.ext import Updater, CommandHandler, MessageHandler, Filters, CallbackContext
import pdfplumber
import openai
from utils.config import TELEGRAM_BOT_TOKEN, OPENAI_API_KEY, LOG_LEVEL, LOG_FILE
from utils.logger import setup_logging

# Настройка логирования
setup_logging(LOG_LEVEL, LOG_FILE)
logger = logging.getLogger(__name__)

# Инициализация OpenAI API
openai.api_key = OPENAI_API_KEY

def start(update: Update, context: CallbackContext):
    update.message.reply_text('Добро пожаловать! Отправьте ваш PDF-файл с заявлением на убежище для анализа.')

def handle_document(update: Update, context: CallbackContext):
    document = update.message.document
    if document.mime_type != 'application/pdf':
        update.message.reply_text('Пожалуйста, отправьте PDF-файл.')
        return

    file = context.bot.get_file(document.file_id)
    file_path = file.download()

    try:
        with pdfplumber.open(file_path) as pdf:
            text = ''
            for page in pdf.pages:
                text += page.extract_text() + '\n'

        if not text.strip():
            update.message.reply_text('Не удалось извлечь текст из PDF-файла. Пожалуйста, проверьте файл и попробуйте снова.')
            return

        # Отправка текста в OpenAI API для анализа
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "Ты помощник для анализа заявлений на политическое убежище."},
                {"role": "user", "content": f"Анализируй следующий кейс и предоставь рекомендации:\n\n{text}"}
            ],
            max_tokens=500,
            temperature=0.5
        )

        recommendations = response.choices[0].message['content'].strip()
        update.message.reply_text(f"**Рекомендации по вашему кейсу:**\n{recommendations}", parse_mode='Markdown')

    except Exception as e:
        logger.error(f"Ошибка при обработке файла: {e}")
        update.message.reply_text('Произошла ошибка при обработке вашего файла. Пожалуйста, попробуйте позже.')

def main():
    updater = Updater(TELEGRAM_BOT_TOKEN, use_context=True)
    dp = updater.dispatcher

    dp.add_handler(CommandHandler("start", start))
    dp.add_handler(MessageHandler(Filters.document.mime_type("application/pdf"), handle_document))

    logger.info("Запуск Telegram-бота.")
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()

4.2. utils/logger.py

Отвечает за настройку логирования в проекте.

# utils/logger.py

import logging

def setup_logging(level, log_file):
    """Настраивает логирование для проекта."""
    logging.basicConfig(
        level=getattr(logging, level.upper(), logging.INFO),
        format='%(asctime)s - %(name)s - %(levelname)s - %(message)s',
        handlers=[
            logging.FileHandler(log_file),
            logging.StreamHandler()
        ]
    )

4.3. utils/config.py

Хранит конфигурационные параметры и ключи API.

# utils/config.py

TELEGRAM_BOT_TOKEN = 'your_telegram_bot_token'
OPENAI_API_KEY = 'your_openai_api_key'

# Настройки логирования
LOG_LEVEL = 'INFO'
LOG_FILE = 'bot.log'

5. Пример использования ключевых функций

5.1. Загрузка и обработка PDF-файла

Процесс включает в себя получение PDF-файла от пользователя, извлечение текста, отправку его в OpenAI API и возврат рекомендаций пользователю.

Пример запуска процесса:

# bot.py (часть функции handle_document)

def handle_document(update: Update, context: CallbackContext):
    document = update.message.document
    if document.mime_type != 'application/pdf':
        update.message.reply_text('Пожалуйста, отправьте PDF-файл.')
        return

    file = context.bot.get_file(document.file_id)
    file_path = file.download()

    try:
        with pdfplumber.open(file_path) as pdf:
            text = ''
            for page in pdf.pages:
                text += page.extract_text() + '\n'

        if not text.strip():
            update.message.reply_text('Не удалось извлечь текст из PDF-файла. Пожалуйста, проверьте файл и попробуйте снова.')
            return

        # Отправка текста в OpenAI API для анализа
        response = openai.ChatCompletion.create(
            model="gpt-4",
            messages=[
                {"role": "system", "content": "Ты помощник для анализа заявлений на политическое убежище."},
                {"role": "user", "content": f"Анализируй следующий кейс и предоставь рекомендации:\n\n{text}"}
            ],
            max_tokens=500,
            temperature=0.5
        )

        recommendations = response.choices[0].message['content'].strip()
        update.message.reply_text(f"**Рекомендации по вашему кейсу:**\n{recommendations}", parse_mode='Markdown')

    except Exception as e:
        logger.error(f"Ошибка при обработке файла: {e}")
        update.message.reply_text('Произошла ошибка при обработке вашего файла. Пожалуйста, попробуйте позже.')

5.2. Пример взаимодействия пользователя с ботом

Запрос от пользователя:

Пользователь отправляет PDF-файл с заявлением на убежище через Telegram.

Ответ от бота:

**Рекомендации по вашему кейсу:**
• Укажите конкретные случаи преследования и даты инцидентов.
• Предоставьте документы, подтверждающие угрозы и аресты.
• Опишите, как преследования повлияли на вашу безопасность и решение покинуть страну.

6. Улучшения и оптимизации

6.1. Обработка ошибок

Реализованы базовые сценарии обработки ошибок для взаимодействия с внешними сервисами:

	•	OpenAI API:
	•	Лимиты запросов: Логирование ошибки и информирование пользователя о необходимости повторить попытку позже.
	•	Неверные запросы: Логирование и возврат общего сообщения об ошибке.
	•	Telegram API:
	•	Ошибки доставки сообщений: Логирование и информирование пользователя о необходимости повторить попытку позже.

6.2. Кеширование и ограничение запросов

Для упрощения проекта кеширование не реализовано. В будущем можно добавить кеширование результатов анализа для снижения нагрузки на OpenAI API и предотвращения превышения лимитов.

6.3. Персонализация пользователей

В текущей версии бота отсутствуют возможности для персонализации. В будущем можно добавить функции выбора уровня детализации рекомендаций или дополнительных настроек через команды бота.

6.4. Механизм повторной отправки уведомлений

В данной версии бота механизмы повторной отправки не реализованы. При необходимости можно добавить автоматические повторные попытки отправки рекомендаций в случае неудачной доставки сообщения пользователю.

7. Развертывание и эксплуатация

7.1. Установка зависимостей

Создайте файл requirements.txt со следующими зависимостями:

python-telegram-bot==13.7
openai==0.27.0
pdfplumber==0.10.1

Установите зависимости с помощью команды:

pip install -r requirements.txt

Примечание: Рекомендуется использовать виртуальное окружение:

python3.10 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

7.2. Настройка конфигурации

	•	Файл utils/config.py: Внесите ваши ключи API и настройки в файл utils/config.py.

# utils/config.py

TELEGRAM_BOT_TOKEN = '123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11'
OPENAI_API_KEY = 'your_openai_api_key'

# Настройки логирования
LOG_LEVEL = 'INFO'
LOG_FILE = 'bot.log'

Важно: Убедитесь, что файл config.py не добавлен в систему контроля версий, чтобы защитить конфиденциальные данные.

7.3. Запуск бота

Запустите бота командой:

python bot.py

Убедитесь, что бот работает корректно, проверив логи. Бот начнет автоматически обрабатывать сообщения пользователей и предоставлять рекомендации.

7.4. Мониторинг и поддержка

	•	Мониторинг логов: Проверяйте файл bot.log для выявления и устранения ошибок.
	•	Обновления: Регулярно обновляйте зависимости для обеспечения безопасности и стабильности.
	•	Обратная связь: Собирайте отзывы пользователей для улучшения функционала бота.

7.5. Рекомендации по безопасности

	•	Хранение токенов: Используйте файл config.py для хранения токенов и ключей API. Никогда не храните секреты непосредственно в коде.
	•	Ограничение доступа: Ограничьте доступ к файлу config.py только необходимым пользователям и процессам.
	•	Шифрование: Рассмотрите возможность шифрования конфигурационных файлов в производственной среде.
	•	Соблюдение законодательства: Перед внедрением системы убедитесь, что хранение и обработка данных пользователей соответствует законодательству США о защите данных и конфиденциальности, особенно в контексте заявлений на политическое убежище. Рекомендуется проконсультироваться с юридическими специалистами.

8. Тестирование модулей

Для обеспечения стабильной работы бота рекомендуется добавить автоматические тесты для ключевых модулей: анализа PDF, взаимодействия с OpenAI API и логирования.

Структура тестов:

asylum_case_bot/
├── tests/
│   ├── test_bot.py             # Тесты для основного скрипта bot.py
│   ├── test_case_analyzer.py   # Тесты для модуля case_analyzer.py
│   ├── test_error_detector.py  # Тесты для модуля error_detector.py
│   └── test_logger.py          # Тесты для модуля logger.py

Рекомендуемые тесты:

	1.	bot.py: Тестирование корректности обработки загрузки PDF-файлов и взаимодействия с пользователем.
	2.	case_analyzer.py: Тестирование корректности извлечения текста из PDF и подготовки данных для анализа.
	3.	recommendation_engine.py: Тестирование взаимодействия с OpenAI API и получения корректных рекомендаций.
	4.	logger.py: Проверка корректности настройки логирования.

Пример запуска тестов:

pytest tests/

Примеры тестов:

Пример теста для функции анализа кейса:

# tests/test_case_analyzer.py

from bot import handle_document
import pytest

def test_analyze_case(mocker):
    # Мокаем загрузку PDF и извлечение текста
    mocker.patch('pdfplumber.open', return_value=mocker.Mock())
    mocker.patch('openai.ChatCompletion.create', return_value=mocker.Mock(
        choices=[mocker.Mock(message={'content': 'Ваш кейс соответствует требованиям.'})]
    ))
    mock_update = mocker.Mock()
    mock_context = mocker.Mock()

    mock_update.message.document.mime_type = 'application/pdf'
    mock_update.message.document.file_id = 'test_file_id'
    mock_update.message.reply_text = mocker.Mock()

    handle_document(mock_update, mock_context)

    mock_update.message.reply_text.assert_called_once_with(
        "**Рекомендации по вашему кейсу:**\nВаш кейс соответствует требованиям.",
        parse_mode='Markdown'
    )

Пример теста для функции логирования:

# tests/test_logger.py

from utils.logger import setup_logging
import logging
import os

def test_setup_logging(tmp_path):
    log_file = tmp_path / "test.log"
    setup_logging('DEBUG', str(log_file))
    logger = logging.getLogger("test_logger")
    logger.debug("Тестовое сообщение DEBUG")
    logger.info("Тестовое сообщение INFO")
    logger.error("Тестовое сообщение ERROR")

    with open(log_file, 'r') as f:
        logs = f.read()
        assert "Тестовое сообщение DEBUG" in logs
        assert "Тестовое сообщение INFO" in logs
        assert "Тестовое сообщение ERROR" in logs

9. Запуск и тестирование

9.1. Установка зависимостей

Создайте файл requirements.txt со следующими зависимостями:

python-telegram-bot==13.7
openai==0.27.0
pdfplumber==0.10.1
pytest==7.1.2

Установите зависимости с помощью команды:

pip install -r requirements.txt

Примечание: Рекомендуется использовать виртуальное окружение:

python3.10 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

9.2. Настройка конфигурации

	•	Файл utils/config.py: Внесите ваши ключи API и настройки в файл utils/config.py.

# utils/config.py

TELEGRAM_BOT_TOKEN = '123456:ABC-DEF1234ghIkl-zyx57W2v1u123ew11'
OPENAI_API_KEY = 'your_openai_api_key'

# Настройки логирования
LOG_LEVEL = 'INFO'
LOG_FILE = 'bot.log'

9.3. Запуск бота

Запустите бота командой:

python bot.py

Убедитесь, что бот работает корректно, проверив логи. Бот начнет автоматически обрабатывать сообщения пользователей и предоставлять рекомендации.

9.4. Мониторинг и поддержка

	•	Мониторинг логов: Проверяйте файл bot.log для выявления и устранения ошибок.
	•	Обновления: Регулярно обновляйте зависимости для обеспечения безопасности и стабильности.
	•	Обратная связь: Собирайте отзывы пользователей для улучшения функционала бота.

9.5. Рекомендации по безопасности

	•	Хранение токенов: Используйте файл config.py для хранения токенов и ключей API. Никогда не храните секреты непосредственно в коде.
	•	Ограничение доступа: Ограничьте доступ к файлу config.py только необходимым пользователям и процессам.
	•	Шифрование: Рассмотрите возможность шифрования конфигурационных файлов в производственной среде.
	•	Соблюдение законодательства: Перед внедрением системы убедитесь, что хранение и обработка данных пользователей соответствует законодательству США о защите данных и конфиденциальности, особенно в контексте заявлений на политическое убежище. Рекомендуется проконсультироваться с юридическими специалистами.

10. Техническая Поддержка и Сопровождение

10.1. Рекомендации по обновлению библиотек и зависимостей

	•	Регулярные Обновления:
	•	Периодически проверяйте наличие обновлений для всех используемых библиотек и фреймворков.
	•	Использование pip-tools или poetry:
	•	Управляйте зависимостями с помощью инструментов, обеспечивающих совместимость версий.
	•	Тестирование после Обновлений:
	•	После обновления зависимостей обязательно запускайте все тесты для проверки работоспособности системы.

10.2. Инструкции по мониторингу и диагностике производительности

	•	Логирование:
	•	Внедрите систему логирования (например, logging в Python) для отслеживания ошибок и событий.
	•	Мониторинг Системы:
	•	Используйте инструменты мониторинга (например, Prometheus, Grafana) для отслеживания состояния сервера, использования ресурсов и производительности бота.
	•	Анализ Логов:
	•	Регулярно просматривайте логи для выявления и устранения потенциальных проблем.

10.3. Поддержка и устранение проблем в процессе эксплуатации

	•	Создание Канала Поддержки:
	•	Организуйте канал (например, через GitHub Issues или специализированную систему тикетов) для обработки запросов пользователей и сообщений об ошибках.
	•	План Обновлений и Патчей:
	•	Разрабатывайте и внедряйте обновления для исправления выявленных проблем и улучшения функциональности.
	•	Резервное Копирование:
	•	Регулярно выполняйте резервное копирование конфигурационных файлов и других критически важных данных для предотвращения потери информации.

11. Заключение

Данная техническая документация предоставляет упрощенное и сфокусированное руководство по разработке, настройке, эксплуатации и поддержке Telegram-бота для проверки кейсов на политическое убежище. Следуя представленным инструкциям, разработчики смогут быстро понять основные модули и процессы, а также эффективно реализовать и поддерживать бота, обеспечивая его надежность и безопасность.

Лицензия

Данный проект распространяется под лицензией MIT. Подробнее см. файл LICENSE.

Контакты

	•	Email: support@asylumbot.com
	•	Telegram: @AsylumBotSupport
	•	GitHub: Asylum Case Bot Issues

Дополнительные ресурсы

	•	python-telegram-bot Documentation
	•	OpenAI API Documentation
	•	pdfplumber Documentation
	•	Python 3.10 Release Notes

Дата Последнего Обновления: 2024-04-27

End of Documentation