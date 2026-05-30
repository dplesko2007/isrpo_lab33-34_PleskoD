# Лабораторная работа №33-34 - Полноценный CRUD с базой данных

**Автор:** Плеско Д.Д. 
**Группа:** ИСП-231 

## Описание проекта

NotesApp — REST API для управления заметками по категориям, реализованное на ASP.NET Core 9 с использованием Entity Framework Core и SQLite. Приложение демонстрирует паттерн Repository, валидацию через Data Annotations, единый формат ответов и связи между таблицами (один-ко-многим).

## Структура проекта

NotesApp/
Controllers/
- CategoriesController.cs
- NotesController.cs
Data/
- AppDbContext.cs
- Helpers/
- ApiResponse.cs
Models/
- Category.cs
- Note.cs
- DTOs/
    - CategoryDtos.cs
    - NoteDtos.cs
    - NoteFilterDto.cs
Repositories/
- ICategoryRepository.cs
- CategoryRepository.cs
- INoteRepository.cs
- NoteRepository.cs
Program.cs
appsettings.json

## Маршруты API

### Категории

| Метод | URL | Описание | Коды ответа |
|-------|-----|----------|-------------|
| GET | `/api/categories` | Все категории с кол-вом заметок | 200 |
| GET | `/api/categories/{id}` | Одна категория по ID | 200, 404 |
| GET | `/api/categories/{id}/notes` | Категория с заметками | 200, 404 |
| POST | `/api/categories` | Создать категорию | 201, 400 |
| PUT | `/api/categories/{id}` | Обновить категорию | 200, 400, 404 |
| DELETE | `/api/categories/{id}` | Удалить категорию | 204, 400, 404 |

### Заметки

| Метод | URL | Описание | Коды ответа |
|-------|-----|----------|-------------|
| GET | `/api/notes` | Заметки с фильтрами и пагинацией | 200 |
| GET | `/api/notes/{id}` | Одна заметка по ID | 200, 404 |
| POST | `/api/notes` | Создать заметку | 201, 400 |
| PUT | `/api/notes/{id}` | Обновить заметку | 200, 400, 404 |
| PATCH | `/api/notes/{id}/pin` | Закрепить / открепить | 200, 404 |
| PATCH | `/api/notes/{id}/archive` | Архивировать / восстановить | 200, 404 |
| DELETE | `/api/notes/{id}` | Удалить заметку | 204, 404 |

## Паттерн Repository

Repository — паттерн проектирования, который изолирует логику работы с базой данных от логики контроллера.
Без Repository: Контроллер -> DbContext -> БД
С Repository:   Контроллер -> Repository -> DbContext → БД

**Преимущества:**
- Все запросы к БД в одном месте — легко поддерживать
- Контроллер не знает деталей реализации БД
- При смене БД меняется только репозиторий
- Можно подменить репозиторий на тестовый без БД

## Основные выводы

1. **Паттерн Repository** изолирует логику доступа к данным от логики контроллера
2. **Data Annotations** валидируют входные данные и задают ограничения в БД
3. **ApiResponse\<T\>** — единый формат ответов, фронтенд всегда знает что ждать
4. **DeleteBehavior.Restrict** защищает данные от случайного каскадного удаления
5. **Include() + Select()** — правильный способ получить связанные данные без N+1 проблемы
6. **Async/Await** не блокирует поток при ожидании ответа от БД