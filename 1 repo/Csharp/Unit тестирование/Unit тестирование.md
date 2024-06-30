[[Csharp]]

- используется фреймворк xUnit
- Пример (проект в rider)
```cs
public class UnitTest1  
{  
	// Fact - помечает метод для тестирования
    [Fact]  
    public void TestDivideSuccess()  
    {        double num1 = 3;  
        double num2 = 6;  
        Some testable = new Some();  
  
        double result = testable.Divide(num1, num2,1);  
        // помечает проверку 
        Assert.Equal(0.5,result);  
        Assert.Equal(1,0);  
    }  
    [Fact]  
    public void TestDivideFailure()  
    {        double num1 = 1;  
        double num2 = 0;  
        Some testable = new Some();  
        Action divide = () => { testable.Divide(num1, num2, 0);};  
  
        Assert.Throws<ArgumentException>(divide);  
    }}
```
- В тесте могут быть несколько проверок(лучше одна) - если одна из неверна весь тест отвалится
- У класса  кроме equals много других методов для проверки данных (Throws,Null,All и т.д.)
