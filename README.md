yesdo-admin
===========
Hibernate: 4.3.7.Final

Spring: 4.1.3.RELEASE

Spring-security: 3.2.5.RELEASE

Spring-Data-JPA: 1.7.1.RELEASE

Database: Postgres

ExtJs: 5.x

Logger: logback final version

Cache : Ehcache (final version) или что нормально интегрируется со спринг и хибернейтом.


Основные компоненты:
1. Управление активити

2. Управление мерчантами

3. Управление пользователями

4. Просмотр транзакций

5. Статистика по мерчантам, по продуктам

6. Управление продуктами.

7. Действия пользователей

8. Системные настройки

Стуктура: 
yesdo-admin:

	Пакет: ru.yesdo.admin
	
	Зависимости: 	yesdo-model - список сущностей
	
					yesdo-common-services - список сервисов + репозиториев
					
Все сервисы, не относящие к админке (н-р создание пользователя итп) должны быть вынесены в модуль yesdo-common-services . В yesdo-admin должны быть контроллеры, дто-объекты, всё что относится только к админке. Структура пакетов на собственное усмотрение.
По возможности сделать поддержку локализации.
Все операции с БД должны быть как слой сервисы (делать сервисы через интерфейсы) + spring-data repository
Конфигурация: по возможности через классы (по возможности не использовать XML для конфигов спринга)

	
Авторизация: пользователь заходит под логином , который задаётся в User . Сущность должна реализовать спринговкий интерфейс UserDetails .
Пользователь обладает набором пермиссий, наличие которых позволяет выполнять набор задач. Список пермиссий должен хранится в БД.
Основные пермиссии:
MANAGE_USERS, - доступ к вкладке "Пользователи"

MANAGE_MERCHANTS, - доступ к вкладке "Продавцы"

MANAGE_ACTIVITIES, - доступ к вкладке "Активити"

MANAGE_PRODUCTS, - доступ к вкладке "Продукты"

LIST_STATISTICS, - доступ к владке "Статистика"

LIST_TRANSACTIONS, - доступ к владке "Транзакции"

REVERSE_PERMISSION - возможность выполнять отмену транзакции.

APPLICATION_ADMIN - возможность управлять пользователями, мерчантами, просматривать все транзакции, смотреть стастистику всех мерчантов

REMOVE_PRODUCT - возможность удалять продукт,

LIST_USER_ACTIONS - просмотр данных по действям пользователей

Должна быть возможность смены пароля. Должна быть политика безопасноти по паролям, должна настраиваться в конфиге или через системные настройки.


Вкладка "Активити". 

Активити - род занятий, так как например спорт, парашютный спорт, танцы, боевые искусства итд.
Активити могут в себя включать любое кол-во дочерних элементов, и так же могут иметь любое кол-во родителей
Так например: если мы берём родителя: спорт, то его дочерними элементами являются н-р танцы : [брэкданс, танго: [спортивное танго, латинское танго]],
боевые исскуства : [карате: [карате-до, кькушенкай], бокс, самбо] итд. То есть в данном примере активити спорт имеет
два дочерних элемента, которые имеют под собой своих детей. Но эти дети имеют помимо своих основных родителей ещё также и спорт,
то есть н-р карате-до имеет родителя как карате, так и боевые искусства, так и спорт. Такая связь необходима для поиска.
Также построена система и с мерчантами и с продуктами. Каждый мерчант может находится в нескольких активити, это опять таки
надо для поиска. С продуктами такая же система.

Данная сущность должна хранится в БД.

На вкладке должен отображаться список активити. (делать через пэйджинг, бесконечный скроллинг делать не надо)

Должна быть возможность поиска как по англ. так и по русскому названию активити.
Должна быть возможность открыть в новом окне активити , чтобы можно было редактировать. Удалить нельзя. Должна быть кнопка, "создать".
Должна отображаться информация о кол-ве продавцов, которые присутсвуют в данной активити (делать по отдельному запросу "подсчитать кол-во продавцов").


