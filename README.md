# QA_internship

## Установка Linux и запуск регрессионых тестов

1. Переходим на зеркало официального репозитория PostgreSQL на GitHub, смотрим название ветки со стабильной 15-й версией и клонируем исходный код

![image](https://github.com/user-attachments/assets/d98c8ed6-c5be-43b5-8831-31ca6ebecc93)

2. Создаём директорию для конфигурации, запускаем конфигурицию дерева установки (дополнительно указал место будущей установки)

![image](https://github.com/user-attachments/assets/8129c8a1-690b-4665-9941-3418cbfe53f5)

3. Соберём сервер и запустим регрессионные тесты, только что собранного сервера перед установкой, чтобы проверить работоспособность данного билда

![image](https://github.com/user-attachments/assets/c82a90f1-ed9d-42de-89e7-8f596f1efd0f)

Все тесты пройдены успешны

![image](https://github.com/user-attachments/assets/9cbcaf6f-1500-476e-b3d1-b6431ef2039e)

4. Установим сервер

![image](https://github.com/user-attachments/assets/c191d361-eebb-49c3-8af5-f9a627ea8d0a)

5. Для удобства дальнейшей работы с установленным сервером, пропишем в скрипте запуска оболочки переменные окружения для разделяемых библиотек, бинарников и документации PostgreSQL

![image](https://github.com/user-attachments/assets/d7ac2052-2d1c-4b3e-a205-95d72688510b)

6. При помощи утилиты pg_ctl инициализируем базу данных в указанной дериктории

![image](https://github.com/user-attachments/assets/e3ce4eb8-aba0-4d19-9712-e2475526269f)

7. При помощи утилиты pg_ctl запускаем Postgres, в качестве БД указываем путь до ранее созданой базе

![image](https://github.com/user-attachments/assets/7ad309eb-adce-4752-a1de-c5c807e900fb)

8. Через интерактивный терминал psql подключимся к базе данных "postgres" и выведем ID процесса

![image](https://github.com/user-attachments/assets/e0d1e7e2-8a09-49fe-9877-1d18ef1e645a)

9. Запустим регрессионные тесты для текущей инсталяции

![image](https://github.com/user-attachments/assets/5d19be95-7d2b-43fc-b896-4374d559e386)

Все тесты пройдены успешны

![image](https://github.com/user-attachments/assets/6ec03254-74d4-4a2c-99ca-47eed332da61)

Установка PostgreSQL завершена.

## Изменения конфигурации тестового окружения и проведения тестов

1. Переходим в [официальную документацию](https://postgrespro.ru/docs/postgresql/15/runtime-config-resource) на сайте PostgresPro в главу 20 "Настройка серера" и ищем параметы, потенциальное именение которых может привести к непрохождению регрессионых тестов. Например в [разделе 20.4.1 Память](https://postgrespro.ru/docs/postgresql/15/runtime-config-resource#RUNTIME-CONFIG-RESOURCE-MEMORY) находим описание параметра "shared_buffers", размер памяти для буферов. В каталоге данных, в кофигурационном файле "postgresql.conf" пробуем уменьшить данный параметр со стандартных 128 МБ до минимального 128 кБ.

![image](https://github.com/user-attachments/assets/c2a08b75-32ac-4ea8-b91e-6b34d97ffccf) ![image](https://github.com/user-attachments/assets/788ec2f4-0f2b-44c2-924b-98bedfbb509d)

4. Перезапустим сервер через утилиту pg_ctl и убедимся в изменении значения параметра

![image](https://github.com/user-attachments/assets/e7f1bfd7-e6a3-4a65-92ac-09abf475af8f)

5. Запустим регрессионый тесты для текущей инсталяции (как делали это ранее) и дождёмся их выполнения

13 из 213 тестов не были пройдены

![image](https://github.com/user-attachments/assets/572e4b4e-59fe-4549-aa19-be800b3e0a8d)

7. Список всех тестов и их статус можно узнать в файле "regression.out" (путь до него выводится в терминал)

![image](https://github.com/user-attachments/assets/73b22e6c-5e10-4d8b-bc69-e8ab00741b38)

8. Посмотрим содержимое файла "regression.diffs" (путь до него выводится в терминал), который сгениеровался по окончанию теста, чтобы узнать ошибки которые привели сбою теста

Сразу видим 2 ошибки типа "no unpinned buffers available" в тесте "rangetypes" 
![image](https://github.com/user-attachments/assets/819ab9df-479c-4b69-a6e2-063dfb16bfe3)

Аналогичная ошибка присутствует во всех тестах

Полный [отчёт](https://docs.google.com/document/d/1-upYhPXWS3By14Y7cMnnnp77qKlIMxL3wwTQTWefgVM/edit?usp=sharing) о данной ошибке 

## SQL-запросы

Для того чтобы перегрузить сервер можно использовать рекурсию, которая будет выполняться бесконечно, заполнять память, тем самым снижая быстродействие системы и в конечном итоге сервер и вовсе может завистнусть

![image](https://github.com/user-attachments/assets/9ab49b03-7571-47d5-bc09-c7abbb220b60)

Либо сделать запрос который создаёт огромное количество строк в таблице с объёмными данными, что тоже может привести к исчерпыванию ресурсов, приводящее к падению сервера

![image](https://github.com/user-attachments/assets/65c044c8-48fc-4cef-8404-328a98477147)

Это и подтверждает логи. Ошибка "the database system is in recovery mode" зачастую указывает на не хватку памяти, что и приводит к падению сервера. 

![image](https://github.com/user-attachments/assets/0da3caab-ac97-49a2-8553-4cc9717c2b17)


