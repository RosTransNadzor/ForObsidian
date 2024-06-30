[[Auth]]

- User - хранит всю информацию о пользователе
```cs
// В контроллере(только для чтения)
User
// В middleware(можно задать)
app.Use(async (context, next) =>
{
	context.User = new System.Security.Claims.ClaimsPrincipal();
	await next();
});
```
- User -> Identity - Хранит список утверждений<Claim(ключ,значение)>
```cs
app.Use(async (context, next) =>
	{
		// создаем список утверждений
		var claims = new List<Claim>();
		claims.Add(new Claim("name", "anton"));
		// этот спиок хранится в свойсте - identity
		var identity = new ClaimsIdentity(claims);
		// это свойство передается в User и доступно для чтения(в контроллере)
		context.User = new System.Security.Claims.ClaimsPrincipal(identity);
		await next();
});
// в контроллере - поис утверждений
var abc = User.FindFirst("ABC")?.Value ?? "not found"
// есть ли утверждение (с определенным значением?)
User.HasClaim(key,value?)
```