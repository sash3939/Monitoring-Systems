# Домашнее задание к занятию 13 «Введение в мониторинг» - Егоркин Александр

## Обязательные задания

**ЗАДАНИЕ 1**

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

**ЗАДАНИЕ 2**

Менеджер продукта, посмотрев на ваши метрики, сказал, что ему непонятно, что такое RAM/inodes/CPUla. Также он сказал, что хочет понимать, насколько мы выполняем свои обязанности перед клиентами и какое качество обслуживания. Что вы можете ему предложить?

ОТВЕТ:

RAM - Память, используемая для временного хранения данных и выполнения программ.
inodes - Структуры данных в файловой системе, которые хранят информацию о файлах и каталогах. Ограниченное количество inodes может помешать созданию новых файлов
CPUla - Загрузка ЦПУ показывает, насколько сильно он занят обработкой задач

Для того чтобы сделать метрики более понятными и удобными для анализа с точки зрения качества обслуживания и выполнения обязательств перед клиентами, можно использовать следующие подходы, связав их с понятиями SLA (Service Level Agreement), SLO (Service Level Objective) и SLI (Service Level Indicator)

**Определение ключевых показателей:**

SLA (Service Level Agreement): это соглашение, определяющее уровень услуг, которые клиент ожидает от провайдера. Например, "Платформа должна быть доступна 99.9% времени".
SLO (Service Level Objective): это конкретные цели, которые команда ставит для достижения SLA. Например, "Время отклика на HTTP-запросы должно составлять не более 200 мс для 95% запросов".
SLI (Service Level Indicator): это метрики, которые используются для измерения достижения SLO. Например:
Доступность: процент времени, когда сервис был доступен.
Время отклика: среднее время отклика на HTTP-запросы.

**Перевод технических метрик в понятные клиенту показатели**

_Доступность платформы (Uptime)_
SLI: Процент времени, когда платформа была доступна.
SLO: "99.9% доступности".
Пояснение: платформа была доступна на 99.9% времени в прошлом месяце.

_Время отклика на запросы_

SLI: Среднее время отклика на HTTP-запросы.
SLO: "90% запросов обрабатываются за менее чем 200 мс".
Пояснение: 90% запросов обрабатывались быстрее, чем за 200 мс.

_Число ошибок_

SLI: Количество 4xx и 5xx ошибок за определённый период.
SLO: "Количество ошибок не превышает 1% всех запросов".
Пояснение: Ошибки при обработке запросов составляют менее 1% от общего числа.

Что упростит:
- Создайте регулярные отчёты о выполнении SLO и состоянии сервисов. Это поможет менеджеру продукта видеть прогресс и принимать обоснованные решения.
- Поддерживайте открытый диалог с менеджером продукта, чтобы вместе определять, какие метрики наиболее важны для клиентов, и при необходимости корректировать SLO и SLI в соответствии с изменениями в бизнесе
- Используйте простые и интуитивно понятные термины. Например, вместо "использование RAM" можно говорить "объём памяти, доступный для обработки ваших запросов"
- Предоставляйте данные в виде графиков, диаграмм и дашбордов, чтобы менеджер продукта мог легко визуализировать информацию

**ЗАДАНИЕ 3**

Вашей DevOps-команде в этом году не выделили финансирование на построение системы сбора логов. Разработчики, в свою очередь, хотят видеть все ошибки, которые выдают их приложения. Какое решение вы можете предпринять в этой ситуации, чтобы разработчики получали ошибки приложения?

ОТВЕТ:

1. _Использование встроенных логов._ Логирвоание ошибок в стандартный вывод (stdout) и стандартный вывод ошибок (stderr). Это позволяет легко собирать логи через консоль или оболочку
2. _Инструменты для простого логирования._ systemd или journalctl: Если ваши приложения работают на системах с systemd, можно использовать journalctl для просмотра логов. Использование rsyslog. 
3. _Скрипты для агрегации логов_. Скрипты на Bash или Python, которые будут периодически собирать ошибки из логов и отправлять их по электронной почте разработчикам. Скрипт может запускаться через cron, чтобы проверять файлы логов и отправлять уведомления о новых ошибках.
4._ Использование мессенджеров или уведомлений._ Чат-боты, Телеграм, Slack, Discord
5. _Логирование в базу данных_. Если у вас есть доступ к базам данных, можно записывать ошибки в таблицу. Это позволяет разработчикам выполнять SQL-запросы для извлечения информации об ошибках.
6. _Использование сервиса Sentry_, который позволяет удаленно мониторить баги в фронтенд-приложениях, написанных на JavaScript.
7. _Виртуальные окружения для разработки_. Если разработчики работают в локальных окружениях, убедитесь, что они могут видеть логи прямо в своих средах, например, используя docker logs для контейнеризованных приложений.


**ЗАДАНИЕ 4**

