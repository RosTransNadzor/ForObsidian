[[Создание IObservable]]

- Возможны ошибки из-за температуры последовательностей
- Самый простой способ создания IObservable
```cs
public static IObservable<int> ObserveNumbers(int amount)
{
	return Observable.Create<int>(observer =>
	{
		for (int i = 0; i < amount; i++)
		{
			observer.OnNext(i);
		}
		observer.OnCompleted();
		return Disposable.Empty;
	});
}
```
- (Defer) Отложенное создание с помощью обертки, которая активируется только при подписке
```cs
public IObservable<string> ObserveMessagesDeferred(string user,
 string password)
{
	// При подписке выполнется внутренняя логика и вернется реальная 
	// IObservable
	return Observable.Defer(() =>
	{
		var connection = Connect(user, password);
		return connection.ToObservable();
	});
}
```
- Потребление отложенной последовательности
```cs
// так же как и обычно
var messages = chatClient.ObserveMessagesDeferred("user","password"); 
var subscription1 = messages.SubscribeConsole(); 
var subscription2 = messages.SubscribeConsole();
```