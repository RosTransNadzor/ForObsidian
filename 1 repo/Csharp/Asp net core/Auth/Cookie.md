[[Auth]]

- Куки файл можно получить с помощью Request(запрос)
- и задать с помощью Response(ответ) - в контроллере
```cs
// получить
Request.Cookies.ContainsKey("token")
// по индексу
Request.Cookies["token"]
Request.Cookies.TryGetValue(name,out string? result)
// задать
Response.
	Append(string key, string value) 
	Delete(string key):
```
- Опции для кука
```cs
var cookieOptions = new CookieOptions
{
	// безопасность(false по умолчанию)(HTTPS)
	Secure = true,
	// время жизни
	Expires = DateTime.UtcNow.AddDays(10),
	// не видны в document.cookie(false - по умолчанию)
	HttpOnly = true
};
context.Response.Cookies.Append("refresh", "dasjfosojqwe",cookieOptions);
```
- или другой способ AddCookie - auth
- после куки будут отпраляться при каждом запрос , пока действие куки не завершится (если есть)
```cs
services.AddAuthentication(AuthScheme)
	// authScheme - строка с именем схемы
	.AddCookie(AuthScheme,options =>
	{
		// путь если пользователь не прошел аунтифик.
		options.LoginPath = "/login";
		// путь если нет доступа
		options.AccessDeniedPath = "/login";
		//время до удаления
		options.Cookie.MaxAge = TimeSpan.FromSeconds(20);
		// или время до того как сделать cookie недействительными
		options.ExpireTimeSpan = TimeSpan.FromDays(1);
		// обновлять время до недействительности по запросу
		options.SlideExpiration = true
	});
```
- Как входить для куки [[UseAuthentification]]