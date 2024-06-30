[[Async Rx]]
- В Rx большинство операторов не поддерживают асинхронность, но SelectMany
- поддерживает ожидаемую задачу
- Пример кода с Where
```cs
IObservable<int> obs = Observable.Range(1, 10)
// Select Many с асинхронной задачей
	.SelectMany((number) => isPrimeAsync(number),
		(number, isPrime) => new { number, isPrime })
	.Where(x => x.isPrime)
	.Select(x => x.number);
IDisposable subscription = obs.Subscribe(x => Console.WriteLine(x));
```
- Другой способ
```cs
IObservable<int> primes = Observable.Range(1, 10)
    // в select можно использовать async
	.Select(async (number) => new {number,IsPrime = await svc.IsPrimeAsync(number))
	.Where(x => x.IsPrime)
	.Select(x => x.number);
```