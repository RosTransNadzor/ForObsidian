[[Base]]

- Пример методов расширения
```cs
public static class EnumerabelExtends
{
	public static void ForEach<T>(this IEnumerable<T> coll, Action<T> func)
	{
		foreach (var item in coll)
			func(item);
	}
}
```
- По соглашению классы с методами расширений размещаются в отдельно пространстве
```cs
// импорт пространства имен
using ExtensionMethodsExample;
namespace ProgramNamespace
{
	class Program
	{
		static void Main(string[] args)
		{
			int meaningOfLife = 42;
			Console.WriteLine("is the meaning of life even:{0}",
			meaningOfLife.IsEven());
		}
	}
}
```