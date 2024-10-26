# Задание 1
Запустите два simple python сервера на своей виртуальной машине на разных портах
Установите и настройте HAProxy, воспользуйтесь материалами к лекции по ссылке
Настройте балансировку Round-robin на 4 уровне.
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy.
***
### Конфиг haproxy.

  frontend http_front
       bind *:85
       stats uri /haproxy?stats
       default_backend http_back

   backend http_back
       balance roundrobin
       server server1 127.0.0.1:8888 check
       server server2 127.0.0.1:9999 check
       server server3 127.0.0.1:7777 check

![1](1.jpg)

# Задание 2
Запустите три simple python сервера на своей виртуальной машине на разных портах
Настройте балансировку Weighted Round Robin на 7 уровне, чтобы первый сервер имел вес 2, второй - 3, а третий - 4
HAproxy должен балансировать только тот http-трафик, который адресован домену example.local
На проверку направьте конфигурационный файл haproxy, скриншоты, где видно перенаправление запросов на разные серверы при обращении к HAProxy c использованием домена example.local и без него.

### Конфиг haproxy.

   frontend http_front
       bind *:85
       mode http
       stats uri /haproxy?stats
       acl host_example hdr(host) -i example.local:85
       use_backend servers if host_example

   backend servers
       mode http
       balance roundrobin
       option httpchk GET /nginx/1.18.0/
       http-check expect status 200
       server server1 127.0.0.1:8888 check weight 2
       server server2 127.0.0.1:9999 check weight 3
       server server3 127.0.0.1:7777 check weight 4
       
![1](2.jpg)
