[[Entity Framework]]

- Для сложных запросов нам нужно выбирать только необходимые данные для оптимизации запроса
- Для этого используются DTO - классы хранящие частичные данные
```cs
// хранит толко необходимые данные
public class BookDTO{
	public int BookId { get; set; }
	public string Title { get; set; }
	public int ReviewsCount { get; set; }
	// double? - для average() вычисления в базе данных(уcтарело?)
	public double? ReviewAverageVote { get; set; }
	// ...
}
```
- Метод расширения для обработки запроса внутри его
#### <span style="color:red">Не забыть загрузить на свойства</span>
```cs
// принимается IQueryable<T> и возвращает тот же IQueryablr<T>
public static IQueryable<BookDTO> MapToBookDTO(this IQueryable<Book> books)
{
	return books.Select(book => new BookDTO
	{
		BookId = book.BookId,
		Title = book.Title,
		PublishedOn = book.PublishedOn,
		Price = book.Price,
		ActualPrice = book.Promotion == null ? book.Price : book.Promotion.NewPrice,
		PromotionPromotionalText = book.Promotion == null
			? null
			: book.Promotion.PromotionalText,
		ReviewsCount = book.Reviews.Count(),
		// используя double? - данные обрабатываются в базе(устарело?)
		ReviewAverageVote = book.Reviews.Average(rew => (double?) rew.Stars),
		TagStrings = book.Tags.Select(tag => tag.Name).ToArray(),
		AuthorsOrdered = string.Join(
			" ",
		    book.AuthorsLink.OrderBy(al => al.Order).Select(ba => ba.Author.Name)
		)
	})
}
```
- Использование запросов
```cs
// нужно загрузить не только Tags
var book = context.Books.Include(t => t.Tags).MapToBookDTO()
```