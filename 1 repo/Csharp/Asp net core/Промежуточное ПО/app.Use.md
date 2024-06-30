- Можно создать свое промежуточное ПО для обработки запросов
- С помощью метода 
```cs
// контекст содержит в себе информацию о запросе
// next - передает информацию в следующую часть конвейра
app.Use(async (HttpContext context, Func next) => {
	if (context.Request.Path.StartsWithSegments("/ping")) 
	{ 
		context.Response.ContentType = "text/plain";
		await context.Response.WriteAsync("pong"); 
	// если не вызывается next - остальная часть конвейера 
	//не обработаетданный запрос
	} 
	else {
		 await next();
	}
	// - код который будет на обртном пути конвейра 
});
```
- На данном примере можно понять как аботает конвейер
![[MiddleWare.png]]

```cs
// может находится до вызова next - но всегда этот метод будет выполнен перед отправкой в браузер
context.Response.OnStarting(
	() => { 
	// заголовки которые например нужно задать до отправки в браузер
		context.Response.Headers["X-Content-Type-Options"] = "nosniff"; 
		return Task.CompletedTask; 
	});
```