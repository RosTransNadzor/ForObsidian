[[Entity Framework]]

```sql
-- Получение версии MySql (8,0,33)
SELECT VERSION()
```
Пароль от базы данных MySql
```

3icReiDEvn@34

```

3icReiDEvn@34
- Пароль от PostgreSql
```cs
Celeste123
port - 5432
```
Этот класс используется для настройки базы данных и взаимодействия с ней
```cs
public class AppContext : DbContext
	{
		private readonly string connectionString =
	@"server=localhost;user=root;password=3icReiDEvn@34;database=booksShopDB;";
		protected override void OnConfiguring(DbContextOptionsBuilder ptionsBuilder)
		{
			optionsBuilder
			   .UseMySql(
				  connectionString,
				  new MySqlServerVersion(new Version(8,0,33))
			);
		}
		DbSet<Book> Books { get; set; }
	}
```
- DbSet - Таблица состоящая из книг(просто проекция базы данных)
#### !!! Необязательно все таблицы хранить в DbSet екоторые можно опустить и доступ к ним будет осуществляться через навигационные св-ва
```cs
// получаем таблицу которой нет в DbSet в DbContext
context.Set<BookAuthor>()
```