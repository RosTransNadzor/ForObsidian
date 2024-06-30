[[Csharp]]

- HttpCLient - используется для отправки запросов
- [[Отправка запросов]]
- [[Asp net with HttpClient]]
#### !!! **HttpClient** предназначен для создания экземпляра один раз и повторного использования на протяжении всего потока запросов приложения. Можно создавать несколь для разных настроек (не слишком много)
- Создание объекта соединения
```cs
// нужно только при замене DNS
var socketsHandler = new SocketsHttpHandler
{
  PooledConnectionLifetime = TimeSpan.FromMinutes(2)
};
// создаем статический объект для переиспользования
static HttpClient _client = new(socketsHandler);
// ...
// Базовый Uri
_client.BaseAddress = new Uri("https://jsonplaceholder.typicode.com/");
// Отмена запроса при длительном выполнении (100 секунд здесь)
_client.Timeout = TimeSpan.FromMicroseconds(100);
```

