[[Настройка связей]]
- Это свойство используется при множественном нав свойстве одной сущности
```cs
public class LibraryBook { 
	public int LibraryBookId { get; set; }
	public string Title { get; set; } 
	// 1 Person - лицо отдавшеее книгу
	public int LibrarianPersonId { get; set; } 
	public Person Librarian { get; set; } 
	// 2 Person - лицо взявшее книгу
	public int? OnLoanToPersonId { get; set; } 
	public Person OnLoanTo { get; set; } 
}
// для Ef core -необходимо указать что к чему относится
public class Person { 
	public int PersonId { get; set; }
	public string Name { get; set; } 
	// указание на Librarian из Library Book
	[InverseProperty("Librarian")] 
	public ICollection LibrarianBooks { get; set; }
	[InverseProperty("OnLoanTo")] 
	public ICollection BooksBorrowedByMe { get; set; } }
```