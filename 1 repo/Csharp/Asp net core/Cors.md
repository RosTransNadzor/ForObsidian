[[Asp net]]

- Cors - необходим для клиента на другом домене
```cs
services.AddCors();
//...
app.UseCors(x => {
	x.WithOrigins("https://localhost:7147");
	x.AllowAnyHeaders();
	x.AllowCredentials();
	x.AllowAnyMethod();
});
```