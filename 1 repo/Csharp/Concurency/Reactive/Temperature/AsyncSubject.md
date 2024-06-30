[[Rx temperature]]
[[Subject]]

- Хранит последнее значение и постит его после завершения
```cs
AsyncSubject<int> asubject = new AsyncSubject<int>();

asubject.OnNext(2);
asubject.OnNext(5);
asubject.OnCompleted();
// подпиcаться можно после завершения
asubject.Subscribe(x => Console.WriteLine(x));
```

