[[Razor]]
- [[Forms]]
- В этом файле подключаются тэг-хелперы ко всем razor страницам
```cs
// _ViewImports.cshtml
@using WebApplication1
@namespace WebApplication1.Pages
// подключение
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
```
