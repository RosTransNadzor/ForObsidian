[[Types]]

- [[Шаблоны RegEx]] - для построения выражений
- Подключение
```cs
using System.Text.RegularExpressions;
```
- Запись регулярного выражения
```cs
Regex regex = new Regex(@"туп(\w*)");
```
- Поиск совпадений
```cs
MatchCollection matches = regex.Matches(s);
//если конечно совпадения есть
foreach(Match m in matches)
	Console.WriteLine(m.Value)
```
```cs
// есть ли совпадение
regex.IsMatch(s)
```
- Игнор регистра
```cs
Regex regex = new Regex(@"туп(\w*)", RegexOptions.IgnoreCase);
```
- Замена
```cs
// простая замена
string s = "nan fdgaf nan"
Regex reg = new Regex("nan")
string news = "fe"
// fe fdgaf fe
string replaced = reg.Replace(s,news);
```
- Замена  с делегатом
```cs
string s = "nan gor nan";
Regex reg = new Regex(@"(nan|gor)");
// делагат принимает match объект и возвпащает строку
string replaced = reg.Replace(s, (match) =>
{
    switch (match.Value)
	{
		case "nan": return "a";
		case "gor": return "b";
		default: return string.Empty;
	}
});
// вывод: a b a
```
- Также есть split для разделения по шаблону