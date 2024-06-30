[[Миграции]]
```cs
// получение сервиса и очищени блока кода
using(var serviceScope = app.Services.CreateScope())
{
	var context = serviceScope.ServiceProvider.GetService<AppDbContext>();
	if(context != null)
	{
	// асинхронная миграция
		await context.Database.MigrateAsync();
	}
}
// асинхронный запуск
await app.RunAsync();
```
