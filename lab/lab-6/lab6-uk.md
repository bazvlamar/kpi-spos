## Мета

Мета цієї роботи — ознайомитись з інтерфейсом сокетів ОС для взаємодії програм через мережу.

В результате ее выполнения должно возникнуть понимание принципа сетевого взаимодействия на прикладном уровне и навыки использования сокетов для создания клиентских сетевых приложений.

У результаті її виконання повинне з'явитись розуміння принципу мережевої взаємодії на прикладному рівні, атакож навички використання сокетів для створення клієнтських мережевих додатків.

## Завдання

Мовою С, не використовуючи клієнтські бібліотеки, а працюючи напряму з сокетними з'єднаннями, зв'язатися з одним із наступних сервісів і виконати задані команди.

Для виконання роботи необхідно встановити зазначений сервер на локальний комп'ютер і з'єднання виконувати до хосту `localhost`. Перевірити результати потрібно за допомогою рідних клієнтів або іншим способом, який вказаний у варіанті завдання.


## Інтерфейс сокетів

API сокетів — це стандартний спосіб взаємодії програм через мережу. Воно реалізує клієнт-серверну модель взаємодії. Докладніше про це див. конспект лекції за темою «Мережа».


## Базові мережеві утиліти

Для перевірки працездатності мережі і виконання простих операцій з її використанням в ОС присутній наступний напів-стандартний ряд інструментів.

* Утиліта `ping` дозволяє перевірити доступність певного хоста, IP-адреса або DNS-ім'я якого відомі. Приклад роботи:

        $ ping 8.8.8.8
        PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
        64 bytes from 8.8.8.8: icmp_seq=1 ttl=46 time=50.3 ms
        64 bytes from 8.8.8.8: icmp_seq=2 ttl=46 time=49.8 ms
        ^C
        --- 8.8.8.8 ping statistics ---
        2 packets transmitted, 2 received, 0% packet loss, time 1001ms
        rtt min/avg/max/mdev = 49.885/50.101/50.318/0.311 ms

  В даному випадку можна побачити, що хост `8.8.8.8` доступний і швидкість з'єднання висока (невеликий час відповіді, до сотні міллісікунд:` time = 50.3 ms`). У разі відсутності можливості підключитися до хосту пінг не видає результатів.

* Утиліта `nslookup` дозволяє дізнатись IP-адресу хоста по його DNS-імені. Приклад роботи:

        $ nslookup kpi.ua
        Server:		127.0.1.1
        Address:	127.0.1.1#53

        Non-authoritative answer:
        Name:	kpi.ua
        Address: 77.47.133.222

  У даному випадку ми встановили, що символічному імені хоста `kpi.ua` відповідає IP-адреса `77.47.133.222`.

* Утиліта `netstat` показує поточний стан мережевих з'єднань даного хоста. Приклад роботи:

        $ netstat -nat
        Active Internet connections (servers and established)
        Proto Recv-Q Send-Q Local Address     Foreign Address  State
        tcp        0      0 0.0.0.0:445       0.0.0.0:*        LISTEN
        tcp        0      0 0.0.0.0:139       0.0.0.0:*        LISTEN
        tcp        0      0 192.168.1.2:60014 199.16.156.8:443 ESTABLISHED
        tcp        1      0 192.168.1.2:50013 87.245.216.59:80 CLOSE_WAIT

  В даному випадку на хості активні 4 TCP з'єднання, які знаходяться в різних станах (`LISTEN`, `ESTABLISHED`, `CLOSE_WAIT`).

* Утиліта `telnet` дозволяє встановити TCP з'єднання з заданим хостом за вказаним портом і передавати текстові дані через це з'єднання. Приклад роботи:

        $ telnet rainmaker.wunderground.com 23
        Trying 38.102.137.140...
        Connected to rainmaker.wunderground.com.
        Escape character is '^]'.
        ------------------------------------------------------------------------------
        *               Welcome to THE WEATHER UNDERGROUND telnet service!            *
        ------------------------------------------------------------------------------
        ...

  У цьому прикладі ми підключилися до хосту `rainmaker.wunderground.com` по порту `23` і отримали у відповідь текстову інформацію. Ця утиліта - це базовий клієнт для будь-якого TCP-сервісу, за допомогою якого можна виконувати відлагодження його роботи.

* Утиліта `netcat` — це більш функціональний аналог телнет, який може виконувати як TCP, так і UDP з'єднання, а також працювати не тільки в режимі клієнта, а й сервісу.

* Утиліта `traceroute` дозволяє перевірити наявність мережевого маршруту між нашим хостом та іншим хостом. При цьому вона показує, які проміжні вузли присутні на цьому маршруті. Більш сучасною її альтернативою є утиліта `mtr`.