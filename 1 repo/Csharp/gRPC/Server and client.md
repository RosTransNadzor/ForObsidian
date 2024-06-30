[[gRPC]]

- Сервер представляет собой приложение asp net core
```cs
services.AddGrpc();

app.MapGrpcService<TodoService>();  
app.MapGrpcService<DuplexService>();

// todoService - определение сервиса
public class TodoService : TodoCrud.TodoCrudBase  
{  
    private readonly ILogger<TodoService> _logger;  
    private readonly MyContext _context;  
    public TodoService(ILogger<TodoService> logger,MyContext context)  
    {        _logger = logger;  
        _context = context;  
    }  
    public override async Task<Status> CreateTodo(TodoModel request, ServerCallContext context)  
    {        var todo = new Todo()  
        {            IsCompleted = request.IsCompleted,  
            Name = request.Name  
        };  
        _context.Todos.Add(todo);  
        try  
        {  
            await _context.SaveChangesAsync();  
        }        catch (Exception ex)  
        {            return new Status()  
            {                Message = ex.Message  
            };  
        }  
        return new Status()  
        {            Message = "successfully added"  
        };  
    }
	...
}
// todo.proto
syntax = "proto3";  
  
option csharp_namespace = "gRPC";  
  
package greet;  
  
// The greeting service definition.  
service TodoCrud {  
  // Sends a greeting  
  rpc CreateTodo (TodoModel) returns (Status);  
  rpc UpdateTodo(UpdateRequest) returns(Status);  
  rpc DeleteTodo(DeleteRequest) returns(Status);  
  rpc FindOneTodo(FindOneRequest) returns(TodoModel);  
  rpc FindAllTodo(FindAll) returns(stream TodoModel);  
}  
  
// The request message containing the user's name.  
message TodoModel {  
  string name = 1;  
  bool is_completed = 2;  
}  
message UpdateRequest{  
  int32 todo_id = 1;  
  bool is_completed = 2;  
}  
message DeleteRequest{  
  int32 todo_id = 1;  
}  
message FindOneRequest{  
  int32 todo_id = 1;  
}  
message FindAll{}  
// The response message containing the greetings.  
message Status {  
  string message = 1;  
}

```
- Описание клиента (консольное приложение)
```cs
// создание подключения к серверу
var channel = GrpcChannel.ForAddress("http://localhost:5148"); 
// описание клиентов по адресу
var client = new TodoCrud.TodoCrudClient(channel);  
var clientDuplex = new Duplex.DuplexClient(channel);

var response = await client.CreateTodoAsync(new TodoModel()  
{  
    IsCompleted = true,  
    Name = ""  
});
```
- Клиент в asp net core (когда asp net - клиент gRPC)
```cs
// внедрение зависимостей
builder.Services.AddGrpcClient<Greeter.GreeterClient>(o => 
	{ o.Address = new Uri("https://localhost:5001"); }
)
// получение через внедрение
private readonly Greeter.GreeterClient _client; 
public AggregatorService(Greeter.GreeterClient client) { _client = client; }
```
- Отмена запросов(через добавление дедлайна)
```cs
var response = await client.SayHelloAsync( 
	new HelloRequest { Name = "World" }, 
	// отмена запроса через 5 секунд
	deadline: DateTime.UtcNow.AddSeconds(5));
// на сервере
public override async Task<HelloReply> SayHello
	(HelloRequest request, ServerCallContext context) 
{ 
	// контекст содержит cancellation token
	var user = await _databaseContext.GetUserAsync(request.Name, context.CancellationToken);
	return new HelloReply { Message = "Hello " + user.DisplayName }; }
```
- Распостранение цепочки - один дедлайн на всю цепочку вызовов 
```cs
// в deadline - передаем deadline
// в итоге будет один общий дедлайн на всю цепочку вызовов
var response = await client.GetUserAsync( new UserRequest { Id = request.Id }, deadline: context.Deadline);
```
- Ручная отмена на клиенте
```cs
var response = await clientTodo.CreateTodoAsync(
	new TodoModel()  
	{  
	    IsCompleted = true,  
	    Name = ""  
	},
	// передаем заголовки
	headers:new Metadata(){},	
	// передаем свой токен отмены						 
	cancellationToken:cts.Token,
	// деделайн  - автоматический токен отмены
	deadline:DateTime.Now.AddSeconds(5)
);
```
- Загаловки
```cs
// на сервере они содержатся  в контексте
context.RequestHeaders
// получение по ключу
var userAgent = context.RequestHeaders.GetValue("user-agent");
foreach(var header in context.RequestHeaders)
{
	Console.WriteLine($``"{header.Key}: {header.Value}");
}
// на клиенте
usin var call = client.SendMessageAsync(new Request());

`// получаем ответ`

Response response = await call.ResponseAsync;

Console.WriteLine($``"Response: {response.Content}");

var headers = await call.ResponseHeadersAsync;

// создание заголовков
Metadata responseHeaders = new Metadata();

responseHeaders.Add("secret-code","123445");

// отправка на клиенте
var response = await clientTodo.CreateTodoAsync(
	new TodoModel()  
	{  
	    IsCompleted = true,  
	    Name = ""  
	},
	// передаем заголовки
	headers:new Metadata(){},	
	// передаем свой токен отмены						 
	cancellationToken:cts.Token,
	// деделайн  - автоматический токен отмены
	deadline:DateTime.Now.AddSeconds(5)
);
// отправка на сервере
Metadata responseHeaders = new Metadata();

responseHeaders.Add("secret-code","123445");

await context.WriteResponseHeadersAsync(responseHeaders);
```
- Авторизация происходит как в ванильном asp net core - к примеру можно сгенерировать jwt и передать его в заголовке клиенту и ожидать что он его отправит
- 
```cs
// получение контекста http из контекста сервиса gRPC
var user = context.GetHttpContext().User;
// к сервисам также применяются аттбирубы [Authorize(имя политики если есть)]
[Authorize("MyAuthorizationPolicy")] 
public class TicketerService : Ticketer.TicketerBase { }
```