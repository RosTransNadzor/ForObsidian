[[Csharp]]

- Для быстрого описания метода или типа используются комментарии
```cs
/// <summary>
/// This is beatiful class
/// </summary>
class Point
{
	public DateTime CreatedOn { get; set; }
}
```
- В code - пишется код и отображается как код
```cs
/// <summary>
/// Some <code>var c = 4+5</code>
/// </summary>
/// <param name="pattern"></param>
/// <param name="s"></param>
/// <returns></returns>
public static bool WordPattern(string pattern, string s)
{}
```
В para (не param) - отображает абзац