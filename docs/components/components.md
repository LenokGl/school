# Компонентная архитектура
<!-- Состав и взаимосвязи компонентов системы между собой и внешними системами с указанием протоколов, ключевые технологии, используемые для реализации компонентов.
Диаграмма контейнеров C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783368
-->
## Контейнерная диаграмма

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(customer, "Пользователь", "Лицо желающее принять участие в конференции")

System_Boundary(c, "Портал конференции") {
   Container(app, "Клиентское веб-приложение", "html, JavaScript, Angular", "Портал конференции")
   Container(autorization_service, "Авторизация пользователя", "Java", "Сервис авторизации пользователя", $tags = "microService")      
   ContainerDb(user_db, "User Catalog", "MySQL", "Хранение информации о пользователе", $tags = "storage")
   
   Container(presentation_service, "Презентации", "Golang, nginx", "Сервис загрузки презентаций", $tags = "microService")      
   ContainerDb(presentation_db, "Presentation Catalog", "MySQL", "Хранение презентаций", $tags = "storage")
    
   Container(message_bus, "Message Bus", "RabbitMQ", "Транспорт для бизнес-событий")
   Container(audit_service, "Audit Service", "C#/.NET", "Сервис аудита", $tags = "microService")      
   Container(audit_store, "Audit Store", "Event Store", "Хранение произошедших события для аудита", $tags = "storage")
}

System_Ext(e_mail_system, "Mail.ru", "Система рассылки уведомлений.")  

Lay_R(autorization_service, presentation_service)
Lay_R(autorization_service, e_mail_system)
Lay_D(autorization_service, audit_service)

Rel(customer, app, "Портал конференции", "HTTPS")
Rel(app, autorization_service, "Авторизация пользователя", "JSON, HTTPS")

Rel(message_bus, presentation_service, "Загрузка презентации", "AMPQ")
Rel(autorization_service, user_db, "Сохранение информации о пользователе", "JDBC, SQL")

Rel(autorization_service, message_bus, "Пользователь зарегистрирован как докладчик", "AMPQ")
Rel_U(audit_service, message_bus, "Получение события аудита(Событие)", "AMPQ")

Rel(presentation_service, presentation_db, "Сохранение презентации", "SQL")
Rel(audit_service, audit_store, "Сохранение события(Событие)")

Rel(presentation_service, e_mail_system, "Рассылка уведомлений", "SMTP")
Rel(autorization_service, e_mail_system, "Рассылка уведомлений", "SMTP")  

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| *Название компонента* | *Описание назначения компонента* |