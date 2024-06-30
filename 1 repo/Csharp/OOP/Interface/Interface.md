[[Base]]

##### !!! При реализации интерфейса все члены интерфейса должны быть открытыми
```cs
public interface IAbstact
{
	void Main()
}
public abstract class Parent : IAbstact
{
	// этотчлен обязан быть открытым
	public abstract void Main();
}
```
- Но можно явно указать private
```cs
public interface IAction{
	void Move();
}
class HeroAction : IAction
{
	// своя новая реализация метода
    public new void Move() => Console.WriteLine("Move in HeroAction");
    // явная реализация интерфейса(private)
    void IAction.Move() => Console.WriteLine("Move in IAction");
    
    public void Some(){
		// доступ есть только к новой реализации
	    Move();
    }
}
var hero = new HeroAction();
hero.Move();
hero.Some();
// доступ к этому методу есть только с помощью приведения типов
var parent = (IAbstact)new HeroAction();
parent.Main();
```
- Реализация по умолчанию
```cs
interface IA { 
	void M() 
	{ 
		WriteLine("IA.M"); 
	} 
}
// можно не реализовывать интерфейс - будет использована реализация по умолчанию
class NonRealize : IA{}
```