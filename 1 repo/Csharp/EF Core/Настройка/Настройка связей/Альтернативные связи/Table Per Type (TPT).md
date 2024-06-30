[[Альтернативные связи]]

- Для каждого типа из иерархии выделяется отдельна таблица
```cs
namespace Data.Entities.TPT;  
  
public abstract class Building  
{  
    public int BuildingId { get; set; }  
    public int Square { get; set; }  
    public required string Name { get; set; }  
}  
  
public class House:Building  
{  
    public required string FlatAddress { get; set; }  
}  
public class Factory : Building  
{  
    public int CountMetalPerDay { get; set; }  
    public required string Contract { get; set; }  
}
```
- Настройка
```cs
public DbSet<Building> Buildings { get; set; } = null!;
...
modelBuilder.Entity<House>()  
    .ToTable(nameof(House));  
modelBuilder.Entity<Factory>()  
    .ToTable(nameof(Factory));
```
- Что в итоге(3 таблицы)

![[tpt1.png]]
![[tpt2.png]]
![[tpt3.png]]

- Обращение к типам
```cs
var factory = await context.Buildings.OfType<Factory>()  
    .FirstOrDefaultAsync(stoppingToken);
```