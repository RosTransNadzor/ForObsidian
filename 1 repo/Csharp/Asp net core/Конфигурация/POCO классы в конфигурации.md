[[Конфигурация]]

- Для упрощения конфигурации лучше хранить объекты в классах
```cs
//класс для записи конфигурации
public class AppDisplaySettings 
{ 
	public string Title { get; set; } 
	public bool ShowCopyright { get; set; } 
}
```
```json
// необходимая секция в appsettings.json
// ... другой код json
"AppDisplaySettings": {
    "Title": "Acme Store Locator",
    "ShowCopyright": true
}
// ...другой код json
```
- регистрация
```cs
// сразу нескольких 
services.Configure<MapSettings>( Configuration.GetSection("MapSettings")); services.Configure<AppDisplaySettings( Configuration.GetSection("AppDisplaySettings"));
```
- Это является частью [[Внедрение зависимостей]] и в итоге в контейнере регистрируется объект IOptions как Singleton [[Жизненный цикл зависимостей]]
- Потребление PoCo объектов
```cs
// Index.cshtml.cs
public AppDisplaySettings settings { get; set; }
// использование через внедрение зависимостей
// необходимый объект содержится в options.Value
public IndexModel(IOptions<AppDisplaySettings> options)
{
	settings = options.Value;
}
```
- Перезагрузка при изменении options
```cs
// просто изменить на IOptionsSnapshot
public IndexModel(IOptionsSnapshot<AppDisplaySettings> options)
{
    settings = options.Value;
}
```
- Для данной привязки все свойства должны быть открытыми, а класс не должен быть абстрактным
- Внедрение зависимостей без IOptions
```cs
var settings = new MapSettings (); Configuration.GetSection("MapSettings").Bind(settings); 
// обязательно singleton
services.AddSingleton(settings);
```
```cs
//потребление класса при обычном внедрении зависимостей
// теряется горячая перезагрузка как при IOptionsShanpshot
public class MyMappingController 
{ 
	private readonly MapSettings _settings; 
	public MyMappingController(MapSettings settings)
	{
		 _settings = settings;
	} 
}
```
