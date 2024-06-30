[[Web API]]

- Контроллеры лучше помещать в папку Controllers
- Это просто cs файлы для управления запросами
- Подключение
```cs
builder.Services.AddControllers();
//...
app.MapControllers();
```
- Пример
```cs
//Friut.cs
using Microsoft.AspNetCore.Mvc;

// пометка контроллера
[ApiController]
public class FruitController : ControllerBase 
{
	List<string> _fruit = new List<string>
	{
		"Pear",
		"Lemon",
		"Peach"
	};
	// обработка get на url fruit
	// данные автоматически генерируются в формате JSON
	[HttpGet("fruit")]
	public IEnumerable<string> Index()
	{
		return _fruit;
	}
}
```
- Пример с маршрутизацией и привязкой
```cs
// маршрутизация по id
[HttpGet("fruit/{id}")] 
// привязка id из маршрута к id из параметра
// string - возвращемый тип
public ActionResult<string> View(int id) 
{ 
	if (id >= 0 && id < _fruit.Count) 
	{ 
	    // код 200 
	    //или OK(_fruit[id])
		return _fruit[id]; 
	} 
	//код 404
	return NotFound(); 
}
//Также BadRequest() - отправляет код 400
```
- Множественная маршрутизация
```cs
//При совпадении одного из маршрутов будет вызван метод
[Route("car/start")] 
[Route("car/ignition")] 
[Route("start-car")] 
public IActionResult Start() 
{ /* Реализация метода */ }
```
- Подробнее о маршрутизации [[Csharp/Asp net core/Razor Pages/Маршрутизация/Маршрутизация]]
- Общий маршрут
```cs
// общий маршрут
[Route("api/car")]
public class CarController 
{ 
	// /api/car/start
	[Route("start")]
	public IActionResult Index(){}
	// /api/car/info
	[Route("info")]
	public IActionResult Info(){}
}
```
- Методы запроса
```cs
//два метода GET и POST
// asp net также поодерживает множество других запросов
[HttpGet("/appointments")] 
public IActionResult ListAppointments() 
{ /* Реализация метода */ } 

[HttpPost("/appointments")] 
public IActionResult CreateAppointment() 
{ /* Реализация метода */ }
// 405 - ошибка возникает при отсутсвии необходимого метода запроса
```
- Модели привязки (подробнее в [[Binding Model]])
```cs
[HttpPost("fruit")] 
public ActionResult Update(UpdateModel model) 
{ 
	if (model.Id < 0 || model.Id > _fruit.Count)  return NotFound();  
	_fruit[model.Id] = model.Name; 
	return Ok(); 
} 
// модель привязки
public class UpdateModel { 
	public int Id { get; set; } 
	[Required] 
	public string Name { get; set; } 
}
```