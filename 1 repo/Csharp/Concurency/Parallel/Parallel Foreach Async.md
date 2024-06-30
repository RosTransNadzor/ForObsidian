[[Parallel]]
```cs
await Parallel.ForEachAsync(nums, async (i, state) =>
            {
                await Task.Delay(1000);
                hashSet.AddItem(Thread.CurrentThread.ManagedThreadId);
            });
```
- Первый await отвечает за ожидание завершение всех параллельных задач(не обязательно)
- Второй await за ОСВОБОЖДЕНИЕ  потоков в асинхронных задачах которые запускаются параллельно
### При простом вызове:
```cs
Parallel.ForEach(
    parallelOptions: new ParallelOptions { MaxDegreeOfParallelism = 5 },
    source: nums,
    body:async (el,token) =>
    {
        await Task.Delay(500);
        Console.WriteLine(el);
    } 
);
```
- Программа завершится мгновенно  не ожидая завершения методов ( в отличии от ParallelForEachAsync)
### Длительность выполнения

- ParallelForEach - программа выведет все числа очень быстро т.к. потоки освобождаются
```cs
Parallel.ForEach(
    parallelOptions: new ParallelOptions { MaxDegreeOfParallelism = 5 },
    source: nums,
    body:async (el,token) =>
    {
        await Task.Delay(500);
        Console.WriteLine(el);
    } 
);
```
- ParallelForEachAsync - МЕДЛЕННО программа выведет по 5 чисел, несмотря на освобождение потоков
```cs
await Parallel.ForEachAsync(
    parallelOptions: new ParallelOptions { MaxDegreeOfParallelism = 5 },
    source: nums,
    body:async(el,token) =>
        {
            await Task.Delay(500);
            Console.WriteLine(el);
        } 
    );
```
- 2 пример работает более верно и правильно освобождает поток