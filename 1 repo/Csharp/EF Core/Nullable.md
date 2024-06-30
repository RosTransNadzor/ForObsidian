[[Entity Framework]]

- примитивные типы (например, int) или структуры (например, DateTime) по умолчанию не имеют значения NULL;
- Все типы(строки - ссылочный) кроме нав свойств объявлять с помощью required
```cs
public required string Name{get;set;}
```
- Нав свойства объявлять с помощью null!
```cs
public PriceOffer Promotion { get; set; } = null!;
```
- Примитивные с null
```cs
// в таблице может иметь значение null
public int? Age{get;set;}
```