- Позволяет с помощью последовательных  вызовов плавно изменять объект
```cs
var model = new Model();
model.Entity("car").WithDriver("some").WithPrice(100).Build();
```
- Для этого все методы данного класса должны возвращать this
- Но для правильной последовательности выполнения этих методов необходимо добавлять интерфейсы - это будет реализация нескольких стадий обработки
- с помощью приведения типов
```cs
var model = Model.CreateModel().Entity("some").WithDriver("e").WithPrice(100).Build();  
  
Console.WriteLine(model.Price);  
  
public class Model :IEntityStage ,IDriverStage ,IPriceStage ,IBuilderStage  
{  
    private string entity;  
    private string driver;  
    private int price;  
    private bool isBuilded = false;  
    public int Price => price; 
    // закрытый конструктор для защиты от прямого создания 
    private Model(){}  
	// этот стат метод служит для создания объекта на первом этапе
    public static IEntityStage CreateModel()  
    {        return new Model();  
    }    
    public IPriceStage WithDriver(string name){  
        driver = name;  
        return this;  
    }    public IBuilderStage WithPrice(int newprice)  
    {        price = newprice;  
        return this;  
    }    public Model Build(){  
        isBuilded = true;  
        return this;  
    }  
    public IDriverStage Entity(string name)  
    {        entity = name;  
        return this;  
    }}  
public class Some{}  
public interface IEntityStage  
{  
    public IDriverStage Entity(string name);  
}  
public interface IDriverStage  
{  
    public IPriceStage WithDriver(string name);  
}  
public interface IPriceStage  
{  
    public IBuilderStage WithPrice(int price);  
}  
public interface IBuilderStage{  
    public Model Build();  
}
```