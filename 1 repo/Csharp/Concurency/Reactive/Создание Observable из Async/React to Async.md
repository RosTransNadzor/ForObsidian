[[Csharp/Concurency/Reactive/React]]

```cs
IObservable observable = ...; 
int nextElement = await observable.FirstAsync();
```
- ожидание первого элемента в потоке событий
```cs
IObservable observable = ...; 
IList allElements = await observable.ToList();
```
- ожидание завершения потока и получение всего списка
```cs
IObservable observable = ...; 
int lastElement = await observable.LastAsync();
```
- ожидание последнего элемента после завершения потока
```cs
int b = await obs.ToTask()
```
- получение задачи которая после завершения будет содержать последний элемент
(завершается после OnCompleted)