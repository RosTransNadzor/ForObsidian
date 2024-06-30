[[Base]]

- <span style="color:red">Ref</span> - прямая ссылка на объект (озночает что если это ссылочный тип он может)
- Ссылка на значимый тип
```cs
// ссылочный тип
public class Action
{
	// ссылочный тип
	public required List<int> ints;
	// тип значение
	public int value;
	public void IterateInts()
	{
		foreach(int i in ints)
	        Console.WriteLine(i);
    }
}
var action = new Action() {value = 45,ints = new List<int>() { 1, 2, 3 } };
// ссылка только на поля,но не на св-ва
ref var a = ref action.value;
// изменяем  action.value
a = 8
```
- Ссылка на ссылoчный тип 
```cs
// ссылочный тип по умолчанию дает ссылку
var arr = action.ints
// изменит action.ints
arr.Add(5);
// однако это не изменит action.ints
arr = new List<int>();

// для изменения необходимо использовать ссылку
ref var arr = action.ints
// прямая ссылка изменит action.value
arr = new List<int>();
```
- Ссылку можно использовать в методе и даже возвращать
```cs
RefReturn(ref action.ints);

static void RefReturn(ref List<int> ints)
{
	ints = new List<int>();
}
```
- Параметр(в методе) <span style="color:red">out</span> работает точно также как ref но гарантирует что передаваемый
- объект должен быть изменен(оператор присвоения)
```cs
RefReturn(out action.ints);

static void RefReturn(out List<int> ints)
{
	// если не изменить объект возникнет ошибка
	ints = new List<int>();
} 
```
- Параметр <span style="color:red">in</span> наоборот гарантирует что объект не будет изменен(оператор присвоения)
```cs
RefReturn(in action.ints);

static void RefReturn(in List<int> ints)
{
	ints.Add(1);
} 
```