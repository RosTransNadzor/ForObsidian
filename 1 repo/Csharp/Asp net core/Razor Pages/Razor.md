[[Asp net]]
- [[Пример кода страницы]]
- [[Именование]]
- [[Модель страницы]]
- [[Csharp/Asp net core/Razor Pages/Маршрутизация/Маршрутизация]]

Используется для создания многостраничных приложений с отрисовкой на стороне сервера

Добавление:
```cs
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddRazorPages();
```

```cs
app.UseRouting();
app.MapRazorPages();
```
- Добавление Routing и использование Razor
- Страницы должны находится в папке Pages