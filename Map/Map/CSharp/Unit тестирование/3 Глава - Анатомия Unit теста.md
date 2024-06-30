[[Unit Тестирование]]

<span style="color:HotPink;font-weight:bold;">Паттерн  AAA</span>

- Избегать множественный секций arrang,act,assert
	- Допустимо иметь несколько таких секций только в интеграционных тестах
- Секция подготовки - самая большая
	- Лучше выделять отдельные операции подготовки в фабрику ,чем в методе Setup
- Секция действия 
	- Обычно состоит из одной строки кода, если это не так, то какие-то проблемы с API системы
- Несколько проверок в одном тесте
	- Это нормально ,так как одна единица поведения может приводить к нескольким результатам
- Комментарии к секциям
	- Лучше каждую секцию помечать комментариями
	- //Arrange //Act //Assert
- C# и xUnit
	 - Вместо SetUp и TearDown исползуется подход по соглашению - в конструкторе тесты конфигурируются, а в методе Dispose (IDisposable) можно делать очистку (Эти методы выполняются для каждого теста)
	 - Сами тесты помечаются аттрибутом [Fact]
- Правильное выделение логики инициализации
	- Правильным способом выделения логики инициализации - использование фабричных методов,а не инициализация в конструкторе (выполнение секции arrange)
- Параметризированные тесты
	- в xUnit используется аттрибут [InlineData]  и [Theory]
		```cs
			public class DeliveryServiceTests { 
			[InlineData(-1, false)] 
			[InlineData(0, false)] 
			[InlineData(1, false)] 
			[InlineData(2, true)]  
			[Theory] public void Can_detect_an_invalid_delivery_date
				( int daysFromNow, bool expected)
			{ 
				DeliveryService sut = new DeliveryService(); 
				DateTime deliveryDate = DateTime.Now.AddDays(daysFromNow);
				Delivery delivery = new Delivery { Date = deliveryDate };
				bool isValid = sut.IsDeliveryValid(delivery);
				Assert.Equal(expected, isValid); Использование этих параметров } 
			}
		```
	- В С# у аттрибутов могут использоваться только константные значения
	- Для обхода этой проблемы в xUnit существует [MemberData] 
		```cs
		[Theory] 
		[MemberData(nameof(Data))]
		 public void Can_detect_an_invalid_delivery_date
			 ( DateTime deliveryDate, bool expected) 
		{ /* ... */ } 
		public static List Data() { 
			return new List { 
				new object[] { DateTime.Now.AddDays(-1), false }, 
				new object[] { DateTime.Now, false }, 
				new object[] { DateTime.Now.AddDays(1), false }, 
				new object[] { DateTime.Now.AddDays(2), true }
			 }; 
		 }
		```
- Библиотека Fluent Assertions
	- Помогает проще писать секцию проверки
	- Например 
		```cs
		result.Should().Be(30);
		// вместо
		Assert.True(result == 30);
		```