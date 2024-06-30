[[Fluent Api Для связей]]

- Список возможных случаев
![[удаление.png]]

- Client Set Null - когда нав ключ - к примеру int? - необязательный
- Сlient Cascade - когда обязательный int - к примеру
- Set Null и Cascade - необходимо задавать вручную
```cs
// Задание удаления - установка null при удалении основной связи для зависимой
modelBuilder.Entity<Book>()  
    .HasOne(b => b.Promotion)  
    .WithOne()  
    .OnDelete(DeleteBehavior.SetNull);
```
- Отличие Client и Просто в  том что при Client необходимо загружать навигационное свойство - зависимую сущность
- Однако рекомендуется использовать версию ClientSetNull и ClientCascade
```cs
// не загружаем доп свойства
var book = await context.Books.SingleOrDefaultAsync(b => b.BookId == id);  
context.Books.Remove(book);  
  
await context.SaveChangesAsync(stoppingToken);
```