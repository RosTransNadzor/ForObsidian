[[Auth]]

- Есть специальный  middleware -
```cs
services.AddAuthentication();
// ...
// UseAuthentication - параметр схема по умолчанию
app.UseAuthentication("cookie")
	//возвращает файл куки по которому вас можно будет идентифицировать
	// параметр - обрабатываемая схема
	.AddCookie("cookie");

app.Map(async (context, next) =>
	{
		var claims = new List<Claim>
		{
			new Claim("ABC", "anton")
		};
		// принимает имя схемы по которой вошел пользователь
		var identity = new ClaimsIdentity(claims,AuthScheme);
		var user = new ClaimsPrincipal(identity);
		// выполняет вход пользователя и принимет схему
		await context.SignInAsync(AuthScheme,user);
		// есть еще LogOutAsync() - для выхода
		await next();
	});
```
- После этого пользователь будет считаться со входом
- При перезагрузке страницы тоже - пока не удалить файл куки(у них нет временип хранения)