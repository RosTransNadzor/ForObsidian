[[Csharp/Concurency/Reactive/React]]
```cs
Observable.StartAsync( token => client.GetAsync("http://www.example.com/", token));
```
- возвращает наблюдаемый объект с при отмене подписки отменяется асинхронный метод
```cs
Observable.FromAsync( token => client.GetAsync("http://www.example.com/", token));
```
- тоже что и предыдущий но запускает метод только при подписке
 ###### ! Токен отмены сработает при отписке на поток