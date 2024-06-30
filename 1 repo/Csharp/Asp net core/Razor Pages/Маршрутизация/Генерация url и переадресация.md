[[Csharp/Asp net core/Razor Pages/Маршрутизация/Маршрутизация]]

##### В Razor Pages:
```cs
var url = Url.Page("Currency/View", new { code = "USD" });
```
- Сначала указывается относительный путь, потом данные
при этом они преобразуются в часть Url если они есть в шаблоне
[[Шаблоны маршрутизации]]

##### В MVC:
```cs
// по сравнению с Razor Pages пути идут задом наперед
var url = Url.Action("View", "Currency", new { code = "USD" });
```
##### Redirect
```cs
return RedirectToPage("/Product", new {code = "hi"});
```
- Таким же образом данные указываются в перенаправлении на страницу
##### Пути
"/Product" - абсолютный путь
"Product" - относительный (в данной папке)
##### Link Generator
- Также для Url можно воспользоваться LinkGenerator
[Linkgenerator](https://learn.microsoft.com/ru-ru/dotnet/api/microsoft.aspnetcore.routing.linkgenerator?view=aspnetcore-7.0)