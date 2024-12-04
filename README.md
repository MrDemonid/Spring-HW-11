# Задание: Используйте Spring Actuator для отслеживания метрик вашего приложения. Настройте визуализацию этих метрик с использованием Prometheus и Grafana.

### Actuator

Представлен в модуле `Web-client`. Содержит эндпоинты:
- health
- metrics
- info
- loggers
- logfile
- env
- prometheus

доступны по адресам: `/actuator/<имя эндпоинта>`

Скриншоты в папке `/pics`

### Micrometer

Пример подключения в сервисе `Order-service`. В нем создаем кастомный класс `MicrometerService`,
для замера времени выполнения какого-либо метода. 

Для примера замеряем полное время выполнения
покупки товара. 
Скриншот в файле `/pics/micrometer-custom.png`

### Prometheus
Создаем имидж прометея из Dockerfile, в котором уже прописан настроенный prometheus.yml:
```shell
docker build -t hw-11-prometheus .
```
И запускаем его:
```shell
docker run -d -p 9090:9090 hw-11-prometheus
```

### Grafana
Сначала собираем контейнер из docker-compose.yaml, для чего
в терминале IDEA выполняем:
```shell
docker compose up -d
```
Через 2-3 минуты после старта контейнера можно
открыть веб-интерфейс графаны:
http://localhost:3000



## Модули.
- Config-server - обеспечивает сервисам доступ к конфигурации на GitHub.
- Eureka-server - регистрирует сервисы и позволяет находить их адреса по их именам.
- Gateway-server - обеспечивает доступ к сервисам через единый адрес и порт.
- Catalog-service - ведет учет товаров и их списание.
- User-service - хранит данные о пользователях, а так же их платежные данные.
- Payment-service - проводит транзакции оплаты заказов пользователями.
- Order-service - создает и сопровождает заказ, вплоть до момента оплаты (или отказа от заказа).
- Web-client - веб-интерфейс магазина.

## Тесты. Где и что используется в интеграционных тестах:
1. Service-Catalog - вместо `@MockBean` использую класс конфигурации.
2. Service-Payment - вместо `@MockBean` использую класс конфигурации.
3. Service-Order - оставил устаревший `@MockBean`, для комплекта.
4. Service-User - для этого теста создается временная база данных, поэтому ничего не мопается. Это максимально приближает тест к реальной работе.

## Тест JMeter.
Тест и пара скриншотов в папке Jmeter-test.

Протестировано 1000 покупок, 10-ю пользователями. В среднем на оформление заказа ушло 3 миллисекунды,
плюс-минус 2 миллисекунды.

## H2-Console

http://localhost:8090/CATALOG-SERVICE/h2-console

http://localhost:8090/ORDER-SERVICE/h2-console

http://localhost:8090/PAYMENT-SERVICE/h2-console

http://localhost:8090/USER-SERVICE/h2-console
