- [[FronteEnd/Nest/Controllers|Controllers]]
- 

- Веб фрейворк работающий поверх express для создания backend на js
- Создание проекта - npm
```js
nest new learning
```
- выполнится сборка проекта с следущей структурой

![[structure.png]]

- mait - отвечает за создание приложения - builder
- app.controller - давно знакомый контроллер
- app.service - сервис который контролер получит через DI
- app.module - регистрирует контроллер и сервисы и настраивает DI для них
- app.controller.spec - модульные тесты

- по умолчанию nest запускается на express но это можно изменить на fastify

- Запуск проекта 
```js
npm run start
// перекомпиляция при изменении файлов
npm run start:dev
```

