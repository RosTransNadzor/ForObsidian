[[Base]]

- Представляют собой неизменяемый тип
- ключевое слово init - для свойства позволяет его задать в конструкторе или синтаксисом инициализации и нигде больше - тольк для чтения
```cs
public record Person
    {
	    // init свойство
        public required string Name { get; init; }
        public required string LastName { get; init; }
        // не является init - можно изменять
        public int Age { get; set; }
		// позволяет превращать record в tuple
        public void Deconstruct(out string name,out string lastname,out int age)
        {
            (name, lastname,age) = (Name, LastName,Age);
        }
        public void Deconstruct(out string name,out string lastname)
        {
            (name, lastname) = (Name, LastName);
        }
}
public static void Main(string[] args)
    {
        Person p = new Person() { Name = "me", LastName = "fres" };
        // использование deconstruct
        (string n, string b) = p;
        // tostring выведет все открытые свойства
        Console.WriteLine(p.ToString());
    }
или так
// все переменные в конструкторе являюстя init 
    private record Person(string LastName,string Name,int Age)
    {
        public void Deconstruct(out string name,out string lastname,out int age)
        {
            (name, lastname,age) = (Name, LastName,Age);
        }
        public void Deconstruct(out string name,out string lastname)
        {
            (name, lastname) = (Name, LastName);
        }
    }
    public static void Main(string[] args)
    {
        Person p = new Person(Name: "me",LastName:"fres",Age:5);
        (string n, string b) = p;
        Console.WriteLine(p.ToString());
    }
```
- Record все еще ссылочный тип 
```cs
Person p = new Person() { Name = "me", LastName = "fres" };
Person p2 = p with {};
 Console.WriteLine(p == p2);// сравнивает поля
        
Person p3 = new Person() { Name = "me", LastName = "fres" };
Person p4 = p3 with {};
p4.Age = 23; // изменение не init поля
Console.WriteLine(p3 == p4); // false
        
Person p5 = new Person() { Name = "me", LastName = "fres" };
Person p6 = p5;
// изменение по ссылке
p6.Age = 23;
Console.WriteLine(p5.Age); // 23
```
- Существуют также record struct и record readonly struct
- record record struct - лучше использовать - только неименяемая
- record struct - изменяемая (непонятно почему)