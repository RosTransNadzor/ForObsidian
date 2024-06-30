[[Альтернативные связи]]

- При подходе «Таблица на иерархию» (TPH) все классы, наследуемые друг от друга, хранятся в одной таблице базы данных.
```cs
// главный класс
public abstract class Animal  
{  
    public int AnimalId { get; set; }  
    public required string Name { get; set; }  
    public AnimalTypes AnimalType { get; set; }  
}  
// наследуется от animal
public class Dog:Animal  
{  
    public int? PersonId { get; set; }  
    public Person Owner { get; set; } = null!;  
    public required string Breed { get; set; } = null!;  
}  
  
public class Cat : Animal  
{  
    public int? NumOfKittens { get; set; }  
}  
// вспомогательное перечисление для определения типа в таблице
public enum AnimalTypes  
{  
    Dog,  
    Cat  
}
```
- Эти 2 типа будут отражены в одной таблице
![[описаниеживотных.png]]
![[данныеживотных.png]]

- Конфигурация 
```cs
public DbSet<Animal> Animals { get; set; } = null!;
...
modelBuilder.Entity<Animal>()  
	//использование перечисления для определения типа
    .HasDiscriminator(a => a.AnimalType)  
    .HasValue<Dog>(AnimalTypes.Dog)  
    .HasValue<Cat>(AnimalTypes.Cat);
```
- Работа с иерархическими данными
```cs
//создание
Dog dog = new Dog()  
{  
    Name = "some",  
    PersonId = 2,  
    Breed = "hasky"  
};  
context.Animals.Add(dog);  

await context.SaveChangesAsync();
// получение данных определенного типа и дальнейшая работа с ними
Cat? kitty = await context.Animals.OfType<Cat>().FirstOrDefaultAsync();
```
