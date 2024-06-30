[[Entity Framework]]

- Для агрегатов Max, Min, Sum и  Average(Кроме Count) требуется результат, допускающий значение NULL
- Документация MySql например: "методы аггрегации могут возвращать null " - поэтому к этому нужно быть готовым
```cs
// пример аггрегации
// (decimal?) - необходимое преобразование
decimal? result = context.Books.Max(x => (decimal?)x.Price).
```
- метод Aggregate не выполняется в базе - выходит ошибка
- он выполняется только на клиенте [[Вычисления не в базе данных]]
```cs
// ToList() - на клиенте
var result = context.Books.Select(b => b.Price).ToList().Aggregate((p1,p2)=> p1+p2);
```
- Group By - в конце необходимо всегда использовать ToList()
```cs
var something = await _context.SomeComplexEntity 
	.GroupBy(x => new { x.ItemID, x.Item.Name }) 
	.Select(x => new { Id = x.Key.ItemID, Name = x.Key.Name, MaxPrice = x.Max(o => (decimal?)o.Price) }) 
	.ToListAsync();
```
- Методы Take,Skip - рекомендуестя использовать только после GroupBy