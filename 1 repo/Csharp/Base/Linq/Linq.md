[[Base]]

- Single - возвращает найденный элемент и выбрасывает исключение если этого элемента нет или таких элементов >1
```cs
db.Books
  .Include(book => book.Author)
  .Single(book => book.Title == "Quantum Networking");
```
- SingleorDefault - не выбрасывает исключение если не найден элемент а значение по умолчанию (может null), если >1 выбрасывает исключение
