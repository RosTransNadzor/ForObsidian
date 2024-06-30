[[Entity Framework]]

- Есть три подхода к настройке Ef core 
	- По соглашению (имена свойств и создание навигационных ссылок - автоматич для EF)
	- [[Csharp/EF Core/Настройка/Fluent API/Fluent Api|Fluent Api]] - прямая настройка в OnModelCreating()
	- [[Csharp/EF Core/Валидация/Data Annotations|Data Annotations]] - с помощью аттрибутов
	- 
	
![[настройка.png]]

- По соглашению
	- Большинство типов по умолчанию не имеют null в базе данных,но с помощью ? (int?)
	- можно задать в базе тип null - yes