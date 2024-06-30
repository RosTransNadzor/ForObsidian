[[Csharp]]

- Статические лябда выражения говорят компилятору,что лямбда не может захватить локальные переменные и состояния экземпляра лямбда-выражения (проверяется на этапе компиляции (вроде)):
- Они могут обращаться только к константам или  к статическим членам
- Пример 
	```cs
	static int Value{get;set;}
	static void Main(string[] args)  
	{  
	    Another an = new Another();  
	    Rem(static num =>  
	    {  
		    // нельщзя обращаться к локальным переменным an.s.GetValue();  
	        Value = 5;  
	        Console.WriteLine(Value);  
	    });
	}  
	public static void Rem(Action<int> action)  
	{  }
	```