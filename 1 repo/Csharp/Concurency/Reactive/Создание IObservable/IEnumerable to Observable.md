[[Создание IObservable]]

- Из коллекции в Observable (завершенный наблюдаемый объект)
```cs
IEnumerable names = new []{"Shira", "Yonatan", "Gabi", "Tamir"}; 
IObservable<int> observable = names.ToObservable();
```
- ToEnumerable (блокируя)
```cs
// поток блокируется пока observable не завершится
var enumerable = observable.ToEnumerable();
```
- ToList() or ToArray в неблокирующем режиме
```cs
IList<int> list = await obs.ToList();(или ToArray)
```
- ToDictionary (не блокируя)
```cs
IDictionary<string, int> dict = await obs.ToDictionary(el => el.ToString());
```
- Если методы ToList,ToArray,ToDictionary - не ожидать 
вернется IObservable которая содержит один элемент - коллекция значений
```cs
IObservable<List<int>> listObservable = observable.ToList();
```