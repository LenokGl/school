# Модель предметной области
<!-- Логическая модель, содержащая бизнес-сущности предметной области, атрибуты и связи между ними. 
Подробнее: https://confluence.mts.ru/pages/viewpage.action?pageId=375782602

Используется диаграмма классов UML. Документация: https://plantuml.com/class-diagram 
-->

```plantuml
@startuml
' Логическая модель данных в варианте UML Class Diagram (альтернатива ER-диаграмме).
namespace User {

 class User
 {
  id : string
  createDate : datetime
  updateDate : datetime
  UserName : string
  UserType : UserType
  cartItems : CartItem[]
 }

 enum UserType
 {
  UserType
 }
 
 class Speaker
 {
  id : string
 }
 
   class Admin
 {
  id : string
 }
  
    class Support
 {
  id : string
 } 
  
 User --  UserType
 User.Speaker -- Schedule.TimeSlot 
 User.Admin -- Schedule.Schedule
 User *-- "1" Speaker
 User *-- "1" Admin
 User *-- "1" Support
}

namespace Schedule {
class Schedule
 {
  id : string
  Task : Task
 }
 
 class Task
 {
 id : string
 TaskNAme:TaskEnum
 }
 
 class StateModel
  {
  id : string
  
  }
  
   enum StateModelkEnum
 {
  Name
 }
  enum TaskEnum
 {
  Name
 }
 
  class TimeSlot
 {
 id : string
 }
 
 Schedule -- Task
 Task -- TaskEnum
 Task -- TimeSlot
 Schedule -- StateModel
 StateModel -- StateModelkEnum
}

namespace Message {
 User.User ..> Message : ref
 Schedule.Schedule ..> Message : ref
}

namespace Translation {
 User.User ..> Translation : ref
 Schedule.Schedule ..> Translation : ref
}
@enduml
```
