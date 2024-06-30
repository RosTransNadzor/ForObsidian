[[Auth]]

- Для защиты паролей  обычный [[DataProtecter]] - не подойдет(?)
- Вместо него можно использвать BCrypt
- Nuget: BCrypt.Net-Next
- Генерация пароля и соли
```cs
var password = "123";

// хеширование пароля
// 2 параметр work-factor - 11 по умолчанию (от 4 до 31)
// больше защиты
// чем больше - тем больше итераций(меньше производительность)
var enchanced = BCrypt.Net.BCrypt.EnhancedHashPassword(password,31);
var mypassword = Console.ReadLine();

// проверка пароля
var verify = BCrypt.Net.BCrypt.EnhancedVerify(mypassword, enchanced);
Console.WriteLine(enchanced + " enchanced");
Console.WriteLine(verify);
```

