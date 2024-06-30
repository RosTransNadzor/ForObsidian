[[Web API]]

- Позволяет просматривать все запросы своего сайта
- Доступен по /swagger/index.html от твоего сайта
- Подключение
```
Nuget: Swashbuckle.AspNetCore
```
```cs
builder.Services.AddSwaggerGen(c =>
{
	c.SwaggerDoc("v1", new OpenApiInfo
	{
		Title = "DefaultApiTemplate",
		Version = "v1"
	});
});
//...
if(app.Environment.IsDevelopment())
{
	app.UseSwagger();
	app.UseSwaggerUI(c => c.SwaggerEndpoint(
	"/swagger/v1/swagger.json", "DefaultApiTemplate v1"));
}
```
- Пример

![[swagger.png]]