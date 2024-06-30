[[Entity Framework]]

- Создание контекста через using
```cs
using (var db = new AppDbContext())
{
  // ... логика
}
```
- Обращение к конкретному DbSet (таблице) 

```cs
db.Books
```
Book из [[Модели базы данных]] и DbSet из [[DbContext]]
- Получение данных только для чтения (делает запрос быстрее)
```cs
db.Books.AsNoTracking()
```
- Получение навигационного свойства [[Модели базы данных]]
```cs
db.Books
   .AsNoTracking()
   // Author - навигационное св-во
   .Include(book => book.Author)
```
- Удаление записей
```cs
// someBook - какой-то экземпляр класса Book
db.Books.Remove(someBook)
```
- Изменение записей
```cs
db.Books[0].Name = "Fred Martin"
```
- Сохранение  изменений
```cs
db.SaveChanges();
// или асинхронно
await db.SaveChangesAsync();
```