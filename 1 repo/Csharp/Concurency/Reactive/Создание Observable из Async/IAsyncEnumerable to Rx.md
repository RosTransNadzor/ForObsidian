[[Async Rx]]

```cs
// nuget пакет
using System.Linq.Async
```
- Преобразование
```cs
static async IAsyncEnumerable<int> GetNums()
		{
			for(int i = 0; i<10; i++)
			{
				await Task.Delay(100);
				Console.WriteLine("working");
				yield return i;
			}
		}
// ... 
IObservable<int> obs = GetNums().ToObservable();
IDisposable a = obs.Subscribe(x => Console.WriteLine(x));
// при отмене подписки метод завершится
```