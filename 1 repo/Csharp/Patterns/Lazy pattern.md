[[Pattenrs]]

- Позволяет создавать "тяжелые" объекты при первом обращении к ним
```cs
private readonly Lazy<Blob> _lazy = new Lazy<Blob>(); 
public Blob BlobData => _lazy.Value;
```
- Создание потокобезопасного отложенного объекта
```cs
private readonly Lazy<Blob> _lazy = new Lazy<Blob>(true); 
```
- Добавление функции для сложной инициализации
```cs
// в func может быть сложная логика
private Lazy<Blob> _lazy = new Lazy<Blob>(() => new Blob());
```