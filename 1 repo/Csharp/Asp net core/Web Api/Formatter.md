[[Web API]]

- Добавление форматера
```cs
services.AddControllers().AddXmlSerializerFormatters();
```
- Теперь данные можно будет получать в формате XML
- Ограничение формата
```cs
// в данном случае доступен будет только JSON
[Produces("application/json")]
[HttpGet("some")]
public ActionResult<string> Index(int id)
	{
		if (id >= 0 && id < _fruit.Count) return Ok(_fruit[id]);
		return NotFound();
	}
```
- Изменение настроек по умолчанию
```cs
builder.Services.AddControllers(options => 
{ 
    // заставляет вводит формат в браузере
	options.RespectBrowserAcceptHeader = true;
	// отключает дефолтный формат text/plain
	options.OutputFormatters.RemoveType(); 
});
```
