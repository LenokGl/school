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

Person(speaker, "Докладчик", "Выступающий участник")

Person(admin, "Администратор", "Участник не принимающий непосредственного участия в конференци, обладающий правами доступа ко всем данным системы")

System_Boundary(auth, "Авторизация пользователей") {
 Container(app, "Клиентское веб-приложение", "html, JavaScript, Angular", "Портал конференции")
   Container(autorization_service, "Авторизация пользователя", "Java", "Сервис авторизации пользователя")      
   ContainerDb(user_db, "User Catalog", "MySQL", "Хранение информации о пользователе", $tags = "storage")
 
    

}
System_Boundary(doc, "Загрузка презентаций") {
  Container(presentation_service, "Презентации", "Java", "Сервис загрузки презентаций") 
  
  ContainerDb(presentation_db, "Presentation Catalog", "MySQL", "Хранение презентаций", $tags = "storage") 
  
} 

System_Boundary(re, "Рецензирование презентаций") {
  Container(presentation_review, "Рецензирование презентаций", "", "Сервис рецензирования презентаций")   
} 

System_Boundary(schedule, "Формирование расписания") {
  Container(presentation_schedule, "Формирование расписания", "", "Сервис формирования расписания")   
} 

System_Boundary(mail, "Рассылка уведомлений") {
System_Ext(e_mail_system, "Mail.ru", "Сервис рассылки уведомлений.")  

Rel(customer, app, "Портал конференции", )

Rel(app, autorization_service, "Авторизация пользователя")

Rel(autorization_service, user_db, "Сохранение информации о пользователе" )

Rel( speaker, presentation_service, "Загрузка презентации")
Rel( presentation_service, presentation_db, "Сохранение презентации")

Rel( admin, presentation_review, "Рецензирование презентации")
Rel( presentation_review, presentation_db, "Сохранение результатов рецензирования")

Rel( admin, presentation_schedule, "Рецензирование презентации")

Rel(presentation_service, e_mail_system, "Рассылка уведомлений", "SMTP")
Rel(autorization_service, e_mail_system, "Рассылка уведомлений", "SMTP") 
Rel(presentation_schedule, e_mail_system, "Рассылка уведомлений", "SMTP")  

SHOW_LEGEND()
@enduml
```

## Список компонентов
| Компонент             | Роль/назначение                  |
|:----------------------|:---------------------------------|
| autorization_service | Сервис авторизации пользователя|
| presentation_service | Сервис презентаций|
| message_bus | Шина сообщений|
| presentation_db | База презентаций|
| user_db | База участников|
| e_mail_system|Сервис рассылки уведомлений|
