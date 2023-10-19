﻿# README!

Данная утилита используется для получения диагностической информации с системы резервного копирования Кибер Бекап или Акронис Кибер Протект (Acronis Cyber Protect).

Важные замечания:
Программа должна запускаться на хосте с утсановленным сервером управления.
Необходимы root права.


## Поддерживаемые команды:

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Команда&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Описание|
|--|--|
|`-h, --help`| Вывод справки|
|`--create-config`|  Создать новый конфигурационный файл. Эта команда создает конфигурационный файл с найстройками по умолчанию для конкретной ОС в текущей директории. По умолчанию файл должен располагаться в том-же каталоге, где утилита|
|`--config <путь>` |указать путь к конфигурационному файлу. Если конфигурационный файл находится в другом каталоге, можно задать путь.|
|`--base_path` |Указать базовый путь к базам Кибер-бекап. Путь к каталогу с внутренней БД Кибер Бекап если он отличается от пути по умолчанию или не задан в файле конфигурации|
|`--web_api` |Запустить возможность удаленного опроса через Web-Api|

## Получение информации из Кибербекап
|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Команда&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| Описание|
|--|--|
| `--resorce_status` | вывод информации о устройствах|
|-`-with_group` |Выводить в том числе сведения о группах и других организационных единицах(в разработке)|
|`--alert`|вывод журнала сообщений (по умолчанию выводятся только активные предупреждения и ошибки)|
||дополнительные ключи для вывода сообщений:|
|-`a`|вывод всех активных сообщений (включая информационные)|
|`-d`|вывести так-же удаленные (прочитанные) сообщения|
|`-n <N>`| Максимальное количество выводимых записей|
|---------------|-----------------------|
|`--agents`|вывод информации о машинах с установленными агентами|
|`--asm` |вывод информации о Узлах Хранения (storage node)|
|`--archives`|вывод информации о архивах в хранилище|
|`--tapes`|вывод информации о лентах|
|`--backups`|вывод инофрмации о бекапах в хранилище|
|`--vaults`|вывод информации о хранилищах|
||Дополнительные парметры|
|`-p`|Использовать форматированный вывод (добавить в вывод переносы строк и пробелы для лучшей читаемости)|
|`-v` |Подробный, расширенный вывод. Большинство команд по умолчанию выводят минимально необходимую информацию, для вывода максимального количества известных параметров используйте этот ключ.|
|`-d`| Показывать в т.ч. удаленные обекты
|`-n <N>` | Максимальное количество выводимых записей|

                

# Использование
Утилита работает на машине с установленным сервером администрирования. Использование утилиты возможно в двух вариантах: локально или удаленно.

Важно: перед первым запуском в любом режиме необходимо создать конфигурационный файл cyber_api.conf:

    cd /root
    ./cyber_backup_api --create-config

В текущей директории будет создан новый конфигурационный файл для текущей ОС, при необходимости отредактировать его<br>
### Пример:

    {
      "BasePath": "C:\\ProgramData\\Acronis\\",
      "Databases_dir": "Databases",
      "BackupManager_dir": "BackupManager",
      "AlertManager_dir": "AlertManager",
      "VaultManager_dir": "VaultManager",
      "VaultDBextend": "db3",
      "Server_IP": "127.0.0.1, localhost",
      "Server_Port": 8099,
      "Server_Username": "admin",
      "Server_Password": "admin"
    }
|Параметр| Комментарий  |
|--|--|
| BasePath |  Базовый путь для БД Кибер Бекап (устанавливается по умолчанию в зависимости от ОС)|
|Databases_dir, BackupManager_dir, AlertManager_dir, VaultManager_dir| Имена каталогов |
|VaultDBextend| Раширение для БД|
|Server_IP| IP-адрес интерфейса для доступа по api, для доступа по всем интерфейсам укажите "*"|
|Server_Port| TCP порт для api|
|Server_Username| Имя пользователя для доступа к api (если имя пользователя будет не задано то авторизация запрашиваться не будет) |
|Server_Password |  Пароль пользователя(если пароль будет не задан то авторизация запрашиваться не будет)|

## Локальное использование 
При локальном использовании вывод осуществляется в консоль

## Примеры:

Вывести информацию о всех устройствах с кратким выводом:

    sudo ./cyber_backup_api --resorce_status

Вывести информацию о всех устройствах с полным выводом и форматированием вывода:
 
    sudo ./cyber_backup_api --resorce_status -v -p

Вывести информацию о агентах и вывести в форматированном виде:

    sudo ./cyber_backup_api --agents -p
    
Вывести полную информацию о хранилищах

    sudo ./cyber_backup_api --vaults -v
Вывести все активные предупреждения и ошибки из журнала сообщений

    sudo ./cyber_backup_api --alert

Вывести 10 сообщений из журнала сообщений включая информационные

    sudo ./cyber_backup_api --alert -a -n 10



## Использование API
Важно: перед первым запуском в любом режиме необходимо создать конфигурационный файл cyber_api.conf:

    cd /root
    sudo ./cyber_backup_api --create-config
    
Запуск API производится командой:

    sudo ./cyber_backup_api --web_api
Api будет запущен с пармаетрами указанными в конфигурационном файле cyber_api.conf

Доступ к api осуществляется через http, параметры запросов стандартны:

    http://ip-адрес:порт/api_v1/команда?параметр1&параметр2&параметрN

Команды и параметры аналогичны локальному использованию, но без знаков "-" или "--"

Например:

Вывести информацию о всех устройствах с кратким выводом:

    http://127.0.0.1:8099/api_v1/resorce_status?

Вывести информацию о всех устройствах с полным выводом и форматированием вывода:

       http://127.0.0.1:8099/api_v1/resorce_status?p&v

Вывести 10 сообщений из журнала сообщений включая информационные с форматированным выводом

    http://127.0.0.1:8099/api_v1/alert?a&n=10&p


