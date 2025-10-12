1. Задание 1. Создание Server Pool для backend (10 баллов)

    - Создайте у себя в home dir 2 папки - ~/backend1 и ~/backend2, в каждой из которых должен лежать 1 index.html
        - `"<h1>Response from Backend Server 1</h1>"` для backend1
        - `"<h2>*** Response from Backend Server 2 ***</h2>"`  для backend2

   ![](./screenshots/create_backends.png)

    - Внутри этих папок запустите питоновый http сервер на портах 8081 и 8082 соответственно

   ![](./screenshots/backend1_start.png)

   ![](./screenshots/backend2_start.png)

   ![](./screenshots/curl_backends.png)

2. Задание 2. DNS Load Balancing с помощью dnsmasq (20 баллов)

    - При помощи dnsmasq создайте 2 A записи
        - `my-awesome-highload-app.local,127.0.0.1`
        - `my-awesome-highload-app.local,127.0.0.2`

   ![](./screenshots/dnsmasq_conf.png)

    - Запустите dnsmasq и при помощи dig обратитесь к 127.0.0.1 для резолва my-awesome-highload-app.local

   ![](./screenshots/dnsmasq_restart.png)

   ![](./screenshots/dig.png)

    - Проанализируйте вывод. Что произойдет с DNS записями, если backend2 сервер сломается?

   Ничего не исчезнет само по себе: dnsmasq не делает health-check бэкендов и продолжит отдавать обе A-записи, поскольку
   это просто раунд робин. Клиент попавший на нерабочий адрес (например, backend2 недоступен) увидит ошибку уже на этапе
   подключения. Чтобы DNS динамически исключал мёртвые адреса, нужны внешние проверки, например как с nginx в 4 задании

3. Задание 3. Балансировка Layer 4 с помощью IPVS (35 баллов)

    - Создайте dummy1 интерфейс с адресом 192.168.100.1/32. Используя ipvsadm создайте VS для TCP порта 80, ведущего в
      127.0.0.1:8081 и 127.0.0.1:8082, использующего round-robin тип балансировки.

   ![](./screenshots/dummy1_create.png)

   ![](./screenshots/setup_ipvs.png)

    - Используя curl, сходите в http://192.168.100.1, продемонстрируйте счетчики на ipvs, убедитесь, что балансировка
      происходит

   ![](./screenshots/curl_loop.png)

   ![](./screenshots/cat_curl.png)

   ![](./screenshots/ipvs_stats.png)

   Изначально в скриптах была допущена ошибка, поэтому после ее исправления они были запущены снова. Отсюда странное
   количество открытых соединений в выводе ipvsadm

    4. Задание 4. Балансировка L7 с помощью NGINX (35 баллов)

        - Создайте пул из 127.0.0.1:8081 и 127.0.0.1:8082 в nginx с active-backup балансировкой

       ![](./screenshots/nginx_conf.png)

       ![](./screenshots/curl_nginx.png)

        - Убедитесь, что переключение на backup сервер происходит после 7 неудачных попыток сходить в активный сервер.
          Nginx
          должен слушать только 127.0.0.1 tcp порт 8888 и проставлять заголовок X-high-load-test 123, проксируя в
          апстрим.
          Покажите при помощи tshark http запрос и ответ.

       Запустим на рабочих серверах

       ![](./screenshots/curl_nginx_loop_alive.png)

       Выключим backend1

       ![](./screenshots/curl_nginx_loop_dead.png)

       ![](./screenshots/cat_curl_nginx.png)

       Запустим tshark

       ![](./screenshots/tshark_run.png)

       ![](./screenshots/tshark_log1.png)

       ![](./screenshots/tshark_log2.png)

       Видим в ответе наш заголовок
