# 2. Микросервисы
 
Как мы выяснили из первой главы, обзора концепции и технологий, “созданных для облака” (cloud native), практически неотъемлемой частью проектирования и разработки приложений для работы в облаке стали “микросервисы”, особенно бурно ворвавшиеся в тренд популярности на волне успеха стека технологий и способа разработки Twitter, Netflix и до этого идей от Amazon.

Определить точно, что это за архитектура, и чем она формально отличается от очень известного до этого подхода SOA (service oriented architecture), то есть архитектуры ориентированной на сервисы, довольно сложно. Многие копья сломаны на конференциях и форумах, создано множество блогов, можно сделать определенные выводы. Прежде всего микросервисы отличаются от “монолитов” (monolith), приложений, созданных с помощью единой технологии или платформы, внутри которой находятся вся деловая логика системы, анализ данных, обслуживание и выдача данных пользовательским  интерфейсам. Любое взаимодействие модулей, сервисов и компонентов внутри монолита как правило происходит в рамках одного или максимум несколько процессов.

Плюсы монолита очевидны - мгновенная скорость общения между сервисами и компонентами, зачастую в рамках одного процесса, общая база кода, меньшее количества ограничений на общений между компонентами и модулями, менее общие, более точные и выделенные интерфейсы между ними. 

Однако с развитием облачных вычислений, и особенно легких контейнеров, изолирующих любые технологии, возрастанием скорости обмена данных по сети и общей надежности и встроенной устойчивости к отказам, предоставляемых основными провайдерами облака, стало особенно удобно разбивать приложение на множество более мелких приложений. Они предоставляют друг другу сфокусированные, маленькие услуги и сервисы с помощью обмена информацией (как правило текстовый HTTP/JSON или двоичный формат gRPC), независимые по технологиям. Подобное разбиение идеально ложиться на разделение бизнес-функций в общем приложении, а что еще лучше, великолепно разделяет обязанности большой команды инженеров на независимые, способные к экспериментам и использованиям любых технологий, маленькие команды. 

## Монолиты

Красивое слово монолит описывает хорошо известный, наиболее часто используемый способ разработки программного продукта. Ваша команда определяется с набором требований к продукту и делает примерный выбор технологий и архитектуры. 

### Склонность к единой технологии

### Медленное обновление и его гранулярность


### Трудность опробования инноваций




## Архитектура на основе сервисов (SOA)

Более гибким решением является разработка на основе компонентов, отделенных друг от друга, прежде всего на уровне процессов, в которых они исполняются. Архитектуру подобных проектов называют ориентированной на сервисы (service oriented architecture, SOA).

Разработка приложения в виде компонентов, и стремление свести сложные приложения к набору простых, хорошо стыкующихся между собой компонентов известна видимо с тех самых времен как программы стали разрабатывать, да и по большому счету применима во многих областях человеческой деятельности.

Часто говорят, что архитектура на основе сервисов отличалась от текущих “микро”-сервисов тем, что многое отдавала на откуп “посредникам” (middleware), например системам обмена и настройки сообщений между компонентами и сервисами, так называемым брокерам событий (event broker). Микросервисы же эпохи облака минимизируют зависимость от работы со сложными посредниками и их правилами и проводят свои операции напрямую друг с другом.

## Микросервисы


### Известные шаблоны проектирования

Прерыватель (circuit breaker)
Известный пример часто применяемого шаблона проектирования в микросервисах - “прерыватель” (circuit breaker). Название его пришло из известного всем предохранителя в электрических схемах. Как вы помните из элементарных знаний по электричеству, предохранитель размыкает сеть и защищает потребителей электрического тока если какие-то параметры превышают принятые нормы.

В случае сервисов защищают сам сервис и среду (сервер или виртуальное окружение), на котором он работает. Суть заключается в том, что предохранитель отслеживает время ожидания (latency) результата от сервиса, и следит за превышением разумного времени ответа (здесь большое поле для творчества, скорее всего это время вы будете примерно понимать, запуская тесты на производительность, и отслеживая параметры работы сервиса в том облаке, что вы выберете для своего приложения). В том случае если сервис отвечает с большей чем задано задержкой, включается предохранитель - он отказывается обслуживать новые запросы (так что они отправляются теперь на другие процессы такого же сервиса) в течение определенной вами паузы. Пауза выдерживается чтобы дать сервису обслужить уже имеющиеся задачи, и восстановить ресурсы, необходимые для качественного обслуживания новых задач.

### Возможные проблемы микросервисов

Одна из основных и практически не имеющая готовых рецептов проблема микросервисов - разделение их обязанностей. Качественный процесс дизайна и архитектуры приложения подразумевает разделение компонентов, сервисов и объектов, представляющих собой данные, согласно области бизнеса, для которого приложение разрабатывается. Это основа DDD (domain driven design). Однако именно здесь для микросервисов кроется неприятные подводный камень. Дело в том, что разбить компоненты и сервисы заранее очень тяжело, требования, как водяные знаки, появляются лишь по мере проявления приложения, пользователи зачастую полностью меняют свое мнение как только видят первые варианты приложения. 

Однако, это не проблема для монолитов - рефакторинг компонентов и их интерфейсов довольно просто сделать внутри одного процесса и одной базы технологий и языков. В случае микросервисов перенести часть функциональности в другой сервис, зачастую написанный на другом языке или платформе, полностью поломать и поменять крупные интерфейсы REST/gRPC между ними очень непросто.


