[[Создание IObservable]]

- Упрощает создание IObservable
```cs
IObservable<int> observable = Observable.Generate(
	 0, //начальное состояние
	 i => i < 10, //условие(пока будет продолжаться цикл)
	 i => i + 1, //итерированиие
	 i => i * 2); // вывод
```


