[[HTTPClient]]
- Глава из книги еще не прочитана
- Регистрация зависимостей
```cs
// регистрация фабрики
builder.Services.AddHttpClient();
```
- Именованный клиент
```cs
services.AddHttpClient("instagram", c =>
{
    c.BaseAddress = new Uri("https://instagram.com/");
    c.Timeout = TimeSpan.FromSeconds(10);
});
```
- В контроллерах и прочей чепухе используется Factroy 
```csharp
public class MyController : Controller
{
    private readonly IHttpClientFactory _clientFactory;

    public MyController(IHttpClientFactory clientFactory)
    {
        _clientFactory = clientFactory;
    }
	public async Task<IActionResult> Get()
	{
	    // получение именованного клиента
	    using var client = _clientFactory.CreateClient("instagram");
	}
}
```
- Но в собственных клиентах ипользуестя  HttpClient
```csharp
// Собственный клиент
public class WeatherClient
{
    private readonly HttpClient _client;

    public WeatherClient(HttpClient client)
    {
        _client = client;
        _client.BaseAddress = new Uri("https://weather-api.com/");
        _client.Timeout = TimeSpan.FromSeconds(10);
    }

    public async Task<Weather> GetCurrentWeather()
    {
        var response = await _client.GetAsync("current");
        return JsonConvert.DeserializeObject<Weather>(await response.Content.ReadAsStringAsync());
    }
}
```
- Регистрация собственного клиента в DI
```csharp
services.AddHttpClient<WeatherClient>();
```
- Использование собственного клиента
```csharp
public class MyService
{
    private readonly WeatherClient _weatherClient;

    public MyService(WeatherClient weatherClient)
    {
        _weatherClient = weatherClient;
    }
}
```
