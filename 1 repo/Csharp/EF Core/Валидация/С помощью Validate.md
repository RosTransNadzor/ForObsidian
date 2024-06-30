[[Csharp/EF Core/Валидация/Валидация|Валидация]]

- Для валидации таким способом необходимо реализовать интерфейс
```cs
public class LineItem : IValidatableObject {
	// при валидации это тоже будет добавлено в массив ошибок
	[Range(1,5, ErrorMessage = "This order is over the limit of 5 books.")] 
	public byte LineNum { get; set; }
	// ...
	// метод Validate - возвращает cписок ValidationResult
	// это массив ошибок
	// ValidationResult - принимает (string?)сообщение об ошибке и (Enumerable<string?>)список с тэгами об ошибке(необязательно)
	public IEnumerable<ValidationResult> Validate(ValidationContext validationContext)
	{
		var currContext = validationContext.GetService(typeof(DbContext)); 
		if (ChosenBook.Price < 0) 
			// yield для добавления в массив ошибок
			yield return new ValidationResult( $"Sorry, the book '{ChosenBook.Title}' is not for sale."); 
		if (NumBooks > 100) 
			yield return new ValidationResult( "If you want to order a 100 or more books"+ " please phone us on 01234-5678-90", new[] { nameof(NumBooks) });	
	}
}
```
- Сам процесс валидации
```cs
// объект класса для валидации,
var entity = entry.Entity; var valProvider = new 
// этот тип реализует интерфес IServiceProvider для ValidationContext
ValidationDbContextServiceProvider(context); 

// хранит информию о необходимую для валидации
var valContext = new ValidationContext(entity, valProvider, null);
var entityErrors = new List();

// принимает сущность контекс и пустой список ошибок для его заполнения
if(!Validator.TryValidateObject( entity, valContext, entityErrors,true)){
	возвращает true - если все прошло успешно
}
```