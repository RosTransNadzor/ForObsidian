[[Async Rx]]

- Перегрузка Observable.Create позволяет вызывать создание и подписку в другом потоке
```cs
public IObservable<int> GeneratePrimes(int amount)
{
	// также предоставляет Cancellation Token Souce
	return Observable.Create<int>((o, ct) =>
	{
		return Task.Run(() =>
		{
			foreach (var prime in Generate(amount))
			{
				// Срабатывает при отписке
				ct.ThrowIfCancellationRequested();
				o.OnNext(prime);
			}
			o.OnCompleted();
		});
	}
```