[[Auth]]

#### Не использовать для паролей (ключ можно подобрать)
- Для паролей [[Password Hashing]]
- Шифрует данные по ключу
```cs
// внедрение класса шифровки
// program.cs
services.AddDataProtection();
// код контроллера
private readonly IDataProtector _protector;
// получаем поставщика и вводим ключ для данного защитника
public GetCookie(IDataProtectionProvider protector)
{
	_protector = protector.CreateProtector("auth3444");
}
// ...
// protect - создает защифрованную строку по ключу
_protector.Protect(authCookie)
// unprotect - расшифровывает зашифрованную строку(должен быть тот же ключ)
 _protector.Unprotect(authCookie);
```
- Шифровка с ограничением по времени
```cs
var timeProtector = _protector.ToTimeLimitedDataProtector();
// шифруем с установкой времени жизни токена
string encryptedResult = timeProtector.Protect(result, TimeSpan.FromSeconds(100));
// другой файл 
// чтобы расшифровать нужен временный защитник(ToTimeLimitedDataProtector()) 
// с тем же кодом
var name = _protector.ToTimeLimitedDataProtector().Unprotect(authCookie);
```