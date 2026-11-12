# Учебный центр: лабораторная работа (PostgreSQL, Sequelize, REST API)

## ER-диаграмма

```mermaid
erDiagram
  TEACHERS ||--o{ COURSES : "ведёт"
  COURSES ||--o{ ENROLLMENTS : "содержит"
  STUDENTS ||--o{ ENROLLMENTS : "записан"
  TEACHERS {
    int id PK
    string full_name
    string email UK
  }
  COURSES {
    int id PK
    string title
    string description
    int hours
    int teacher_id FK
  }
  STUDENTS {
    int id PK
    string full_name
    string email UK
    string group_name
  }
  ENROLLMENTS {
    int student_id PK, FK
    int course_id PK, FK
    date enrolled_at
    int grade
  }
```

## Нормальная форма

Схема находится в третьей нормальной форме. Все атрибуты атомарны (1НФ), а в таблице `enrollments` с составным ключом дата записи и оценка зависят от пары «студент + курс» целиком, а не от её части (2НФ). Нет транзитивных зависимостей: данные преподавателя хранятся только в `teachers`, а в `courses` лежит лишь ссылка `teacher_id`, поэтому имя и email преподавателя не дублируются. Так же сведения о студентах хранятся только в `students`, и аномалий обновления и удаления не возникает.