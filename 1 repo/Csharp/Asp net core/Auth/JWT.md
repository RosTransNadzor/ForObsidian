[[Auth]]

- Nuget
```cs
Microsoft.AspNetCore.Authentication.JwtBearer
```
- Добавление аунтификации с помощью jwt подразумевает создание токена и отправка его клиенту
- Клиент добавляет к каждому запросу заголовок(так система сможет его увидеть)
```cs
Authorization : Bearer <токен>
```
- Добавляет аунтификацию с помощью jwt(автоматическая проверка)
```cs
// также можно задать схему аунтификации
services.AddAuthentication()
	.AddJwtBearer(o =>
	{
		o.TokenValidationParameters = new TokenValidationParameters
		{
			// задавать отправителя(необязательно)
			ValidateIssuer = false,
			// задавать получателя(необязательно)
			ValidateAudience = false,
			// проверка ключа(обязательно)
			ValidateIssuerSigningKey = true,
			// токен с временем действия
			ValidateLifetime = true,
			// обязательно если нужно время действия
			ClockSkew = TimeSpan.Zero,
			//задать ключ
			IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("qwertydfkjljdfjfigjdsfio"))
		};
	});
```
- Отправка jwt клиенту
```cs
app.Map("/login", async(context) =>
{
	// вписать тот же ключ и с алгоритмом шифрования
	var signingCredentials = new SigningCredentials(new SymmetricSecurityKey(Encoding.UTF8.GetBytes("qwertydfkjljdfjfigjdsfio")), SecurityAlgorithms.HmacSha256);

	// создание токена
	var jwt = new JwtSecurityToken(
		// утверждения - пойдут в User после аунтификации(также как и в cookie)
		claims: new List<Claim> { new Claim("id", "5") },
		// время действительности токена
		expires: DateTime.UtcNow.AddSeconds(100),
		// ключ
		signingCredentials: signingCredentials
	);
	// создание строки из jwt
	string jwtSigned = new JwtSecurityTokenHandler().WriteToken(jwt);
	// отправка клиенту
	await context.Response.WriteAsync(jwtSigned);
});
```
- Чтобы пройти аунтификацию нужно отправлять заголовок с токеном

![[jwt.png]]

#### Время жизни ограничивает время для пользования(15 мин макс) а для обновления его используется [[Jwt Refresh token]]

- Ручная проверка
```cs
services.AddAuthentication()
	.AddJwtBearer(o =>
	{
		//для автоматической проверки  необходимо оставить эти парамерты пустыми
		o.TokenValidationParameters = new TokenValidationParameters
		{
		};
	});
```
- Ручной проверкой нужно самостоятельно читать токен из заголовка и самостоятельно валидировать токен,читать утверждения пользователя,не будет работать аттрибут [Authorize] - вместо него нужно будет все делать вручную