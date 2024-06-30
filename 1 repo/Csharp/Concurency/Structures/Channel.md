[[Csharp/Concurency/Structures/Structures]]

```cs
var queue = Channel.CreateBounded<int>(
            new BoundedChannelOptions(2){FullMode = BoundedChannelFullMode.DropOldest}
);
```
- Channel представляет собой асинхронную очередь - в опциях 2 это лимит
FullMode = BoundedChannelFullMode.DropOldest - при переполнении убирать последний элемент
```cs
var writter = queue.Writer;
await writter.WriteAsync(6);
```
- writter поставляет данные в очередь
```cs
await foreach(int i in reader.ReadAllAsync())
{
    Console.WriteLine(i);
}
```
- reader - асинхронно читает данные