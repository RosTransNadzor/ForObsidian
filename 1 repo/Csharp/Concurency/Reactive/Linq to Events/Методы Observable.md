[[Linq to Events]]

- Concat - объединение наблюдаемых
```cs
IObservable<int> obs = Enumerable.Range(10,10).ToObservable()
// общий наблюдаемый объект
IObservable<int> nums = Enumerable.Range(30,10).ToObservable().Concat(obs)
// ! только после завершения первой наблюдаемой запускается вторая
```
- StartsWith - добавляет IEnumerable в начало Observable
```cs
IEnumerable en = Enumerable.Range(0,10)
// сначала элементы из коллекции en потом все остальное
IObservable<int> nums = Enumerable.Range(20,3).ToObservable().StartsWith(en)
```
- Диапозоны
```cs
Observable.Range(0,10);
```
- Repeat
```cs
// повторяет 2 раза (без параметра бесконечно)
Observable.Range(1, 3).Repeat(2).SubscribeConsole("Repeat(2)");
```
- Do - что-то делает но не влияет на поток
```cs
Observable.Range(1, 5) 
	.Do(x=> { Console.WriteLine("{0} was emitted",x); })
	.Where(x=>x%2==0)
	.Do(x => { Console.WriteLine("{0} survived the Where()", x); })
	.Select(x=>x*3) .SubscribeConsole("final")
```
