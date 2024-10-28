# QA_internship

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

9. 
