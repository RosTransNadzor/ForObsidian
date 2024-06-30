[[Base]]

- Генератор случайных чисел для паролей
```cs
var randomNumber = new byte[32];

using (var rng = RandomNumberGenerator.Create())

{
	rng.GetBytes(randomNumber);

	return Convert.ToBase64String(randomNumber);

}
```