Вы, как опытный SRE, сделали мониторинг, куда вывели отображения выполнения SLA = 99% по http-кодам ответов. 
Этот параметр вычисляется по формуле: summ_2xx_requests/summ_all_requests. Он не поднимается выше 70%, но при этом в вашей системе нет кодов ответа 5xx и 4xx. Где у вас ошибка?

ОТВЕТ:

Проблема связана с тем, что в системе присутствуют HTTP-коды ответов, которые не относятся ни к 2xx, ни к 4xx, ни к 5xx, и они не учитываются в SLA.
HTTP-коды в диапазоне 1xx (информационные ответы) и 3xx (перенаправления) также могут присутствовать, и они могут снижать процентное соотношение успешных (2xx) запросов. Например, если в системе много перенаправлений (3xx-коды), они будут включены в общее количество запросов, что понизит метрику SLA, даже если нет ошибок 4xx и 5xx.

_Необходимо:_
Учесть коды 1xx и 3xx в расчете. Пересмотреть формулу, чтобы исключить из общего количества запросов ответы с кодами 1xx и 3xx, если они не должны влиять на SLA.
Верная формула:
SLA = summ_2xx_requests / (summ_2xx_requests + summ_4xx_requests + summ_5xx_requests) - формула с исключением 1хх и 3хх
Таким образом, вы будете рассчитывать SLA только по тем кодам, которые прямо влияют на качество обслуживания (успешные и ошибочные ответы).

**ЗАДАНИЕ 5**

Опишите основные плюсы и минусы pull и push систем мониторинга.

ОТВЕТ:

**Pull система мониторинга**

Принцип работы: центральный сервер (мониторинг-сервер) инициирует запросы к каждому узлу (например, к агенту на сервере) и собирает метрики.

_Плюсы_:
1. Централизованное управление:
Все запросы отправляются из одной точки, что упрощает настройку и контроль.
Мониторинг-сервер контролирует, какие метрики собираются и с какой частотой.

2. Безопасность:
Клиенты (сервера, с которых собираются метрики) могут быть настроены таким образом, что они просто отвечают на запросы, не отправляя данные. Это снижает риск утечек.
Мониторинг-сервер инициирует все соединения, что уменьшает количество открытых портов на серверах для входящих соединений.

3. Надежность:
Если один из клиентов падает или становится недоступным, мониторинг-сервер это легко обнаружит (метрики перестанут собираться).

4. Гибкость:
Легче добавить новые узлы: мониторинг-сервер просто начинает опрашивать их.
Легче управлять частотой сбора метрик с различных узлов.

_Минусы_:
1. Масштабируемость:
По мере увеличения числа узлов и объема данных мониторинг-сервер может стать узким местом.
Сервер может испытывать задержки при сборе метрик с большого количества клиентов.

2. Сложность настройки брандмауэра:
Мониторинг-сервер должен иметь доступ к каждому узлу через брандмауэры и сети, что может потребовать дополнительной настройки.

3. Сложность обработки короткоживущих процессов:
Метрики собираются только в момент запроса, поэтому процессы, которые завершаются между запросами, могут быть пропущены.

**Push система мониторинга**

Принцип работы: узлы (агенты на серверах) сами отправляют данные на центральный сервер с метриками (мониторинг-сервер).

_Плюсы_:

1.Масштабируемость:
Легче масштабировать на большое количество узлов, так как каждый узел отправляет данные самостоятельно, а нагрузка на мониторинг-сервер распределена по времени.

2.Мониторинг короткоживущих процессов:
Метрики отправляются с узлов по мере их появления, что позволяет отслеживать короткоживущие процессы более эффективно.

3. Прохождение брандмауэров:
Серверы могут отправлять данные через существующие каналы связи (например, HTTPS), что упрощает настройку брандмауэров, так как соединения инициируются клиентами.

4. Низкая зависимость от центрального сервера:
Даже если центральный сервер на время недоступен, агенты могут продолжать собирать и отправлять метрики, когда сервер восстановится.

_Минусы_:

1.Безопасность:
Необходимость открывать входящие порты на мониторинг-сервере, что увеличивает потенциальные риски (например, DDoS-атаки).
Метрики передаются от клиентов, и при неправильной настройке это может привести к утечке данных.

2. Сложность управления:
Труднее централизованно управлять частотой отправки данных и количеством собираемых метрик с каждого узла.
Необходимо конфигурировать каждый узел для отправки метрик на центральный сервер.

3.Обнаружение недоступных узлов:
Если узел "умолкает" (перестаёт отправлять данные), это не всегда легко заметить. Потребуется настраивать дополнительные механизмы проверки доступности агентов.


**ЗАДАНИЕ 6**

Какие из ниже перечисленных систем относятся к push модели, а какие к pull? А может есть гибридные?

    - Prometheus 
    - TICK
    - Zabbix
    - VictoriaMetrics
    - Nagios

ОТВЕТ:

