# Gibbon Docker Setup

## Краткое резюме проекта

**Gibbon** — гибкая платформа с открытым исходным кодом для управления школой.

- **Версия**: 31.0.00
- **Язык**: PHP 8.0+
- **Лицензия**: GPL-3.0
- **Документация**: https://docs.gibbonedu.org

## Docker конфигурация

### Запуск проекта

```powershell
# Сборка и запуск контейнеров
docker compose up --build -d

# Установка зависимостей (после первого запуска)
docker exec gibbon-web bash -c "COMPOSER_PROCESS_TIMEOUT=600 composer install --no-dev --optimize-autoloader --no-interaction"
```

### Доступные сервисы

| Сервис | URL | Описание |
|--------|-----|----------|
| **Gibbon** | http://localhost:8080 | Основное приложение |
| **phpMyAdmin** | http://localhost:8081 | Управление базой данных |

### Данные для подключения к БД

| Параметр | Значение |
|----------|----------|
| Хост | `db` (внутри Docker) |
| База данных | `gibbon` |
| Пользователь | `gibbon` |
| Пароль | `gibbon123` |
| Root пароль | `rootpassword` |

### Управление контейнерами

```powershell
# Остановить контейнеры
docker compose down

# Запустить снова
docker compose up -d

# Посмотреть логи
docker compose logs -f

# Посмотреть статус
docker compose ps

# Перезапустить
docker compose restart
```

## Созданные файлы

- `docker-compose.yml` — конфигурация Docker Compose
- `Dockerfile` — образ PHP 8.2 + Apache с расширениями

## Модули Gibbon

Платформа включает 28 функциональных модулей:
- Planner, Markbook, Timetable, Calendar
- Students, Attendance, Behaviour, Individual Needs
- Staff, User Admin, Departments
- Finance, Activities
- Formal Assessment, Rubrics, Tracking, Reports
- School Admin, System Admin, Admissions
- Library, Messenger, Data Updater, Form Groups

## Технологический стек

- Slim 4.0, Twig 3.3
- MySQL 8.0 (PDO)
- TCPDF, mPDF, PHPSpreadsheet
- Omnipay (PayPal, Stripe)
- Google API, Microsoft Graph, OAuth2
- PHPMailer, двухфакторная аутентификация

## Настройка русского языка

### Активация русского языка

```sql
-- Активировать русский язык
UPDATE gibboni18n SET active = 'Y' WHERE code = 'ru_RU';

-- Установить русский как системный язык по умолчанию
UPDATE gibboni18n SET systemDefault = 'N' WHERE systemDefault = 'Y';
UPDATE gibboni18n SET systemDefault = 'Y', installed = 'Y' WHERE code = 'ru_RU';
```

### Через Docker:

```powershell
docker exec gibbon-db mysql -uroot -prootpassword -e "UPDATE gibbon.gibboni18n SET active = 'Y' WHERE code = 'ru_RU';"
docker exec gibbon-db mysql -uroot -prootpassword -e "UPDATE gibbon.gibboni18n SET systemDefault = 'N' WHERE systemDefault = 'Y'; UPDATE gibbon.gibboni18n SET systemDefault = 'Y', installed = 'Y' WHERE code = 'ru_RU';"
```

### Статус локализации

| Параметр | Значение |
|----------|----------|
| Файл | i18n/ru_RU/LC_MESSAGES/gibbon.po |
| Фраз для перевода | ~7,569 |
| Переведено | ~635 фраз (~8%) |

Перевод неполный. Для участия в переводе: support@gibbonedu.org

---
*Создано: 14 января 2026*
