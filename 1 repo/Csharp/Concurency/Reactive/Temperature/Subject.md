[[Rx temperature]]

```cs
using System.Reactive.Subjects
```
- Subject - простейшая обертка IObservable для вызова основных методов
- (не сохраняет значения до подписки)
```cs
Subject<int> subject = new Subject<int>();
// подписка на subject
IDisposable sub = subject.Subscribe(x => Console.WriteLine(x));
// публикация
subject.OnNext(1);
subject.OnNext(3);
subject.OnCompleted();
// после oncompleted - это не сработает
subject.OnNext(5);
```
- Публикация значений внуть subject
```cs
Subject<long> subject = new Subject<long>();
// лучше использовать merge
Observable.Interval(TimeSpan.FromSeconds(1)).Subscribe(subject);

IDisposable sub = subject.Subscribe(x => Console.WriteLine(x));
```