Вкладка "Продавцы"

На владке должен отображаться список продавцов (делать через пэйджинг, бесконечный скроллинг делать не надо).
Должна быть возможность поиска продавца по англ. названию.
Должны быть все поля, которые есть в сущности. Плюс возможность выставлять опции.
По умолчанию должен быть создан рутовый мерчант с именем yesdo.
Для гео-данных необходимо давать возможность отмечать на карте (яндекс или гугл карты, что проще). То есть просто открывается карта, где указывается точка расположения.
Для админа (APPLICATION_ADMIN) должна быть возможность отображение продавцов на карте (открывается карта, 

Вкладка "Пользователи" 

На владке должен отображаться список пользователей (делать через пэйджинг, бесконечный скроллинг делать не надо). Пользователь, который может смотреть список пользователей (MANAGE_USERS) , видит только пользователей своего мерчанта.
Должны быть возможность создать, редактировать пользователей. Удалять нельзя. 
По умолчанию должен быть создан один пользователь с пермиссией APPLICATION_ADMIN, логин = admin, пароль = admin. 

Вкладка "транзакции" (можно аналогично как сделано в твоей админке)

Пользователь видит только транзакции своего мерчанта. 
На вкладке отображается список транзакций отсортированных по времени. Должен быть фильтр для поиска транзакций. Должна быть возможность открывать отдельную транзакцию для просмотра её деталей. 
Транзакции: так как модуль админки не включает создание транзакции, необходимо создать форму для проведение транзации из личного кабинета. Форма должна содержать основные поля, которые присутсвуют в сущности ProcessingOperation. Для тестирования необходимо сделать возможность генерировать транзации в большом объеме, где указывается кол-во и кнопка: "сгенерировать" + также можно указать мерчанта, по которому производится генерация.

Вкладка "статистика"

Пользователь может смотреть статистику только своего мерчанта. Если это админ (APPLICATION_ADMIN), то есть возможность выбирать мерчанта.
Статистика должна быть по продуктам, по которым были проведены транзакции. Если не сложно, то сделать в виде цветных чартов, где можно видеть какой продукт больше продаётся, выручка по деньгам, можно выбирать временной промежуток времени, за который построить статистику. Должна быть возможность экспортировать результат в виде графиков (пдф или ещё как) - как проще. 

Вкладка "Продукты"

Пользователь может усправлять продуктами только своего мерчанта. Должна быть возможность поиска продукта по параметрам (название, диапозон цен, гео-данным (пока не реализовывать, но учесть) итд - выделить в отдельный класс , н-р SearchCriteriaProduct), возможность редактирование, удаление (REMOVE_PRODUCT), создание. Должна быть возможность установки гео-данных для продукта, отображается карта, выставляется значение. Должна быть возможность выставлять несколько картинок, видео, описывать продукт как блог-тип. Отображаться продукты должны как плитка , то есть должны отображться с аватаром, название, ценой. 
Для админа должна быть возможность просмотра как отдельно по мерчанту, так и всех сразу. Также должна быть возможность просмотра на карте всех продуктов или отдельно по мерчантам. 
Список продуктов сделать или с бесконечным скролом или постранично (как удобнее и надежнее)

Владка "действие пользователей" 

Каждое действие пользователя, которое определенно в ActionType (добавить свои) должны логироваться в БД (UserAction). На данной вкладке должно быть отображение всех этих событий. Доступно если есть LIST_USER_ACTIONS (просмотр только пользователей своего мерчанта) или APPLICATION_ADMIN (всех). Если админ, то должна быть выборка по мерчанту. Должна быть фильтрация по времени и ActionType.

Вкладка "системные настройки"

Доступна только для APPLICATION_ADMIN . Просто пара ключ-значение SystemSetting , хранятся в БД, при запсуке загружаются, находятся в памяти. В yesdo-common-services сервис для управление данным (круд операции, сброс кэша). Кнопки: редактировать, сбросить кэшь. 
