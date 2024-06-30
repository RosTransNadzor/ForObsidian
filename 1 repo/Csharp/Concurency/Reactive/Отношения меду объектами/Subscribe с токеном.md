[[Subscribe с токеном]]

- Подписка с токеном отмены
```cs
var cts = new CancellationTokenSource();
cts.Token.Register(() => Console.WriteLine("Subscription canceled"));

Observable.Interval(TimeSpan.FromSeconds(1))
 .Subscribe(x => Console.WriteLine(x), cts.Token);
 
cts.CancelAfter(TimeSpan.FromSeconds(5));

```