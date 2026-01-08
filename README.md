# Ansible Role: Vector

Роль Ansible для установки и настройки **Vector** — универсального агента сбора и обработки логов и метрик.

Роль устанавливает бинарный файл Vector, настраивает systemd-сервис, конфигурационный файл и необходимые директории.

---

## Требования

- Ansible >= 2.9
- ОС семейства Debian (Ubuntu)
- systemd на целевом хосте
- Доступ в интернет для загрузки бинарного архива Vector

---

## Переменные роли

Все переопределяемые переменные находятся в `defaults/main.yml`.

### Основные параметры

| Переменная | Значение по умолчанию | Описание |
|-----------|----------------------|----------|
| `vector_version` | `"0.48.0"` | Версия Vector |
| `vector_install_dir` | `/opt/vector-{{ vector_version }}` | Каталог распаковки Vector |
| `vector_binary_path` | `/usr/local/bin/vector` | Путь установки бинарного файла |
| `vector_config_dir` | `/etc/vector` | Каталог конфигурации |
| `vector_data_dir` | `/var/lib/vector` | Каталог данных Vector |
| `vector_config_path` | `/etc/vector/vector.toml` | Путь к конфигурационному файлу |
| `vector_download_url` | URL packages.timber.io | URL загрузки архива Vector |

### Пользователь и группа

| Переменная | Значение | Описание |
|-----------|---------|----------|
| `vector_user` | `vector` | Системный пользователь |
| `vector_group` | `vector` | Системная группа |

---

## Что делает роль

- Устанавливает зависимости (`wget`, `tar`, `gzip`)
- Загружает архив Vector
- Распаковывает бинарный файл
- Создаёт пользователя и группу `vector`
- Создаёт необходимые каталоги
- Разворачивает конфигурацию `vector.toml`
- Создаёт и включает systemd-сервис
- Перезапускает сервис при изменении конфигурации

---

## Пример использования

```yaml
- hosts: vector
  become: true
  roles:
    - role: vector-role

vector_version: "0.47.0"
vector_data_dir: "/data/vector"

Обработчики (Handlers)

restart vector — перезапуск сервиса Vector при изменении конфигурации или unit-файла.


---

# README.md для `lighthouse-role`

```md
# Ansible Role: Lighthouse

Роль Ansible для установки **Lighthouse** — web-интерфейса для просмотра данных ClickHouse — и настройки Nginx для его публикации.

---

## Требования

- Ansible >= 2.9
- ОС семейства Debian (Ubuntu)
- Установленный systemd
- Доступ в интернет для загрузки Lighthouse

---

## Переменные роли

Все переопределяемые переменные находятся в `defaults/main.yml`.

### Основные параметры

| Переменная | Значение по умолчанию | Описание |
|-----------|----------------------|----------|
| `lighthouse_dir` | `/var/www/lighthouse` | Каталог размещения Lighthouse |
| `lighthouse_download_url` | GitHub master.zip | URL архива Lighthouse |
| `lighthouse_zip_path` | `/tmp/lighthouse.zip` | Временный путь архива |

### Настройки Nginx

| Переменная | Значение | Описание |
|-----------|---------|----------|
| `lighthouse_nginx_site_name` | `lighthouse` | Имя сайта в Nginx |
| `lighthouse_nginx_listen_port` | `80` | Порт прослушивания |

---

## Что делает роль

- Устанавливает Nginx и вспомогательные пакеты
- Загружает и распаковывает Lighthouse
- Настраивает каталог размещения
- Создаёт конфигурацию Nginx через шаблон
- Активирует сайт Lighthouse
- Отключает сайт Nginx по умолчанию
- Запускает и включает Nginx
- Перезапускает Nginx при изменении конфигурации

---

## Пример использования

```yaml
- hosts: lighthouse
  become: true
  roles:
    - role: lighthouse-role

lighthouse_dir: /opt/lighthouse
lighthouse_nginx_listen_port: 8080

Обработчики (Handlers)

restart nginx — перезапуск сервиса Nginx при изменении конфигурации сайта.


