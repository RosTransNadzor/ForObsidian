[[Entity Framework]]

- Набор методов для настройки свойств таблицы
```cs
protected override void OnModelCreating(ModelBuilder modelBuilder)  
{  
    modelBuilder.Entity<BookAuthor>()  
	    //cоздание составного ключа связующей таблицы(для связи многие ко многим)
        .HasKey(x => new { x.BookId, x.AuthorId });  
    modelBuilder.Entity<Book>()  
        .Property(b => b.PublishedOn)  
        .HasColumnType("date");  
    modelBuilder.Entity<Book>()  
        .Property(b => b.Price) 
        // пишется тип который называется в конкретной базе (здась Mysql) 
        .HasColumnType("decimal(10,2)");  
  
}
```
- Создание уникальных полей(должен быть индексом)
```cs
modelBuilder.Entity<Author>()  
    .HasIndex(a => a.Name)  
    .IsUnique(true);
```