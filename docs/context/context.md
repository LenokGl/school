# Контекст решения
<!-- Окружение системы (роли, участники, внешние системы) и связи системы с ним. Диаграмма контекста C4 и текстовое описание. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375783261
-->
```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

Person(pbc, "Пользователь", "Лицо желающее принять участие в конференции")
System(ibs, "Система авторизации пользователя", "Позволяет Пользователю зарегистрироваться для участия в конференции")
System_Ext(es, "Система рассылки уведомлений", "Сервис Mail.ru")
System_Ext(mbs, "Личный кабинет пользователя", "Хранит всю информацию о пользователе")

Rel(pbc, ibs, "Пользователь")
Rel(es, pbc, "Отправка e-mails Пользователю")
Rel(ibs, es, "Отправка e-mails", "SMTP")
Rel(ibs, mbs, "Пользователь")
@enduml
```
