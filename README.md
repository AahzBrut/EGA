# EGA
Enterprise Game Architecture

## Цель: 
- Ускорить процесс разработки игр казуальной и гиперказуальной направленности с помощью шаблонных решений основных задач;
- Упростить командную разработку за счет снижения вероятности конфликтов при мерже новых фич;
- Сократить время подключения к проекту новых разработчиков;


### Используемые сторонние ассеты:
1. Zenject - все связи между сервисами/контроллерами/репозиториями игры только через DI.
1. GSTU - интеграция с гугл таблицами.

### Верхнеуровневая структура проекта:
- Статические справочники (загружаемые из гугл таблиц);
- Модель данных игры, содержащая все необходимые для сохранения стейта сущности;
- Репозитории моделей данных - сервисы, управляющие инциализацией, получением и сохранением моделей;
- Сервисы - основная игровая логика, события, предоставление API доступа к данным моделей;
- Контроллеры - управление визуальным представлением игры, получение данных для визуализации из сервисов;

### Связи между компонентами:
- Репозитории знают только о моделях и других репозиториях (ZInject);
- Сервисы знают только о репозиториях и других сервисах (о моделях только через репозитории) - ZInject;
- Контроллеры знают только о сервисах (ZInject) и других контроллерах (Inspector inject);
- Собыйтийная модель на подписках и инциации событий сервисов, SignalBus не нужна;

### Интеграция с гугл таблицами:
- Класс интеграции (database) должен обладать следующим функционалом:
  * Загрузка и сохранение в SO списка записей;
  * Загрузка в настраеваемый каталог записей в виде SO;

### Инициализация репозиториев и сервисов (Zenject)
- SO инициализатор с настройками (корневой каталог, флаг перезагрузки после окончания компиляции)

Репозитории и сервисы размещаются каждые в своей структуре каталогов,
Инициализатор собирает и сохраняет их в сериализуеиый список, который
Используется для поднятия их экземпляров при старте игры в контексте Zenject 
Данный подход упростит и ускорит добавление новых сервисов и сведёт к нулю конфликты при мержах