| Система         | Модель                          |
|-----------------|---------------------------------|
| Prometheus      | Pull (Push с Pushgateway)       |
| TICK            | Push                            |
| Zabbix          | Push (Pull с Zabbix Proxy)      |
| VictoriaMetrics | Push/Pull, зависит от источника |
| Nagios          | Pull                            |


**ЗАДАНИЕ 7**

Склонируйте себе [репозиторий](https://github.com/influxdata/sandbox/tree/master) и запустите TICK-стэк, 
используя технологии docker и docker-compose.

В виде решения на это упражнение приведите скриншот веб-интерфейса ПО chronograf (`http://localhost:8888`). 

P.S.: если при запуске некоторые контейнеры будут падать с ошибкой - проставьте им режим `Z`, например
`./data:/var/lib:Z`

<img width="616" alt="Screen Chronograf" src="https://github.com/user-attachments/assets/4d95e9fc-510a-44d8-9927-5cf1b0c5cb73">

























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
2. По желанию можно не ограничивать себя только сбором метрик из `/proc`.


ОТВЕТ:

Python script

'''
!/usr/bin/env python3
import time
import json
import os

Путь для логирования
log_dir = '/var/log'
log_file = time.strftime('%Y-%m-%d') + '-awesome-monitoring.log'
log_path = os.path.join(log_dir, log_file)

Функция для получения метрик из /proc
def get_metrics():
    metrics = {}
    
    Временная метка
    metrics['timestamp'] = int(time.time())
    
    Загрузка процессора (метрика из /proc/loadavg)
    with open('/proc/loadavg', 'r') as f:
        loadavg = f.read().split()
        metrics['loadavg_1min'] = float(loadavg[0])
        metrics['loadavg_5min'] = float(loadavg[1])
        metrics['loadavg_15min'] = float(loadavg[2])
    
    Использование памяти (метрика из /proc/meminfo)
    with open('/proc/meminfo', 'r') as f:
        meminfo = {}
        for line in f:
            key, value = line.split(':')
            meminfo[key] = int(value.split()[0])  
        metrics['mem_total'] = meminfo['MemTotal']
        metrics['mem_free'] = meminfo['MemFree']
        metrics['mem_available'] = meminfo['MemAvailable']
    
    Использование CPU (метрика из /proc/stat)
    with open('/proc/stat', 'r') as f:
        for line in f:
            if line.startswith('cpu '):
                cpu_fields = [int(x) for x in line.strip().split()[1:]]
                idle_time = cpu_fields[3]  # idle time
                total_time = sum(cpu_fields)
                metrics['cpu_idle'] = idle_time
                metrics['cpu_total'] = total_time
                break
    
    Использование диска (метрика из os.statvfs для корневой директории)
    statvfs = os.statvfs('/')
    metrics['disk_free'] = statvfs.f_bfree * statvfs.f_frsize // 1024  # в kB
    metrics['disk_total'] = statvfs.f_blocks * statvfs.f_frsize // 1024  # в kB

    return metrics
def log_metrics():
    metrics = get_metrics()
    with open(log_path, 'a') as f:
        f.write(json.dumps(metrics) + '\n')

if __name__ == '__main__':
    log_metrics()
'''

**CRON расписание**

* * * * * /usr/bin/python3 /home/ansible/script.py

**LOG**

{"timestamp": 1695469200, "loadavg_1min": 0.23, "loadavg_5min": 0.15, "loadavg_15min": 0.10, "mem_total": 16334892, "mem_free": 8373920, "mem_available": 9211840, "cpu_idle": 18273994, "cpu_total": 29183023, "disk_free": 2023488, "disk_total": 4194304}
{"timestamp": 1695469260, "loadavg_1min": 0.24, "loadavg_5min": 0.16, "loadavg_15min": 0.11, "mem_total": 16334892, "mem_free": 8373800, "mem_available": 9211820, "cpu_idle": 18274094, "cpu_total": 29183223, "disk_free": 2023488, "disk_total": 4194304}
{"timestamp": 1695469320, "loadavg_1min": 0.26, "loadavg_5min": 0.18, "loadavg_15min": 0.12, "mem_total": 16334892, "mem_free": 8373700, "mem_available": 9211800, "cpu_idle": 18274194, "cpu_total": 29183423, "disk_free": 2023488, "disk_total": 4194304}
{"timestamp": 1695469380, "loadavg_1min": 0.27, "loadavg_5min": 0.19, "loadavg_15min": 0.13, "mem_total": 16334892, "mem_free": 8373600, "mem_available": 9211780, "cpu_idle": 18274294, "cpu_total": 29183623, "disk_free": 2023488, "disk_total": 4194304}
{"timestamp": 1695469440, "loadavg_1min": 0.29, "loadavg_5min": 0.21, "loadavg_15min": 0.14, "mem_total": 16334892, "mem_free": 8373500, "mem_available": 9211760, "cpu_idle": 18274394, "cpu_total": 29183823, "disk_free": 2023488, "disk_total": 4194304}


---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.
