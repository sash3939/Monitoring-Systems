# Домашнее задание к занятию 13 «Введение в мониторинг»

## Обязательные задания

**ЗАДАНИЕ 1******
   Вас пригласили настроить мониторинг на проект. На онбординге вам рассказали, что проект представляет из себя платформу для вычислений с выдачей текстовых отчётов, которые сохраняются на диск. 
Взаимодействие с платформой осуществляется по протоколу http. Также вам отметили, что вычисления загружают ЦПУ. Какой минимальный набор метрик вы выведите в мониторинг и почему?

ОТВЕТ:

**Загрузка процессора (CPU)**
Вычисления загружают ЦПУ, мониторинг использования процессора является приоритетным. Это поможет отслеживать, не приближается ли система к пределам своих возможностей.

_Метрики_:
- Средняя загрузка ЦПУ (в процентах).
- Загрузка по ядрам.
- Количество процессов, ожидающих ЦПУ (load average).
- Температура процессора и датчиков системы: полезно для серверов, особенно при высоких вычислительных нагрузках.

**Использование памяти (RAM)**
Нагрузки в процессе вычислений могут потреблять много оперативной памяти. Недостаток памяти может привести к использованию swap'а или сбоям в работе платформы.

_Метрики_:
- Использование оперативной памяти (в абсолютных значениях и процентах).
- Объем использованного swap (индикатор нехватки памяти).

**Доступность и время отклика HTTP-сервера**
Взаимодействие с платформой осуществляется по протоколу HTTP, поэтому важно отслеживать ее доступность и время отклика, чтобы удостовериться, что она отвечает на запросы.

_Метрики_:
- Время ответа на HTTP-запросы (latency).
- Статус-коды HTTP (чтобы отслеживать ошибки 4xx и 5xx).
- Количество активных соединений.

**Дисковое пространство и I/O операции**
Важно следить за достаточностью дискового пространства, отчеты созраняются на диск. Также стоит отслеживать активность дисковых операций, так как интенсивный I/O может снижать общую производительность.

_Метрики_:
- Доступное дисковое пространство.
- Нагрузка на диск (операции чтения/записи в секунду).
- Время отклика диска (disk latency).

**Ошибки в работе приложения**
Ошибки в работе приложения могут указывать на проблемы в логике вычислений или интеграции. Логирование ошибок поможет оперативно выявлять и исправлять неполадки.

_Метрики_:
- Логирование критических ошибок или исключений (по уровням log severity).

**Дополнительно**: Нагрузка на сеть: если результаты вычислений передаются по сети, можно отслеживать объем передаваемых данных и активность сети.


2. Менеджер продукта, посмотрев на ваши метрики, сказал, что ему непонятно, что такое RAM/inodes/CPUla. Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и какое качество обслуживания. Что вы можете ему предложить?

3. Вашей DevOps-команде в этом году не выделили финансирование на построение системы сбора логов. Разработчики, в свою очередь, хотят видеть все ошибки, которые выдают их приложения. Какое решение вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?

3. Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA = 99% по http-кодам ответов. 
Этот параметр вычисляется по формуле: summ_2xx_requests/summ_all_requests. Он не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?

## Дополнительное задание* (со звёздочкой) 

Выполнение этого задания необязательно и никак не влияет на получение зачёта по домашней работе.

_____

Вы устроились на работу в стартап. На данный момент у вас нет возможности развернуть полноценную систему 
мониторинга, и вы решили самостоятельно написать простой python3-скрипт для сбора основных метрик сервера. 

Вы, как опытный системный администратор, знаете, что системная информация сервера лежит в директории `/proc`. Также знаете, что в системе Linux есть  планировщик задач cron, который может запускать задачи по расписанию.

Суммировав всё, вы спроектировали приложение, которое:

- является python3-скриптом;
- собирает метрики из папки `/proc`;
- складывает метрики в файл 'YY-MM-DD-awesome-monitoring.log' в директорию /var/log 
(YY — год, MM — месяц, DD — день);
- каждый сбор метрик складывается в виде json-строки, в виде:
  + timestamp — временная метка, int, unixtimestamp;
  + metric_1 — метрика 1;
  + metric_2 — метрика 2;
  
     ...
     
  + metric_N — метрика N.
  
- сбор метрик происходит каждую минуту по cron-расписанию.

Для успешного выполнения задания нужно привести:

* работающий код python3-скрипта;
* конфигурацию cron-расписания;
* пример верно сформированного 'YY-MM-DD-awesome-monitoring.log', имеющий не меньше пяти записей.

Дополнительная информация:

1. Количество собираемых метрик должно быть не меньше четырёх.
1. По желанию можно не ограничивать себя только сбором метрик из `/proc`.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.
