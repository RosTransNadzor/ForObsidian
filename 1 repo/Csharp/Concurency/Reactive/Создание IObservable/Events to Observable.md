[[Csharp/Concurency/Reactive/React|React]]

- Только события с типом void(Action)
```cs
public static event EventHandler<int> someEvent;
public static event Action<int> someEvent2;
```
- Преобразовать в IObservable
```cs
// !! в IObservable передается EventPattern
IObservable<EventPattern<int>> obs = Observable.FromEventPattern<int>(
	handler => someEvent += handler,
	handler => someEvent -= handler
);
IObservable<int> obs2 = Observable.FromEvent<int>(
	handler => someEvent2 += handler,
	handler => someEvent2 -= handler
);
```
- С несколькими аргументами
```cs
// данное событие
public delegate void ExtendedNetworkFoundEventHandler(string ssid, int strength);
IObservable> networks = Observable.FromEvent>
( 
	// создание кортежа для обертки события
	rxHandler => 
		(ssid, strength) => rxHandler(Tuple.Create(ssid, strength)), 
	h => wifiScanner.ExtendedNetworkFound += h , 
	h => wifiScanner.ExtendedNetworkFound -= h
);
```
- События без аргументов
```cs
IObservable connected = Observable.FromEvent( 
	h => wifiScanner.Connected += h,
	h => wifiScanner.Connected -= h
);
```
- Либо через ObservableBase (лучше использовать Observable.Create)
```cs
// просто интерфейс класса генерирующего события
public interface IChatConnection
{
	event Action<string> Received;
	event Action Closed;
	event Action<Exception> Error;
	void Disconnect();
}
// Пеобразование класса с событиями
public class ObservableConnection : ObservableBase<string>
{
	private readonly IChatConnection _chatConnection;
	public ObservableConnection(IChatConnection chatConnection)
	{
		_chatConnection = chatConnection;
	}
	protected override IDisposable SubscribeCore(IObserver<string> observer)
	{
		Action<string> received = message =>
		{
			observer.OnNext(message);
		};
		Action closed = () =>
		{
			observer.OnCompleted();
		};
		Action<Exception> error = ex =>
		{
			observer.OnError(ex);
		};
		_chatConnection.Received += received;
		_chatConnection.Closed += closed;
		_chatConnection.Error += error;
		return Disposable.Create(() =>
		{
			_chatConnection.Received -= received;
			_chatConnection.Closed -= closed;
			_chatConnection.Error -= error;
			_chatConnection.Disconnect();
		});
	}
}
```
- Потребление потоков событий
```cs
var connection = new SomeConnection(); 
IObservable observableConnection = new ObservableConnection(connection); 
var subscription= observableConnection.Subscribe(data =>{
	Console.WriteLine(data);
});
```

