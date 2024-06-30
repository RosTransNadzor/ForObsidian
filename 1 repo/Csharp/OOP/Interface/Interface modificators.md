[[OOP]]
- Здесь речь пойдет про особенности модификоторов доступа у интерфейсов
<span style="color:yellow"></span>
#### Начальный раздел 
- Тело по умолчанию - у членов методов определенных в интерфейсах могут быть реализации по умолчанию.
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
- Явная реализация 
```cs
interface IA
{
    void M() { WriteLine("IA.M"); }
}
class B : IA
{
    void IA.M() { WriteLine("IB.M"); } // Explicit implementation
}
B b = new B();
IA ia = b;
b.M(); // нет доступа
ia.M(); // ест доступ
```
#### Общая информация

- По умолчанию все члены интерфейса определены с ключевым словом <span style="color:yellow">public</span>
- они доступны в явном виде интерфейса и в классе

```cs
public interface ISome
{
	// public - по умолчанию
    void Me();
    // одно и тоже
    public Me();
}
public class Ral: ISome{
	// здесь обязан быть public
	public void Me(){...}
}
// ...
Ral ral = new Ral();
ISome isome = ral;
// доступ есть
ral.Me();
// и здесь тоже
isome.Me();
```
- Реализация по умолчанию используется в модификаторе<span style="color:yellow"> private</span> у интерфейсов(обязательно должно быть тело)
- эти методы не видны ни в классе реализующем данный интерфейс ни при явном приведении к интерфейсу
- их можно применять для базовой реализации другого метода интерфейса
```cs
public interface ISome
{
	// private - необходимо тело
    private void Do(){};
    // вызов в явной реализации другого метода
    public void Act(){Do()}
}
public class Ral: ISome{
}
// ...
Ral ral = new Ral();
ISome isome = ral;
// доступа нету
ral.Do();
// и здесь тоже
isome.Do();
// Do используется только в Act
ral.Act()
```
- Модификатор <span style="color:yellow">protected</span> - не виден снаружи интерфейса но не виден внутри(собственно что и есть protected) + наследуется, у класса виден везде т к  реализуется с помощью <span style="color:yellow">public</span>
```cs
public interface ISome
{
    protected void Fork();

    public void Any()
    {
	    // виден внутри
        Fork();
    }
}
public class Ral:ISome
{
	// внутри
    public void Fork()
    {
        Console.WriteLine("fork in class");
    }
}
Ral ral = new Ral();
ISome isome = ral;
// виден снаружи
ral.Fork();
// не виден снаружи
isome.Fork();
```
- При явной реализации работает как <span style="color:yellow">private</span>, но с возможностью переопределения в реализующем типе и необязательной реализацией по умолчанию вследствие этого
```cs
public interface ISome
{
	// тело не обязательно
    protected void Fork();
	// Fork - доступен только внутри реализаций по умолчанию других методов
    public void Any()
    {
        Fork();
    }
}
public class Ral:ISome
{
	// определние Fork
    void ISome.Fork()
    {
        Console.WriteLine("fork in class");
    }
}
// доступа нету
ral.Fork();
// и здесь тоже
isome.Fork();
// Fork используется в Act
ral.Any()
```
- Переабстрагирование (reabstraction) - 
```cs
interface IA
{
    void M() { WriteLine("IA.M"); }
}
interface IB : IA
{
    abstract void IA.M();
}
class C : IB { }
```